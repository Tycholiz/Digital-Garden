
The newly added ON CONFLICT clause allows to specify an alternative to
raising a unique or exclusion constraint violation error when inserting.
ON CONFLICT refers to constraints that can either be specified using a
inference clause (by specifying the columns of a unique constraint) or
by naming a unique or exclusion constraint.  DO NOTHING avoids the
constraint violation, without touching the pre-existing row.
