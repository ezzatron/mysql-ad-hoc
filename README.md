# MySQL ad hoc

*Command-line tool for creating short lifespan MySQL servers.*

[![The most recent stable version is 0.1.0][version-image]][Semantic versioning]

## Purpose

*MySQL ad hoc* is an [SQLite]-like command line tool for creating short lifespan
[MySQL] databases for testing purposes. It allows the user to create a database
in an arbitrary directory, completely isolated from other databases on the same
system.

*MySQL ad hoc* is designed to be incorporated as a part of functional tests that
rely on a real MySQL server.

## Usage

### `mysql-ad-hoc`

    mysql-ad-hoc [path]

Opens a command line MySQL client connected to the database at `[path]`. If the
database does not exist, a new one will be created. `[path]` defaults to the
current working directory.

### `mysqld-ad-hoc`

    mysqld-ad-hoc [path]

Creates a temporary MySQL server for the database at `[path]`. If the database
does not exist, a new one will be created. `[path]` defaults to the current
working directory.

To connect to the server, use the socket file at `[path]/mysql.sock`:

    mysql --socket=mysql.sock

## Features

- Doesn't use any networking, only local socket access is allowed.
- User permissions are ignored, no permission granting is necessary.
- Socket located at `[path]/mysql.sock`.
- PID file located at `[path]/mysql.pid`.
- Error log located at `[path]/mysql.err`.
- General log located at `[path]/mysql.log`.

## Caveats

- Unsuitable for production use. Seriously, just don't.

## Requirements

- Bash
- [mysql_install_db], [mysqld_safe], [mysql], and [mysqladmin] must be in the
  system path.
- Probably requires a new-ish version of MySQL. Tested with MySQL 5.6.16.
- No Windows support, sorry.

<!-- References -->

[SQLite]: http://www.sqlite.org/
[MySQL]: http://www.mysql.com/
[mysql_install_db]: https://dev.mysql.com/doc/refman/5.7/en/mysql-install-db.html
[mysqld_safe]: http://dev.mysql.com/doc/refman/5.7/en/mysqld-safe.html
[mysql]: http://dev.mysql.com/doc/refman/5.7/en/mysql.html
[mysqladmin]: http://dev.mysql.com/doc/refman/5.7/en/mysqladmin.html

[Semantic versioning]: http://semver.org/
[version-image]: http://img.shields.io/:semver-0.1.0-yellow.svg "This project uses semantic versioning"
