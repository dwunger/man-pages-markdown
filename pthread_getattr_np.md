---
title: pthread_getattr_np - Linux Programmer's Manual
date: 2021-03-22
categories: [Linux, Manual]
---

# NAME

pthread_getattr_np - get attributes of created thread

# SYNOPSIS

```c
#define _GNU_SOURCE             /* See feature_test_macros(7) */
#include <pthread.h>

int pthread_getattr_np(pthread_t thread, pthread_attr_t *attr);
```

Compile and link with `-pthread`.

# DESCRIPTION

The `pthread_getattr_np()` function initializes the thread attributes object referred to by `attr` so that it contains actual attribute values describing the running thread `thread`.

The returned attribute values may differ from the corresponding attribute values passed in the `attr` object that was used to create the thread using `pthread_create(3)`. In particular, the following attributes may differ:

- The detach state, since a joinable thread may have detached itself after creation.
- The stack size, which the implementation may align to a suitable boundary.
- The guard size, which the implementation may round upward to a multiple of the page size or treat as 0 if the application is allocating its own stack.

Furthermore, if the stack address attribute was not set in the thread attributes object used to create the thread, then the returned thread attributes object will report the actual stack address that the implementation selected for the thread.

When the thread attributes object returned by `pthread_getattr_np()` is no longer required, it should be destroyed using `pthread_attr_destroy(3)`.

# RETURN VALUE

On success, this function returns 0; on error, it returns a nonzero error number.

# ERRORS

- `ENOMEM`: Insufficient memory.

In addition, if `thread` refers to the main thread, then `pthread_getattr_np()` can fail because of errors from various underlying calls:

- `fopen(3)`, if `/proc/self/maps` can't be opened.
- `getrlimit(2)`, if the `RLIMIT_STACK` resource limit is not supported.

# VERSIONS

This function is available in glibc since version 2.2.3.

# ATTRIBUTES

For an explanation of the terms used in this section, see `attributes(7)`.

```
Interface   Attribute   Value
pthread_getattr_np()   Thread safety   MT-Safe
```

# EXAMPLES

The program below demonstrates the use of `pthread_getattr_np()`. The program creates a thread that then uses `pthread_getattr_np()` to retrieve and display its guard size, stack address, and stack size attributes. Command-line arguments can be used to set these attributes to values other than the default when creating the thread.

In the first run, on an x86-32 system, a thread is created using default attributes:

```shell
$ ulimit -s   # No stack limit ==> default stack size is 2 MB
unlimited
$ ./a.out
Attributes of created thread:
        Guard size          = 4096 bytes
        Stack address       = 0x40196000 (EOS = 0x40397000)
        Stack size          = 0x201000 (2101248) bytes
```

In the following run, we see that if a guard size is specified, it is rounded up to the next multiple of the system page size (4096 bytes on x86-32):

```shell
$ ./a.out -g 4097
Thread attributes object after initializations:
        Guard size          = 4097 bytes
        Stack address       = (nil)
        Stack size          = 0x0 (0) bytes

Attributes of created thread:
        Guard size          = 8192 bytes
        Stack address       = 0x40196000 (EOS = 0x40397000)
        Stack size          = 0x201000 (2101248) bytes
```

In the last run, the program manually allocates a stack for the thread. In this case, the guard size attribute is ignored.

```shell
$ ./a.out -g 4096 -s 0x8000 -a
Allocated thread stack at 0x804d000

Thread attributes object after initializations:
        Guard size          = 4096 bytes
        Stack address       = 0x804d000 (EOS = 0x8055000)
        Stack size          = 0x8000 (32768) bytes

Attributes of created thread:
        Guard size          = 0 bytes
        Stack address       = 0x804d000 (EOS = 0x8055000)
        Stack size          = 0x8000 (32768) bytes
```

# SEE ALSO

- `pthread_attr_getaffinity_np(3)`
- `pthread_attr_getdetachstate(3)`
- `pthread_attr_getguardsize(3)`
- `pthread_attr_getinheritsched(3)`
- `pthread_attr_getschedparam(3)`
- `pthread_attr_getschedpolicy(3)`
- `pthread_attr_getscope(3)`
- `pthread_attr_getstack(3)`
- `pthread_attr_getstackaddr(3)`
- `pthread_attr_getstacksize(3)`
- `pthread_attr_init(3)`
- `pthread_create(3)`
- `pthreads(7)`
