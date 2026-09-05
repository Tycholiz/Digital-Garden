
# Overview
- provides MVC in the client-side
- opinionated— follows convention over configuration
- modules import other modules implicitly because they know exactly where to look. Therefore, following the conventional structure is paramount (like Ruby on Rails) 
	- ex. data that's fetched in the `/routes/index.js` will be available in `/templates/index.hbs` as `@model`

![](/assets/images/2021-03-10-22-39-10.png)

# Decorators
- they are special functions that make modifications to the following line
- they can be thought of as wrapper functions in a sense, if they are before a function.
- ex. this decorator will cause the following getter to cache the result on the first time.

## Types
- `@tracked` - in a class component, This annotation on a variable tells Ember to monitor this variable for updates. Whenever this variable's value changes, Ember will automatically re-render any templates that depend on its value.
- `@action` - in a class component, define a function that is available to the component layout (handlebars html)
```js
@cache
get count() {
  return this._count++;
}
```
- can receive arguments: `@alias('fullName') name`

# Anatomy of Ember app
![](/assets/images/2021-03-10-22-39-28.png)
- url determines the current state of the app
	- ex. are they looking at a list? a post? are they editing the post?
- When the url is entered in the address bar, it...
	- connects to a route handler which loads a particular model.
	- It renders a template, which has access to the model.

## Models
- Models represent persistent state. 
	- ex. in an airbnb app, the details of a rental (price, description, photos etc) would be stored in the `rental` model. we'd also have a `user` model to store the state of the currently logged in user
- models persist information, whether it's to a web server or as local state
- model layers can be swapped in, so we could use Redux or Apollo

## Templates
- similar to `ejs`
- the route handler makes the data of the model available to the template

### application.hbs
- a wrapper around the whole application. This is where we can specify a footer and header, since they will appear on all pages.
- use `{{outlet}}` to specify the whole application (think of it as `children`, which contains the rest of the app)

## Components
- essentially just a template that goes in another template
- they can take args (just like passing props in React)
![](/assets/images/2021-03-10-22-39-41.png)
- You can think of components as Ember's way for letting you create your own HTML tags.
- use the `{{yield}}` keyword to pass "the rest of the data" (similar to `children` in React)

### Namespaced components
- you can have a component that exists within another component by:
	a. creating a folder within `/components` with `ember generate component <parentDir>/<subComponent>`
- invoked in templates like this `<Parent::Child>`
- We can pass down HTML attributes just like props in react:
```html
<div class="image">
  <img ...attributes>
</div>
```

### Class components
- enable us to add behavior to components (by using states)
- run `ember generate component-class <nameOfComponent>
- class components extend `Glimmer` components, giving it functionality similar to classes in React, such as state and lifecycle methods.
- whenever a component is invoked, an instance of its related class component will be instantiated, allowing us to store state in it and call any relevant lifecycle methods.
- initial state is stored in the constructor (in Ember, writing out constructor seems to be optional)
- in the template part of the component (ie. the html) we get access to the instance variables (the component state) defined in the class component
- Glimmer components have access to `this.args`, which is just like `this.props`
	- All arguments that can be accessed from this.args are implicitly marked as @tracked

#### Block parameters
- a block is any code that is between the opening and closing tags of a component (in react it would be called `children`)
- What if there is a variable that we want to pass to the block content?
	- In this case we can the `as |results|` syntax, which would make `results` available to everything inside the block
	- this is similar to when we do a `items.map(item)`, and we have the current iteration available to us as `map`
```html
  <ul class="results">
    <Rentals::Filter @rentals={{@rentals}} @query={{this.query}} as |results|>
      {{#each @rentals as |rental|}}
		<li>
		  <Rental @rental={{rental}} />
		</li>
      {{/each}}
    </Rentals::Filter>
  </ul>
```
- this also allows us to pass down the resultings data in the corresponding module that pertains to `rentals/filter.hbs` with `{{yield this.results}}` (see next section)

#### Provider component
- A pattern we use when we want to set up a piece of state for a component, but don't have any html to render for it. Instead, the html is just passed on down to the next level by using `{{yield this.results}}`
- The child component then passes data up to it's parent
	- look at `rentals.hbs` and `rentals/filter.js`. 
		- `@query={{this.query}} as |results|` passes the `query` variable down to the child, giving it access to it. The child (a class component) uses that variable to make computations, then returns a result, which gets put in the variable `results` (due to the `|results|` line above)

## Routes
### Model hook
- The model hook is responsible for fetching and preparing any data that you need for your route. Ember will automatically call this hook when entering a route, so that you can have an opportunity to run your own code to get the data you need. The object returned from this hook is known as the model for the route.
	- Usually, this is where we'd fetch data from a server. Since fetching data is usually an asynchronous operation, the model hook is marked as async
	
## Services 
- Services are just singleton objects (ie. they get instantiated only once) to hold long-lived data such as user sessions.
- serve a similar role to global variables, in that they can be easily accessed by any part of your app
- For example, we can inject any available service into components, as opposed to having them passed in as an argument. This allows deeply nested components to "skip through" the layers and access things that are logically global to the entire app, such as routing, authentication, user sessions, user preferences, etc.
- A major difference between services and global variables is that services are scoped to your app, instead of all the JavaScript code that is running on the same page. This allows you to have multiple scripts running on the same page without interfering with each other.

### Store service
- can be injected into a route with `@service store`, making the Ember Data store available to use as `this.store`, and giving us `find` and `findAll` methods.
```ts
import { inject as service } from '@ember/service';

export default class IndexRoute extends Route {
  @service store;
  async model() {
    return this.store.findAll('rental');
  }
}
```
- store service might be compared to Redux in its role to fetch from the database and cache it

## Controller
- def - an object that receives the return value of the `model()` method (which is found in the associated route).
- def - an object that receives one property when its associated route is hit: `model` (the return value of the Route's model method)
- controller is only needed if we want to customize the properties or provide actions to the Route
	- in other words, they are an extension of the model loaded from the Route
- if we don't make a `controller` file, one is generated for us by default (we just don't see it)
- the controller name must match the route that renders it
- controller is a singleton (ie. they get instantiated only once) 
	- this means we shouldn't keep state in the controller 
		- (unless it comes from either the Model or the Query params; since these would persist in between activations such as when a user leaves the Route and then re-enters it)
- Controllers can also contain actions, Query Parameters, Tracked Properties, and more
- Basically, use controllers when: 
	1. we want to pass down actions or variables to the components found in a route. 
	2. we want to support query params
	3. we want to compute a value (that we will ultimately pass down to the route's components) that depends on the model hook 
		- in other words, the controller takes in the result of `model()` as its sole argument. What if we want to pass a variable down to the components that depend on the return value of `model()`?

# Libraries
## Ember Data
- a library that helps manage data and application state in Ember applications.
- built around the idea of organizing your app's data into model objects (in `/models` directory).
	- These objects represent units of information that our application presents to the user
- The model represents the shape of the data

```js
import Model, { attr } from '@ember-data/model';

const COMMUNITY_CATEGORIES = [
  'Condo',
  'Townhouse',
  'Apartment'
];

export default class RentalModel extends Model {
  @attr title;
  @attr owner;
  @attr city;
  @attr location;
  @attr category;
  @attr image;
  @attr bedrooms;
  @attr description;

  get type() {
    if (COMMUNITY_CATEGORIES.includes(this.category)) {
      return 'Community';
    } else {
      return 'Standalone';
    }
  }
}
```
- Ember Data uses Adapters and Serializers. The idea is that, provided that your backend exposes a consistent protocol and interchange format to access its data, we can write a single adapter-serializer pair to handle all data fetches for the entire application.
	- Adapters deal with how and where Ember Data should fetch data from your servers, such as whether to use HTTP, HTTPS, WebSockets or local storage, as well as the URLs, headers and parameters to use for these requests. 
	- Serializers are in charge of converting the data returned by the server into a format Ember Data can understand.

# Structure
- the root of the ember application is `templates/application.hbs`

# Tests
- integration tests - components that exist in isolation. They don't have to interact with the context in which they are placed. They can exist as a unit. Essentualy these are our unit tests.
- acceptance tests - components that need to interact with other areas of the app (ex. navbar link functionality)

# Mirage
## Factories
- allow you to create blueprints for your data. In other words, seed your development database
