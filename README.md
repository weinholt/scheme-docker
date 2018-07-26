# Scheme implementations in Docker

This repository contains Dockerfiles that provide various Scheme
implementations. They all install the `scheme-script` wrapper, so that
programs starting with `#!/usr/bin/env scheme-script` run straight out
of the box.

The Dockerfiles are provided on separate branches so that automated
builds on Docker Hub will not be triggered unnecessarily. The builds
are available here: https://hub.docker.com/u/weinholt/

## Usage

To be compatible with most Schemes, put this at the top of your
programs:

```Scheme
#!/usr/bin/env scheme-script
;; -*- mode: scheme; coding: utf-8 -*- !#
#!r6rs
```

The `!#` token is needed to have Guile stop parsing the `#!` block
comment. The `#!r6rs` token is needed for Racket to switch to the R6RS
language. The mode and coding comment is not strictly needed, but
helps Emacs users.

## Chez Scheme

Chez Scheme is built in a special way that removes the expression
editor and therefore removes the ncurses and X dependencies. This is
useful when you want to distribute the resulting binary bundled
together with your own program, because the result is more portable
and only requires GNU libc.
