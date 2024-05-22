delete all privileges, assigned for user or role:
```sql
drop owned by user
drop owned by role
```

view all privileges by user or role:
```sql
SELECT grantee, table_name, privilege_type
FROM information_schema.table_privileges
WHERE grantee = 'user'
```

connect:
```bash
psql -h hostname -d database -U username -W
```

list all:
```sql
--databases:
\l
--users and roles:
\du
--schemas:
\dn
--tables:
\dt
--views:
\dv
--materialized views:
\dm
--functions:
\df
```

switch to another database:
```sql
\c database
```

describe table, with extra info:
```sql
\d table
\d+ table
```

retrieve specific user of role:
```sql
\du username
```

save query results to a file:
```sql
\o filename
...
\o
```

run commands from file:
```sql
\i filename
```

show privileges for table:
```sql
\dp table
```