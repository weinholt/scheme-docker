# Scheme implementations in Docker

This repository contains Dockerfiles that provide various Scheme
implementations. They all install a `scheme-script` or `scheme-r7rs`
wrapper, so that programs starting with the right shebang run straight
out of the box.

The Dockerfiles are provided on separate branches so that automated
builds on Docker Hub will not be triggered unnecessarily. The builds
are available here: https://hub.docker.com/u/weinholt/

## Usage

### R6RS

To be compatible with most Schemes, put this at the top of your
programs:

```Scheme
#!/usr/bin/env scheme-script
;; -*- mode: scheme; coding: utf-8 -*- !#
#!r6rs
```

This follows the [R6RS non-normative appendix](http://www.r6rs.org/final/html/r6rs-app/r6rs-app-Z-H-6.html#node_sec_D.1).

The `!#` token is needed to have Guile stop parsing the `#!` block
comment. The `#!r6rs` token is needed for Racket to switch to the R6RS
language. The mode and coding comment is not strictly needed, but
helps Emacs users.

### R7RS

To be compatible with the R7RS implementations in this project, put
this at the top of your programs:

```Scheme
#!/usr/bin/env scheme-r7rs
(define (main args)
  <your program here>)
```

This follows [SRFI-22](https://srfi.schemers.org/srfi-22/).

## Implementation notes

### Chez Scheme

Chez Scheme in this project is built in a special way that removes the
expression editor and therefore removes the ncurses and X
dependencies. This is useful when you want to distribute the resulting
binary bundled together with your own program, because the result is
more portable and only requires GNU libc.

### Loko Scheme

The `loko:base` image provides *only* Loko Scheme and no other user
space.
