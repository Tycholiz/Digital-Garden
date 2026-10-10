
## Tracking Changes
- GM has an .gmrc file, which allows us to define hooks. We can therefore define a hook that will run `pg_dump` on the shadow db after every migration. If we track this file in git, then we can see a clear history of what each migration has done.

## Development
- In Development, aside from the main database, there is a shadow db which is used internally by graphile-migrate and is mainly for testing consistency of the migrations, among other minor tasks
- the shadow database gets reset frequently
- `commit`, `uncommit`, `watch` and `reset` are development-only commands.

## Flow
1. We write our new idempotent migration in `current.sql`
	- any seed data we have can be placed at the bottom, to be removed prior to committing.
2. when ready, we run `commit`
	- this should only be done immediately before the branch is merged into master (see README###Collaboration). This it to ensure commits are linear.
	- all sql in `current.sql` is removed and catalogued into `committed/`
	- the shadow database is dropped, and recreated by running all migrations. This is to ensure that the migration works without a hitch.

* * *

### Pitfalls
- when dropping tables, schemas, and functions, we should use CASCADE
- use idempotent commands whenever possible.
	- ex. DROP ____ IF EXISTS
	- ex. CREATE OR REPLACE FUNCTION
	- The initial migrations don't need to be idempotent if this migration starts off by dropping all schemas (which implicitly drops all tables attached to those schemas)
