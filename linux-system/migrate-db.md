## Supabase 

### Dump

[Install postgres tools 15 if needed (it is for supabase)](https://linuxcapable.com/install-postgresql-15-on-ubuntu/)

PGPASSWORD='' pg_dump -h **HOST NAME** -p 5432 -U **DB USRENAME** -d **DB NAME** -F p --no-owner --no-privileges -f supabase_dump.sql

### Restore (to avien)
```
  PGPASSWORD=your_avien_password psql \
  -h avien_host \
  -p 5432 \
  -U avien_user \
  -d avien_db \
  -f supabase_dump.sql
```
