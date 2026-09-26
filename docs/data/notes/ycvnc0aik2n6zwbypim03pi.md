
### One-to-many approach
Have a roles table, which has a many-to-one relationship with users:
```sql
create table roles (
  id uuid primary key,
  name text
);

create table permissions (
  id uuid primary key,
  access text,
  roles_id uuid references roles
);

insert into roles (name) values ('admin')
insert into permissions (access) values ('canDeleteUsers');
```

### Many-to-many approach
```sql
create table roles (
  id uuid primary key,
  name text
);

create table user_roles (
  user_id uuid references users,
  role_id uuid references roles,
  unique (role_id, user_id)
);

insert into roles (name) values ('admin')
insert into user_roles (user_id, role_id) values ('123', 'abc');
```