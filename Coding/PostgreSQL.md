# PostgreSQL

## Installation

```bash
# download package
yay -S postgresql

# initialize database
sudo -iu postgres initdb -D /var/lib/postgres/data

# start
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

### Allow external connections

```bash
sudo nvim /var/lib/postgres/data/postgres.conf
listen_address = '*'
:wq

sudo nvim /var/lib/postgres/data/pg_hba.conf
host all all 0.0.0.0/0 md5
:wq

sudo systemctl restart postgresql
```

## PostgreSQL shell

You can log in to the DB with these steps

```bash
# log in as 'postgres' user
sudo -iu postgres

# enter postgre shell
psql

# ---------- do stuff ----------

# exit shell
\q

# log out from 'postgres' user account
exit
```

### PostgreSQL commands

```sql
-- create new database and user
create database mydb;
create user myuser with encrypted password 'mypassword';

-- permissions and stuff
grant all privileges on database mydb to myuser;
grant all privileges on schema public to myuser;
grant usage, create on schcema public to myuser;

-- list all...
select datname from pg_database;  -- databases
select table_name from information_schema.tables where table_schema='f';  -- tables from schema
```


---

# Psycopg2

`Psycopg2` is a Python library to work with PostgreSQL

## Installation

```bash
python -m venv venv
source ./venv/bin/activate

pip install psycopg2
# pip install psycopg2-binary
```

## Usage
```python
import p
```
