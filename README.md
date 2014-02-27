# MySQL ad hoc

*Command-line tool for creating short lifespan MySQL servers.*

[![The most recent stable version is 0.1.0][version-image]][Semantic versioning]

## Purpose

*MySQL ad hoc* is a command line tool for creating short lifespan [MySQL]
databases for testing purposes. It allows the user to create a database in an
arbitrary directory, completely isolated from other databases on the same
system.

*MySQL ad hoc* is designed to be incorporated into functional tests that rely on
a MySQL server.

## Usage

### mysql-ad-hoc

    mysql-ad-hoc [path] [port] [address]

Opens a command line MySQL client connected to the database at `[path]`. If the
database does not exist, a new one will be created. `[path]` defaults to the
current working directory.

The MySQL server listens for incoming connections on a temporary unix socket.
If `[port]` is specified, the underlying server will also listen on this TCP
port. Additionally, specifying `[address]` binds the server to the given
address.

When the client exits, the underlying server will be shut down, unless there
are other active connections.

#### Examples

    mysql-ad-hoc /path/to/db
    mysql-ad-hoc /path/to/db 3333
    mysql-ad-hoc /path/to/db 3333 localhost

### mysqld-ad-hoc

    mysqld-ad-hoc [path] [port] [address]

Creates a temporary MySQL server for the database at `[path]`. If the database
does not exist, a new one will be created. `[path]` defaults to the current
working directory.

The MySQL server listens for incoming connections on a temporary unix socket.
If `[port]` is specified, the underlying server will also listen on this TCP
port. Additionally, specifying `[address]` binds the server to the given
address.

If signalled with SIGINT (e.g. by pressing control-c in the terminal), the
server will be shut down, unless there are active connections. To manually
connect to the server use the unix socket at `[path]/mysql.sock`:

    mysql --socket=mysql.sock

The server will also shut down automatically when the parent process exits,
making it extremely suitable for execution from within another application.

#### Examples

    mysqld-ad-hoc /path/to/db
    mysqld-ad-hoc /path/to/db 3333
    mysqld-ad-hoc /path/to/db 3333 localhost

## Features

- User permissions are ignored, no permission granting is necessary.
- Skips as much unnecessary stuff as possible.
- Socket located at `[path]/mysql.sock`.
- PID file located at `[path]/mysql.pid`.
- Error log located at `[path]/mysql.err`.
- General log located at `[path]/mysql.log`.

## Caveats

- Initial database creation takes quite some time; around 10-15 seconds
  on a current-gen iMac. Any tips on reducing this time are welcome.
- Manually opening a connection to a server, then shutting down the server
  script can result in a detached server running in the background. The easiest
  way to fix this is to ensure that all client connections are closed, run
  `mysqld-ad-hoc` on the path, and let it handle shutdown correctly when it
  closes.
- Unsuitable for production use. Seriously, just don't.

## Requirements

- Bash.
- [mysql_install_db], [mysqld_safe], [mysql], and [mysqladmin] must be in the
  system path.
- Probably requires a new-ish version of MySQL. Tested with MySQL 5.6.16.
- No Windows support, sorry.

<!-- References -->

[MySQL]: http://www.mysql.com/
[mysql_install_db]: https://dev.mysql.com/doc/refman/5.7/en/mysql-install-db.html
[mysqld_safe]: http://dev.mysql.com/doc/refman/5.7/en/mysqld-safe.html
[mysql]: http://dev.mysql.com/doc/refman/5.7/en/mysql.html
[mysqladmin]: http://dev.mysql.com/doc/refman/5.7/en/mysqladmin.html

[Semantic versioning]: http://semver.org/
[version-image]: http://img.shields.io/:semver-0.1.0-yellow.svg "This project uses semantic versioning"
