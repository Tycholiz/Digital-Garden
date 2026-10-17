
allows us to do the work of a SELECT statement, but then immediately discard the result
- useful for when we are doing side-effects with no useful result.

```sql
PERFORM create_mv('cs_session_page_requests_mv', my_query);
```

```sql
PERFORM graphile_worker.add_job(
	'user__audit',
	json_build_object(
		'type', 'reset_password',
		'user_id', v_user.id,
		'current_user_id', app_public.current_user_id()
	));
```


role