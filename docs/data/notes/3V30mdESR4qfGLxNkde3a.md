
note: Documentation for Javascript as an Osascript language is pretty bad. It should be avoided. If possible, stick with Applescript.

## CLI
### Run script in terminal
```sh
osascript -l JavaScript myScript.scpt
```

### See available languages
```sh
osalang -l
```

### Start/Exit application
```js
const itunes = Application('iTunes');
itunes.activate();
// Play a song.
itunes.play();
itunes.quit();
```

### Get current application that's running the script
```js
var app = Application.currentApplication();
```

### Get topmost application
```js
// Get the name of the current application.
var system = Application("System Events");

var proc = system.processes.whose({ frontmost: {'=': true } }).name();
```

# Resources
[docs(ish)](https://developer.apple.com/library/archive/releasenotes/InterapplicationCommunication/RN-JavaScriptForAutomation/Articles/Introduction.html#//apple_ref/doc/uid/TP40014508)
https://www.macstories.net/tutorials/getting-started-with-javascript-for-automation-on-yosemite/
https://developer.apple.com/library/archive/documentation/LanguagesUtilities/Conceptual/MacAutomationScriptingGuide/index.html#//apple_ref/doc/uid/TP40016239-CH56-SW1
