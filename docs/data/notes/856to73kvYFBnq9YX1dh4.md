
- vim records keystrokes until we leave *i mode*
- moving around in *i mode* resets the change, meaning once we exit *i mode* and undo with `u`, it will undo all changes made in that *i mode*.

## Insert-Normal Mode
- use *insert normal mode* from within insert mode for a one shot command within normal mode (then return to i mode)
