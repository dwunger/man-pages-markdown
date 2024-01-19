pthread_attr_setdetachstate, pthread_attr_getdetachstate - set/get detach state attribute in thread attributes object

# SYNOPSIS
```c
#include <pthread.h>

int pthread_attr_setdetachstate(pthread_attr_t *attr, int detachstate);
int pthread_attr_getdetachstate(const pthread_attr_t *attr, int *detachstate);
```

Compile and link with `-pthread`.

# DESCRIPTION
The `pthread_attr_setdetachstate()` function sets the detach state attribute of the thread attributes object referred to by `attr` to the value specified in `detachstate`. The detach state attribute determines whether a thread created using the thread attributes object `attr` will be created in a joinable or a detached state.

The following values may be specified in `detachstate`:
- `PTHREAD_CREATE_DETACHED`: Threads that are created using `attr` will be created in a detached state.
- `PTHREAD_CREATE_JOINABLE`: Threads that are created using `attr` will be created in a joinable state.

The default setting of the detach state attribute in a newly initialized thread attributes object is `PTHREAD_CREATE_JOINABLE`.

The `pthread_attr_getdetachstate()` function returns the detach state attribute of the thread attributes object `attr` in the buffer pointed to by `detachstate`.

# RETURN VALUE
On success, these functions return 0; on error, they return a nonzero error number.

# ERRORS
- `EINVAL` (pthread_attr_setdetachstate()): An invalid value was specified in `detachstate`.

# ATTRIBUTES
For an explanation of the terms used in this section, see [attributes(7)](https://man7.org/linux/man-pages/man7/attributes.7.html).

- Thread safety: MT-Safe

# CONFORMING TO
POSIX.1-2001, POSIX.1-2008.

# NOTES
See [pthread_create(3)](https://man7.org/linux/man-pages/man3/pthread_create.3.html) for more details on detached and joinable threads.

A thread that is created in a joinable state should eventually either be joined using [pthread_join(3)](https://man7.org/linux/man-pages/man3/pthread_join.3.html) or detached using [pthread_detach(3)](https://man7.org/linux/man-pages/man3/pthread_detach.3.html); see [pthread_create(3)](https://man7.org/linux/man-pages/man3/pthread_create.3.html).

It is an error to specify the thread ID of a thread that was created in a detached state in a later call to [pthread_detach(3)](https://man7.org/linux/man-pages/man3/pthread_detach.3.html) or [pthread_join(3)](https://man7.org/linux/man-pages/man3/pthread_join.3.html).

# EXAMPLES
See [pthread_attr_init(3)](https://man7.org/linux/man-pages/man3/pthread_attr_init.3.html).

# SEE ALSO
- [pthread_attr_init(3)](https://man7.org/linux/man-pages/man3/pthread_attr_init.3.html)
- [pthread_create(3)](https://man7.org/linux/man-pages/man3/pthread_create.3.html)
- [pthread_detach(3)](https://man7.org/linux/man-pages/man3/pthread_detach.3.html)
- [pthread_join(3)](https://man7.org/linux/man-pages/man3/pthread_join.3.html)
- [pthreads(7)](https://man7.org/linux/man-pages/man7/pthreads.7.html)

