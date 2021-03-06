
Note: do not use arrow functions in mongoose, since it prevents binding
# Hooks (a.k.a. *Middleware*)
Hooks are useful for atomizing model logic and avoiding nested blocks of asynchronous code.
Other use cases:
- complex validation
- removing dependent documents
    - (removing a user removes all his blogposts)
- asynchronous defaults
- asynchronous tasks that a certain action triggers
    - triggering custom events
    - notifications
## pre-hooks
There are two types of pre-hooks:
### Serial
The MW functions are executed one after another
- Note: calling `next()` does not immediately stop execution of the function. To do this, we would need to call `return`:
```
var schema = new Schema(..);
schema.pre('save', function(next) {
  if (foo()) {
    console.log('calling next!');
    // `return next();` will make sure the rest of this function doesn't run
    /*return*/ next();
  }
  // Unless you comment out the `return` above, 'after next' will print
  console.log('after next');
});
```
### Parallel
The hooked method does not get executed until `done` is called by each middleware:
```
var schema = new Schema(..);

// `true` means this is a parallel middleware. You **must** specify `true`
// as the second parameter if you want to use parallel middleware.
schema.pre('save', true, function(next, done) {
  // calling next kicks off the next middleware in parallel
  next();
  setTimeout(done, 100);
});
```
## post-hooks
Functions that are executed *after* the hooked method and all of its `pre` middleware have completed.

Do not directly receive control flow
- i.e. no `next` or `done` callbacks are passed to it

`post` hooks are a way to register traditional event listeners for these methods
- ex. when the `save` pre-hook happens, this function will execute:
```
schema.post('save', function(doc) {
  console.log('%s has been saved', doc._id);
});
```

## Types of middleware
### Document MW
`this` refers to the document itself

Hook methods:
- `init`
    - initialize a document without setters
    - Called internally after a document is returned from mongodb.
- `validate` (a.k.a. *pre-save*)
    - Executes registered validation rules for this document.
    - if a validation rule is violated, save is aborted
- `save`
    - Saves this document
    - calling this (as a pre-hook) will trigger `validate`, though `validate` will execute before `save`
- `remove`
    - Removes the document from database  
### Model MW
`this` refers to the model (schema)
### Query MW
`this` refers to the query
- `count`
- `find`
- `findOne`
- ...
Mongoose will not execute a query until `then` or `exec` has been called on it. The power of this comes when we want to build complex queries (ex. using `populate`/`aggregate`)
- Note: `.then()` in Mongoose are not actually `promises`. If you need a fully-fledged promise, use `.exec()`
```
User.find({ username }) // will not execute

// callback
User.find({ name: 'John' }, (err, res) => {}) // Will execute

// .then()
User.find({name: 'John'}).then(); // Will execute

Promise.all([User.find({name: 'John'}), User.find({name: 'Bob'})]) // Will execute all queries in parallel

// .exec()
User.find({name: 'John'}).exec(); // Will execute returning a promise
```
### Aggregate MW
Aggregate MW executes whe you call `exec()` on an aggregate object
`this` refers to the aggregation object (`<Model>.aggregate`)

If any middleware calls `next()` or `done()` with an argument of type `Error`, the flow is interrupted and the error is passed to the callback

# Populate
Lets us reference documents in other collections
- similar to JOIN in SQL dbs

*population* is the process of automatically replacing the specified paths in the document with document(s) from other collection(s).

In models, we give the `ref` option to a field (property within a document) to indicate which model to use during population.
```
const userSchema = new mongoose.Schema({
    username: String,
    posts: [{
        type: mongoose.Schema.Types.ObjectId, //an array of object ids
        ref: 'Post' //which model to use
    }]
})

const postSchema = new mongoose.Schema({
    content: String,
    author: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'User'
    }
})
```
Later, when we are within our controller, we will be using `.populate()` on the fields that have `type = mongoose.Schema.Types.ObjectId`:
```
User.findOne({ username: 'Kyle' })
    .populate('posts') //we want populate to work with the posts field in the user collection
    .exec((err, posts) => { // similar to .then()
        if (err) return err
        console.log('populated user:', posts)
    }
```
This query will return the specific document within the user collection where `username = 'Kyle'`, and it will *populate* the posts field with all posts made by 'Kyle':
```
{
    _id: 123,
    username: 'Kyle',
    posts: [
        "s8fm39m",
        "c83ncm8",
        "w822m02"
    ]
}
```
# Schemas
## Instance Methods
Here, instance refers to the document (since a document is an instance of a model)

Instance methods are defined like so:
```
// define a schema
var animalSchema = new Schema({ name: String, type: String });

// assign a function to the "methods" object of our animalSchema
animalSchema.methods.findSimilarTypes = function(cb) {
    return this.model('Animal').find({ type: this.type }, cb);
};
```
Now, all `animal` instances will have a `findSimilarTypes` method available on them:
```
var Animal = mongoose.model('Animal', animalSchema);
var dog = new Animal({ type: 'dog' });

dog.findSimilarTypes(function(err, dogs) {
    console.log(dogs); // woof
});
```
Imagine we were making a Medium.com clone. Our Article model would have a field called `claps`. We could define an instance method called `clap()`, which when executed, would increment the field `claps`
## Static Methods
Whereas instance methods are defined on the instance (document), static methods are defined on the Model itself.
```
//static method
const fido = await Animal.findByName('fido');

//instance method
const dogs = await fido.findSimilarTypes();
```
The previous two could not be swapped (ex. `Animals.findSimilarTypes()`), since it would not make sense. 
- Since `Animals` is a model and it has no `type`. This naturally would only exist on an instance of the model

## Query Helper
Instance methods for Mongoose queries:
```
animalSchema.query.byName = function(name) {
    return this.where({ name: new RegExp(name, 'i') });
};
var Animal = mongoose.model('Animal', animalSchema);

Animal.find().byName('fido').exec(function(err, animals) {
    console.log(animals);
});

Animal.findOne().byName('fido').exec(function(err, animal) {
    console.log(animal);
});
```
## Virtual
Lets us define `getters` and `setters` that don't get persisted to the database
- Imagine we need a variable `fullName`, but on the `User` model, we only store `first` and `last`. The naive way would be to concatenate these 2 variables each time. Instead, lets define a virtual so that we can use this "pseudo-field" in our application:
```
personSchema.virtual('fullname').get(function() {
    return `${this.first} ${this.last}`
}
```

### Accessing the parent documents from the child document
- ex. Tour has *1:many* relationship with reviews
- We can add a pre-hook MW function onto the tour model
```
parentSchema.virtual('reviews', { //the name of the virtual field
  ref: 'Review', //the child model
  foreignField: 'tour', //name of the field in the child model that contains the reference to the current (parent) model
  localField: '_id' //name of the field where the ID is stored on the current (parent) model.
});
```
Then, to actually populate this virtual field, we just have to use the `populate()` method within our `getTour` handler.

## Nested routes
- Having 2+ resources at the same url
- 

## Improving Read Performance with Indices
- see bottom of https://medium.com/@SigniorGratiano/modelling-data-and-advanced-mongoose-175cdbc68bb1
- this also allows us to carry out logic such as "each user can only review a tour one time" 
	- ` reviewSchema.index({ tour: 1, user: 1 }, { unique: true });`

# E Resources
[Data modeling and Referencing other collections](https://medium.com/@SigniorGratiano/modelling-data-and-advanced-mongoose-175cdbc68bb1)
