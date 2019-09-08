## install postgresql on ubuntu

sudo apt-get install python-pip python-dev libpq-dev postgresql postgresql-contrib


## install postgresql on mac using homebrew

brew info postgresql

First ensure that you have the correct PG binaries running:
which psql
>>> /usr/local/bin/psql

psql --version

Homebrew installs the data directory in /usr/local/var/postgres/ and doesn't touch that data folder when you upgrade from one version to the next

backup database

mv /usr/local/var/postgres/ /usr/local/var/postgres.9.3.backup/

## create database and user

sudo su - postgres

psql

CREATE DATABASE myproject;

CREATE USER myprojectuser WITH PASSWORD 'password';

ALTER ROLE myprojectuser SET client_encoding TO 'utf8';
ALTER ROLE myprojectuser SET default_transaction_isolation TO 'read committed';

ALTER ROLE myprojectuser SET timezone TO 'UTC';

GRANT ALL PRIVILEGES ON DATABASE myproject TO myprojectuser;

\q

exit


Please note the following commands:
* \list or \l: list all databases
* \dt: list all tables in the current database
psql -U postgres -l


