
# Trigger
- a callback that is executed whenever a table (or view) is modified. Triggers can also be set to listen for specific user actions much like a callback.
	- Can also be set up to be executed by using the INSTEAD OF condition.
- 2 main types of trigger:
	1. row-level trigger
	2. statement-level trigger
	- the difference between these two is in how many times each would be called in response to an event. 
		-  ex. if you issue an UPDATE statement that affects 20 rows, the row-level trigger will be invoked 20 times, while the statement level trigger will be invoked 1 time.
- examples
	- restrict DML (actions that modify the db) operations to business hours 
	- Automatically generate derived column values
- You can think of a trigger like a middleware that sits between the user's request (that interacts with the db) and the sql server. Every time our db is interacted with, listeners are able "intercept" the query and act on it
- Most triggers are only activated by either INSERT or UPDATE statements.
- We can specify columns on a trigger, which will cause the trigger to only fire if those columns are operated on (ex. we update those columns)
- If multiple triggers of the same kind are defined for the same event, they will be fired in alphanumerical order.
	- which is why it is useful to prepend them with numbers

## Syntax
- A trigger that is marked `FOR EACH ROW` is called once for every row that the operation modifies.
- a trigger that is marked `FOR EACH STATEMENT` only executes once for any given operation
- `WHEN` allows us to determine whether or not the trigger should be fired
	- In row-level triggers the `WHEN` condition can examine the old and/or new values of columns of the row
	- ex. only execute the function if `OLD.balance` does not equal `NEW.balance`
		- `WHEN (OLD.balance IS DISTINCT FROM NEW.balance)`
	- ex. only execute function if anything has changed
		- `WHEN (OLD.* IS DISTINCT FROM NEW.*)`
- `CONSTRAINT` allows us to adjust the timing of when the trigger actually fires.
	- The trigger can be fired either:
		- At the end of the statement which caused the triggering event
		- At the end of the containing transaction, which is the `COMMIT` (called deferred triggers)
	- Each constraint has its own IMMEDIATE or DEFERRED mode.
	- only available `AFTER ROW`

# Parts of a Trigger
- 2 parts to a trigger: the trigger itself, and the function that is executed by the trigger

## Trigger

### Before/After Triggers
The trigger can be specified to fire before the operation is attempted on a row, or after the operation has completed (after constraints are checked and the INSERT, UPDATE, or DELETE has completed)
- Trigger can also be set to fire *instead of* having the operation be performed. This only works on views

#### Before
- the constraints will not-yet have been checked, allowing us to perform some action before the actual operation has taken place.
	- ex. Imagine we want to keep only one of the user's credit cards specified as `is_primary`. Because of the uniqueness constraints applied on the table (allowing only one `is_primary` card per user), we must set `is_primary` to `false` for all of our cards, any time the user attempts to insert a new card where `is_primary` is true. 
		- If we had instead ran the trigger *after* the `insert`, then we might have gotten an error, since we are potentially trying to insert a new card with `is_primary = true`, while one may already exist.
- If the trigger fires before the event, the trigger can skip the operation for the current row, or change the row being inserted (for INSERT and UPDATE operations only)
- in `BEFORE`, the `WHEN` condition is evaluated just before the function is executed
	- Therefore, using `WHEN` (in the trigger itself) has the same effect as testing the same condition at the beginning of the triggered function with `TG_WHEN`
	- The implication of this is that the `NEW` value seen by the function is the current value, and not the value that would exist *following* the operation
		- Also consider that while this "current value" normally means "prior to the operation being performed", there is the possibility that previous triggers in the chain have already executed, potentially changing what "current value" means to us.
	- Another implication is that in `BEFORE`, a trigger's `WHEN` condition cannot examine the columns of the `NEW` row that is to be inserted/updated (because they wouldn't have been set yet)

#### After
- rows will be impacted with consideration to the constraints
- If the trigger fires after the event, all changes, including the effects of other triggers, are "visible" to the trigger.
- in `AFTER`, the `WHEN` condition is evaluated after the row update occurs.
	- The `WHEN` condition here also determines whether an event is queued to fire a trigger, following the operation.
		- Therefore, if `WHEN` does not return true, we do not have to queue an event; nor to re-fetch the row at the end of the statement
			- This can result in significant speedups in statements that modify many rows, if the trigger only needs to be fired for a few of the rows.

## Trigger Function
Depending on if we are using row-level triggers or table-level triggers, the function will be called for each row that is affected, or will be called once per table.

A trigger function must return either `NULL` or a row value having exactly the structure of the table the trigger was fired for.
- ie. the the row being returned must have the same type of the table.

- a trigger function returns a `trigger`
- if a trigger function updates a row in the same table that the trigger is for and the operation is the same, then it will trigger a recursive infinite loop, since the trigger function will cause the trigger to fire in response to the 
	- ex. we have a trigger that is set to fire on update of `users` table, and the trigger function itself updates a row in that table. This update, will cause the trigger to fire again, and so on.

### Return value of trigger functions
`BEFORE`
- if we return `null` from the function, we are signalling to the trigger manager signal that we want to skip the rest of the operation for this row, meaning 2 things happen:
	1. subsequent triggers will be short-circuited 
	2. the operation will not occur for this row.
- if we return a non-`null` value, then the operation proceeds with that value. 
	- returning a row value that is different from the original value of `NEW` will alter the row to be inserted/updated
		- Therefore, if we want the trigger to succeed normally without altering the `NEW` row value, then we must return `NEW`
			 - This means that we could alter single columns in the row, with `NEW.col = X`, and proceeding to return `NEW`
- a statement-level trigger fired `BEFORE` always ignores the return value of the trigger function, so it might as well be `null`.

`AFTER`
- return value of a row-level trigger (or statement-level trigger) fired `AFTER` will always be ignored, so it might as well be `null`

### Trigger Local Variables
A function called by a trigger receives data about its calling environment (called `TriggerData`)
- prefixed by `TG_`

`NEW` 
- variable holding the new database row for INSERT/UPDATE operations in row-level triggers
- type: `record`
- in statement-level triggers and `DELETE` operations, this value is `null`
	- this shows why we can return `null` from the trigger function on `DELETE` operations. There is nothing to return to the table!
	- must still return a non-`null` value from a DELETE trigger function, or we will short-circuit the trigger action. Normally, `OLD` is returned, since `NEW` is null

`OLD`
- variable holding the old database row for UPDATE/DELETE operations in row-level triggers
- type: `record`
- in statement-level triggers and `INSERT` operations, this value is `null`
	
`TG_WHEN`
- evaluates to BEFORE, AFTER, or INSTEAD OF, depending on the trigger's definition.
- type: `text`

`TG_OP`
- evaluates to INSERT, UPDATE, DELETE, or TRUNCATE telling for which operation the trigger was fired.
- type: `text`

`TG_ARGV`
- the arguments passed in to the trigger's function call
- type: `text[]`

Also
`TG_TABLE_NAME`, `TG_TABLE_SCHEMA`, `TG_NARGS` (# of args)

