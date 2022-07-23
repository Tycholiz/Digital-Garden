
`RAISE` can be an easy and powerful way to self-document the code.

## Levels
- DEBUG
- LOG
- INFO
- NOTICE
- WARNING
- EXCEPTION (default level)
 - raises an error, aborting current transaction

## Where and when messages get sent
raising with different levels will generate messages of different priority levels.
- Where or not a message of a given priority will be sent to the client or not is determined by the `log_min_messages` and `client_min_messages` configuration variables.
	- This file also determines if we write to server log

## Body
After specifying the level, we can provide a simple string literal, which can be followed by optional argument expressions to be inserted into the string
```
RAISE NOTICE '% %', v_job_id, v_job_name;
```

### Using
We can attach additional info to the error by combining `using` with an option, one of:
- MESSAGE
- DETAIL
- HINT
- ERRCODE
	- Specifies the error code (SQLSTATE) to report, either by condition name or directly as a five-character SQLSTATE code
- COLUMN
- CONSTRAINT
- DATATYPE
- TABLE
- SCHEMA
```
RAISE EXCEPTION 'Nonexistent ID --> %', user_id
      USING HINT = 'Please check your user ID';
```
