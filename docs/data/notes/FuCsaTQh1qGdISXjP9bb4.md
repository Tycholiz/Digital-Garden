
## Setup
1. Create a Javascript Debug Terminal (from command palette)
2. Run command to start servers (ie. `yarn start`)
    - command will take longer than normal to run, because vscode is attaching debuggers to all the processes that are about to run.
3. Set some breakpoints of where we want the code to stop
4. Manually cause the code to be executed
    - ex. by visiting a url, clicking a button in UI etc.

## Analysis
- We can see what each variable evaluates to in the main window

### Debug panel
- In the debugger left panel, we can see the variables

### Debug Console
- We can open the Debug Console to get a Debug Console REPL (cmd+shift+y)

* * *

the debugger will not work for client-side code, since this code is executed in a browser. We would need to use the browser debugging tool to accomplish this.
- alternatively, we can have a `launch.json` file to allow us to debug

# Chrome Debugger
- The vscode debugger can connect to Chrome via its Chrome Debugger protocol. Doing this allows us to map files loaded in the browser to the files open in Visual Studio Code.
    - This enables developers to debug in vscode, by...
        - setting breakpoints directly in their source code.
        - setting up variables to watch and see the full call stack when debugging
