---
title: sem_close - Close a Named Semaphore
date: 2021-03-22
categories: [Linux, Manual]
---

# NAME

sem_close - close a named semaphore

# SYNOPSIS

```c
#include <semaphore.h>

int sem_close(sem_t *sem);
```

Link with `-pthread`.

# DESCRIPTION

`sem_close()` closes the named semaphore referred to by `sem`, allowing any resources that the system has allocated to the calling process for this semaphore to be freed.

# RETURN VALUE

On success, `sem_close()` returns 0; on error, -1 is returned, with `errno` set to indicate the error.

# ERRORS

- `EINVAL`: `sem` is not a valid semaphore.

# ATTRIBUTES

For an explanation of the terms used in this section, see [attributes(7)](man:attributes(7)).

# CONFORMING TO

POSIX.1-2001, POSIX.1-2008.

# NOTES

All open named semaphores are automatically closed on process termination, or upon [execve(2)](man:execve(2)).

# SEE ALSO

- [sem_getvalue(3)](man:sem_getvalue(3))
- [sem_open(3)](man:sem_open(3))
- [sem_post(3)](man:sem_post(3))
- [sem_unlink(3)](man:sem_unlink(3))
- [sem_wait(3)](man:sem_wait(3))
- [sem_overview(7)](man:sem_overview(7))
