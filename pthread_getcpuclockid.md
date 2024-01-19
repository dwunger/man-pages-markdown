---
title: pthread_getcpuclockid - Linux Programmer's Manual
date: 2021-03-22
categories: [Linux, Manual]
---

# NAME

pthread_getcpuclockid - retrieve ID of a thread's CPU time clock

# SYNOPSIS

```c
#include <pthread.h>
#include <time.h>

int pthread_getcpuclockid(pthread_t thread, clockid_t *clockid);
```

Compile and link with `-pthread`.

# DESCRIPTION

The `pthread_getcpuclockid()` function obtains the ID of the CPU-time clock of the thread whose ID is given in `thread`, and returns it in the location pointed to by `clockid`.

The clockid is constructed as follows: `*clockid = CLOCK_THREAD_CPUTIME_ID | (pd->tid << CLOCK_IDFIELD_SIZE)` where CLOCK_IDFIELD_SIZE is 3.

# RETURN VALUE

On success, this function returns 0; on error, it returns a nonzero error number.

# ERRORS

- `ENOENT`: Per-thread CPU time clocks are not supported by the system.
- `ESRCH`: No thread with the ID `thread` could be found.

# VERSIONS

This function is available in glibc since version 2.2.

# ATTRIBUTES

For an explanation of the terms used in this section, see `attributes(7)`.

```
Interface   Attribute   Value
pthread_getcpuclockid()   Thread safety   MT-Safe
```

# CONFORMING TO

POSIX.1-2001, POSIX.1-2008.

# NOTES

When `thread` refers to the calling thread, this function returns an identifier that refers to the same clock manipulated by `clock_gettime(2)` and `clock_settime(2)` when given the clock ID `CLOCK_THREAD_CPUTIME_ID`.

# EXAMPLES

The program below creates a thread and then uses `clock_gettime(2)` to retrieve the total process CPU time, and the per-thread CPU time consumed by the two threads. The following shell session shows an example run:

```shell
$ ./a.out
Main thread sleeping
Subthread starting infinite loop
Main thread consuming some CPU time...
Process total CPU time:    1.368
Main thread CPU time:      0.376
Subthread CPU time:        0.992
```

# SEE ALSO

- `clock_gettime(2)`
- `clock_settime(2)`
- `timer_create(2)`
- `clock_getcpuclockid(3)`
- `pthread_self(3)`
- `pthreads(7)`
- `time(7)`
