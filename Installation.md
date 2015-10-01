# Summary #

`clisp-sqlite` is only available (as the name suggests) for [clisp](http://clisp.cons.org), a common lisp environment running on many common platforms. You can install it using [asdf-install](http://www.cliki.net/asdf-install), if you have [asdf](http://www.cliki.net/asdf) already installed; if not, you may find it easier to simply download and `load` the clisp-sqlite source code.

# Prerequisites #
  1. A recent version of [clisp](http://clisp.cons.org)
  1. A copy of [SQLite](http://sqlite.org)
  1. Optional: [ASDF](http://cliki.net/asdf), if you want to install using http://cliki.net/asdf-install

# Installing using `asdf-install` #
```
(asdf-install:install :clisp-sqlite)
```

# Using the source directly #
  1. Download [clisp-sqlite.lisp](http://clisp-sqlite.googlecode.com/files/clisp-sqlite.lisp) directly into your project
  1. Load it and go!