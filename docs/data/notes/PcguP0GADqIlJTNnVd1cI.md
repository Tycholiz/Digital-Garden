
### Step
A step is an individual task that can run commands in a job

A step can either be:
- a shell command
- an action

Each step in a job executes on the same runner, allowing the actions in that job to share data with each other.

Changing directories in one step does not carry through to the next step:
```yml
# workflow.actions.yml
    - name: cd into publishable
      run: |
        cd publishable 
        pwd

    - name: does the cd carry thru?
      run: |
        pwd
```

log:
```
Run cd publishable
/home/runner/work/Digital-Garden/Digital-Garden/publishable

Run pwd
/home/runner/work/Digital-Garden/Digital-Garden
```