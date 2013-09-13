postgres-up
===========

Make developing with Postgres as convenient as developing with Sqlite.

**postgres-up** lets you create and run self-contained Postgres
instances with a single command. All the database state (including the
connection socket) lives in a single directory which can be easily copied
and moved about so you can easily create experimental "forks" of your
database.

If you need to run different versions of Postgres for different
projects, that's supported too.

Nothing to run as root; no configuration: just type a single command and
you're ready to go.


Installation
------------

This is a standalone bash script, just copy it somewhere you want it:

    curl -O https://raw.github.com/evansd/postgres-up/master/postgres-up && chmod a+x postgres-up

The idea is that you can bundle a copy of this script in your project
repository so developers only need to type a single command to create a
database and start working.

#### Notes

* You will, of course, need the Postgres binaries installed somewhere. If
**postgres-up** can't find Postgres it will display hints as to how to install it
on your system.

* Hopefully this is obvious, but this is for *developing*
applications with Postgres, absolutely not for running Postgres in
production!


Usage
------

To create a new database (or start using an existing one):

    $ ./postgres-up
    Using Postgres 9.2 binaries in /usr/lib/postgresql/9.2/bin
    Found existing database in /home/dave/projects/postgres-up/pgdata

    - To export this connection info:
      export DATABASE_URL='postgres://postgres@localhost/postgres?host=/home/dave/projects/postgres-up/pgdata'

To connect with the CLI client to a database you're already running:

    $ ./postgres-up psql

To load a database dump into your database:

    $ ./postgres-up pg_restore <path/to/dump/file>

In general, to run any Postgres command against your database:

    $ ./postgres-up <command_name> <command_args ...>


Configuration
-------------

**postgres-up** will run quite happily without any configuration, but you can optionally
create a config file in the same directory as the script called `postgres-up.config`:

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
    postgres_bin_path="/my/special/postgres/install:$postgres_bin_path"

    # You can set default arguments for any Postgres command by setting
    # a variable like this:
    # <command_name>_args=' --myarg=myvalue'
    # For example, to disable fsync for your Postgres instance:
    # postgres_args='--fsync=off'

Additionally, any of the variables set in the config file can also be
set by passing them in the shell environment.


Compatibility
-------------

Currently tested on Ubuntu 13.04 and OS X 10.8 but I aiming for as wide
as possible *\*nix* compatibility so if you have problems running this let
me know. (Windows compatibility is not a target unfortunately.)
