
#### Delete from one table while using values from another as the condition
Here, we delete from the `users` table where the `user.id` can be found in the result set. We are doubling up the result set with `UNION`, in an effort to find users that are test users

```sql
delete from app_public.users
where id in
	(
		select user_id from app_public.user_emails where email like 'testuser%@example.com'
	union
		select user_id from app_public.user_authentications where identifier = '123456%'
	)
```