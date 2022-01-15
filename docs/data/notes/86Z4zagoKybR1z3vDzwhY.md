
# Searching
search all code bases with `@json`
- `"json;"`
exclude results from repository WatermelonDB
- `-repo:Nozbe/WatermelonDB`

- checkout a branch from github (ex. for CR)
    - `hub pr checkout <issue number>`


drop role if exists google_data_studio;
create role google_data_studio with login password 'sk338fjw0g74nf92nf%34d8' noinherit;

revoke usage on schema stats from google_data_studio;
grant usage on schema stats to google_data_studio;

revoke select on all tables in schema stats from google_data_studio cascade;
grant select on all tables in schema stats to google_data_studio;



//after all tables

revoke select on stats.daily_registration_historical from google_data_studio;
grant select on stats.daily_registration_historical to google_data_studio;

revoke select on stats.current_active_users_historical from google_data_studio;
grant select on stats.current_active_users_historical to google_data_studio;

revoke select on stats.daily_book_sales_historical from google_data_studio;
grant select on stats.daily_book_sales_historical to google_data_studio;

revoke select on stats.daily_readership_historical from google_data_studio;
grant select on stats.daily_readership_historical to google_data_studio;

revoke select on stats.daily_reading_activity_by_publisher_historical from google_data_studio;
grant select on stats.daily_reading_activity_by_publisher_historical to google_data_studio;
