# PostgreSQL
This API uses version 9.5.10 of PostgreSQL.

## Install for the first time on Ubuntu dev machine

In Ubuntu Software, find the `postgresql96` application and install it. You will need to register an Ubuntu One account.

In Terminal, setup a password for the default `postgres` user

```
sudo -u postgres psql postgres
\password postgres
\q
```

Allow local connections

Edit or create the file `/etc/postgresql/9.5/main/pg_hba.conf` and put at the end of the file:

```
local  all                                          trust
host   all        127.0.0.1      255.255.255.255    trust
```

Restart postgres:
```
sudo service postgresql restart
```

Source: http://suite.opengeo.org/docs/latest/dataadmin/pgGettingStarted/firstconnect.html

## Create the `psd2hackathon` database
```
psql (or psql -U postgres)
create database psd2hackathon;
create user [your database username] with password '[your database password]';
grant all privileges on database psd2hackathon to [your database username];
\q
```

### Create the main table for all apps
```
psql -U [your database username] -d psd2hackathon
create table apps (
  id          varchar(255) CONSTRAINT apps_pk PRIMARY KEY,
  name        varchar(255) NOT NULL UNIQUE,
  author      varchar(255) NOT NULL,
  contact     varchar(255) NOT NULL,
  enabled     boolean,
  created     bigint
);
```

### Optional: add monitoring if it's not already there
```
select * from pg_extension;
create extension pg_stat_statements;
```