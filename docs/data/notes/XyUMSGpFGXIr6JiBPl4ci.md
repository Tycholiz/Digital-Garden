
Hooks are a type of function that allows your system to call some other module.
- It is a broad way to describe this pattern where some piece of code (the hook) can trigger in response to data being sent between software components.

- ex. keylogging
	- Basically, our application code says "ok System, I want to be notified anytime anytime a user presses a key"
- ex. Every Chrome (or other browser) extension is possible because of hooks in the browser.
	- The extension sets up a hook to be notified when a specific page is navigated to.
	- Vimium executes hooks in response to keypresses.
- ex. Video games with mod support are another example of hooks in action.
- ex. a browser might use a hook to make it easier for antivirus software to scan downloads.
	- When it starts a download, it shouts "Hey, anyone listening, I'm starting a download!", and any program listening for that will be able to react and intervene.
- A screen reader for blind users sets up a system hook to know when another program creates a new window on the screen.

### James Bond paintball mode example

If you were to try and implement this mod to the game, you could use a hook provided by the game itself. Essentially, when an event happens in the game (ie. when bullets are shot), a hook is executed, which turns the resulting bullet holes colored.

![](/assets/images/2021-11-23-14-35-06.png)
