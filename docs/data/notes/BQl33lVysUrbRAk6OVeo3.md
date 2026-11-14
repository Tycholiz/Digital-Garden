
### System Administration
there are 2 functions (getter/setter) that allow us to query and alter run-time configuration parameters:
1. `current_setting` - returns the current `value` of the setting associated with the provided `key`
2. `set_config` - pass the setting name and the new value.
- these functions are executed with a SELECT statement. The result is displayed as a table.
