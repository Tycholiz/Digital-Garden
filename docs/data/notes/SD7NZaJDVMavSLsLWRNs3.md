
When our plpgsql function returns a `setof` rows, we must follow a different procedure:
- the items that are returned by the function are specified by the `RETURN NEXT` or `RETURN QUERY` commands.
- at the end, a final `RETURN` command with no argument, to indicate the function has finished execution.

`RETURN NEXT` and `RETURN QUERY` do not actually cause execution to return from the function call. Instead, they simply append zero or more rows to the function's result set.
- As successive RETURN NEXT or RETURN QUERY commands are executed, the result set is appended to further. The final `RETURN` command causes control to exit the function, leaving us with the dataset that has been constructed up until that point
	- note: naturally, we can omit the final `RETURN` if we are at the last line anyway
