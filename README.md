postgres-up
===========

Make developing with PostgreSQL as convenient as developing with Sqlite.

**postgres-up** creates and runs self-contained PostgreSQL instances. All the
database state (including the connection socket) lives in a single
directory which can be easily copied and moved about so you can easily
create experimental "forks" of your database.

If you need to run different versions of Postgres for different
projects, that's supported too.


Installation
------------

This is a standalone bash script, just copy it somewhere you want it:

    curl -O https://raw.github.com/evansd/postgres-up/master/postgres-up && chmod a+x postgres-up

The idea is that you can bundle this script with your project (together with a config file
-- see below) to make it as easy as possible for developers to get started.

Of course, you'll need the Postgres binaries installed somewhere. If *postgres-up*
can't find Postgres it will display hints as to how to install Postgres on your
system.


Example session
---------------

    $ ./postgres-up
    Using Postgres 9.2 binaries in /usr/lib/postgresql/9.2/bin
    Found existing database in /home/dave/projects/postgres-up/pgdata

    - To create a new database just delete or move this directory
      To make a copy, just use cp -r (with the server stopped)

    - To get more logging output from Postgres run:
      bin/postgres-up -d 2

    - To export this connection info:
      export DATABASE_URL='postgres://postgres@localhost/postgres?host=/home/dave/projects/postgres-up/pgdata'

    - To connect to this database with the CLI client:
      psql -d "$DATABASE_URL"

    - To load a database dump (will drop existing tables):
      pg_restore -cOxd "$DATABASE_URL" <path/to/dumpfile>

    LOG:  database system was shut down at 2013-09-07 17:24:48 BST
    LOG:  database system is ready to accept connections


Configuration
-------------

Optionally, you can create a `postgres-up.config` file in the same
directory as the script:

    # Example config file for postgres-up
    #
    # This file is entirely optional: default values will be used if it is missing
    #
    # This file should be placed in the same directory as the postgres-up script
    #
    # This file is read by 'sourcing' it in bash, so you can use any bash constructs
    # you like to set the config variables
    
    # Directory to store data files, evaluated relative to the config file path
    # Default is ./pgdata
    postgres_data_dir=./pgdata
    
    # Postgres version to use (major and minor version only e.g. 9.2 not 9.2.4)
    # If not specified will use the first version it finds
    postgres_version=9.2
    
    # Directories in which to look for Postgres binaries
    # Sensible defaults will be used if this is not specified
    postgres_bin_path="/my/special/postgres/install:$PATH"

Additionally, any of the variables set in the config file can also be
set by passing them in the shell environment.


Compatibility
-------------

Currently tested on Ubuntu 13.04 and OS X 10.8 but I aiming for as wide
as possible \*nix compatibility so if you have problems running this let
me know. (Windows compatibility is not a target unfortunately.)
