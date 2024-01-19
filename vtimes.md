# vtimes(3) - Obsolete Time Accounting Functions

## NAME

`vtimes` - Obsolete time accounting functions

## SYNOPSIS

```c
#define _BSD_SOURCE             /* See feature_test_macros(7) */
#include <sys/time.h>
#include <sys/resource.h>

int vtimes(struct rusage *usage);
```

## DESCRIPTION

The `vtimes` function is an obsolete system call that provided a way to measure process time and system time. It returned information about the time used by a process and its children. This function has been deprecated, and new programs should use the `getrusage(2)` system call instead.

## RETURN VALUE

The `vtimes` function returns 0 on success and -1 on error. In case of an error, `errno` is set to indicate the error.

## ERRORS

- `EFAULT`: The `usage` pointer provided is invalid.
- `EINTR`: The call was interrupted by a signal.

## NOTES

New programs should use the `getrusage(2)` system call for obtaining resource usage information as it provides more comprehensive and accurate data.

## SEE ALSO

- `getrusage(2)` - Replacement for the `vtimes` function. It provides more detailed resource usage information.

Online documentation for `getrusage(2)` can be found [here](https://man7.org/linux/man-pages/man2/getrusage.2.html).

```
No new programs should use vtimes(3).
getrusage(2) briefly discusses vtimes(3), so point the user there.
```

---

For detailed information about the `getrusage` function, please refer to the online documentation linked above.
