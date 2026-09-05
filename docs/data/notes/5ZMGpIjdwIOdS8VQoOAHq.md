
Each item in a workflow is called an `Object`
- an input can be a `trigger`, `input`, `action`, `utility`, or `output`

Each workflow has its own folder, found in `Alfred.alfredpreferences/workflows/user.workflow.20D99320-DB8C-43F0-8128-B0981143AE62`
- To access this folder, click into a workflow object and look for this symbol in the resulting window.
- this is currently stored in Onedrive so multiple machines can sync the same Alfred config.
- We can store scripts in here. For instance, we could have a node script, and then call that script with a simple bash script (ScriptFilter workflow object):
	- `./recentdownloads 'added_reverse'`
		- the current directory here would be the workflow folder.

![](/assets/images/2021-11-30-22-10-28.png)

## Workflow Object types
- *Triggers*: Activate Alfred from a hotkey, another Alfred feature or an external source.
- *Inputs*: Keyword-based objects used to perform an action, on its own or followed by a query.
- *Actions*: The objects that do most of the work in your workflows; opening or revealing files and web searches, running scripts and performing commands.
- *Utilities*: Utilities give you control over how your objects are connected together and how the arguments output by the previous object is passed on to the next object.
- *Outputs*: Collect the information from the earlier objects in your workflow to pop up a Notification Centre message, show output in Large Type, copy to clipboard or run a script containing the result of your workflow.

### Placeholders
- `{query}` - the string that the user adds after the keyword.
- `{arg}` - the arg is what is passed from the previous object.

### Variables and Arguments Utility
We can modify arguments. For instance, `{query}` evaluates to whatever was typed in the Alfred launcher. We can modify this to be `- {query}`, for instance if we were creating a bulleted list.
- otherwise, we can just pass the argument through.

![](/assets/images/2021-11-29-21-58-21.png)

In this example, I've set two variables; The first variable named task will save the {query} input, allowing me to re-use it later in the workflow with {var:task}
![](/assets/images/2021-11-29-21-58-11.png)

#### Custom Environment Variables
Icon at top right of workflow window:
![](/assets/images/2021-12-19-10-34-56.png)

## Debugging
### Bash script
redirect stdout to stderr to see logs in the debug console:
```
>&2 echo $query
```
