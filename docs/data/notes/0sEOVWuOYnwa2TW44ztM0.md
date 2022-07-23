The information_schema is a schema that contains a set of views that contain information about the objects defined in the database.

The information_schema is part of the SQL standard, unlike the [[system catalog|pg.system.catalog]]

- As a consequence of this, you will not find information on Postgres-specific features in the information schema. An example of this is the fact that we can get all user-defined types on `information_schema.user_defined_types`, but we cannot get enums. Enums aren't part of the SQL spec, and therefore are not findable through the information_schema.
