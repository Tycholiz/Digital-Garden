
# Actions
An action creator is found in the /actions directory. When they are called, an action is created. However, it doesn't do anything, it just floats there. The action becomes mobile only once it is dispatched. When store.dispatch is called with the action as its argument, the action is able to reduce the payload into the state
