
PostgreSQL implements the `application_name` parameter, which you can set in the connection string and with the SET command within your session. It is then possible to have it reported in the server’s logs, and it’s also part of the system activity view `pg_stat_activity`.

It is a good idea to be quite granular with this setting, going as low as the module or package level, depending on your programming language of choice. It’s one of those settings that the main application should have full control of, so usually external (and internal) libs are not setting it.
- doing this, we can find out which part of the code is sending a specific query we can see in the monitoring, in the logs or in the interactive activity views.

