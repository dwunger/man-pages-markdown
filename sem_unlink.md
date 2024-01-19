---
title: sem_unlink - Remove a Named Semaphore
date: 2021-03-22
categories: [Linux, Manual]
---

# NAME

sem_unlink - remove a named semaphore

# SYNOPSIS

```c
#include <semaphore.h>

int sem_unlink(const char *name);
```

Link with `-pthread`.

# DESCRIPTION

`sem_unlink()` removes the named semaphore referred to by `name`. The semaphore name is removed immediately. The semaphore is destroyed once all other processes that have the semaphore open close it.

# RETURN VALUE

On success, `sem_unlink()` returns 0; on error, -1 is returned, with `errno` set to indicate the error.

# ERRORS

- `EACCES`: The caller does not have permission to unlink this semaphore.
- `ENAMETOOLONG`: `name` was too long.
- `ENOENT`: There is no semaphore with the given `name`.

# ATTRIBUTES

For an explanation of the terms used in this section, see [attributes(7)](man:attributes(7).

# CONFORMING TO

POSIX.1-2001, POSIX.1-2008.

# SEE ALSO

- [sem_getvalue(3)](man:sem_getvalue(3))
- [sem_open(3)](man:sem_open(3))
- [sem_post(3)](man:sem_post(3))
- [sem_wait(3)](man:sem_wait(3))
- [sem_overview(7)](man:sem_overview(7))
