
## MVC
Even with the so-called MVC design pattern, there is some variation between the traditional MVC pattern and its modern interpretation in various programming languages. For example, some MVC–based frameworks will have the view observe the changes in the models, while others will let the controller handle the view update.

### Model
- Models represent knowledge. A model could be a single object (rather uninteresting), or it could be some structure of objects.
    - There should be a one-to-one correspondence between the model and its parts on the one hand, and the represented world as perceived by the owner of the model on the other hand.
- The classes which are used to store and manipulate state, typically in a database of some kind.
- The model retrieves and populates the data.
- When a model changes, typically it will notify its observers that a change has occurred.

### View
- A view is a visual representation of its model. 
    - It highlights certain attributes of the model and suppress others. It is thus acting as a presentation filter.

### Controller
A controller is the link between a user and the system, or the glue between the view and the model. 
- The controller updates the view when the model changes. It also adds event listeners to the view and updates the model when the user manipulates the view.

It provides the user with input by arranging for relevant views to present themselves in appropriate places on the screen. It provides means for user output by presenting the user with menus or other means of giving commands and data. The controller receives such user output, translates it into the appropriate messages and pass these messages on to one or more of the views.
- The brains of the application. The controller decides what the user's input was, how the model needs to change as a result of that input, and which resulting view should be used.
- job is to provide a bit of orchestration between Models and Views
