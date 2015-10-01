# clisp-sqlite dictionary #
## SQLITE.RAW package ##
These functions call sqlite directly, so they don't provide any fancy
error handling, or argument conversion.
### SQLITE.RAW.sqlite\_open ###
**function** sqlite\_open _database-name-string_ _mode_ => (values _database-pointer_ nil)

attempt to open a database.

### SQLITE.RAW.sqlite\_close ###
**function** sqlite\_close _database-pointer_ => (values)

Close a database. Pass a database pointer returned from sqlite\_open, and don't use it again.

### SQLITE.RAW.sqlite\_exec ###
**function** sqlite\_exec _database-pointer_ _sql-string_ _callback_ nil => (values _error-code_ _error-message_)

Run a query, generating a callback to _callback_ for each returned row. Look at the sqlite docs for more information about callback (_nb: callback should return 0 unless you want trouble!_).

### SQLITE.RAW.sqlite\_last\_inserted\_rowid ###
**function** sqlite\_last\_inserted\_rowid _database-pointer_ => _last-inserted-row-id_

Return the id of the last inserted row.

### SQLITE.RAW.sqlite\_changes ###
**function** sqlite\_changes _database-pointer_ => _rows-modified-or-deleted-or-inserted_

Returns the number of rows inserted, deleted, or modified by the last sqlite\_exec.

## SQLITE package ##
This package provides a more convenient interface to sqlite, complete
with rudimentary condition handling. Database pointers are wrapped in
a sqlite::database object. You can extract the wrapped
database pointer with sqlite::database-address if you
really want it, but it's best not to mix calls between functions from
the sqlite and sqlite.raw packages.

### SQLITE.sqlite-open ###
**function** sqlite-open _filename_ => _open-db-object_

Attempt to open _filename_ as a database object. If it can't be
opened, a condition is raised. _Question: would it be better to
create a special condition to raise instead?_.

### SQLITE.sqlite-close ###
**function** sqlite-close _db-object_ => _closed-db-object_

Close a database connection created by sqlite-open.


### SQLITE.sqlite-with-open-db ###
**macro** with-open-db (_db-var_ _filename_) &body _body_

Bind the _db-var_ variable to an open database named
by _filename_ for the body of the macro, makes certain using
unwind-protect that the database is closed.

### SQLITE.sqlite-exec ###
**function** sqlite-exec _db-object_ _query-string_ _callback_ &optional _argument_ => t

Executes _query-string_ á la sqlite\_exec,
except that t is returned in normal cases. In
exceptional cases, an error is raised instead.

### SQLITE.sql ###
**function** sql _db-object_ _sql-query_ => _sql-result_

Execute an sql query, returning a list of rows, where each row is a
list of columns in the plain-old string format returned by sqlite.
Not a particularly efficient function.

### SQLITE.with-query ###
**macro** with-query (_db-object_ (_arg-0_ ... _arg-n_) _sql-query-format-template_ &rest _query-format-template-args_) &body _body_

A thoroughly useful macro. The sql query is produced by
applying format to the format template and the
template args, that way your query can be computed dynamically.
As each row is returned, each column is bound to the corresponding
variable named in **arg-. Then the body is evaluated. It
could probably stand some optimization. Here's an example:**

```
(with-open-db (db #"foo.db")
  (with-query (db (id name email) "select id, name, email from student where grade=~s" 12)
    (format t "~s ~s ~s~%" id name email)))
```