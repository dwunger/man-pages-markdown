---
title: sigpause - Atomically Release Blocked Signals and Wait for Interrupt
date: 2021-03-22
categories: [Linux, Linux Programmer's Manual]
---

# NAME

sigpause - atomically release blocked signals and wait for interrupt

# SYNOPSIS

```c
#include <signal.h>

int sigpause(int sigmask);  /* BSD (but see NOTES) */
int sigpause(int sig);      /* System V / UNIX 95 */
```

# DESCRIPTION

**Don't use this function. Use [sigsuspend(2)](https://man7.org/linux/man-pages/man2/sigsuspend.2.html) instead.**

The function `sigpause()` is designed to wait for some signal. It changes the process's signal mask (set of blocked signals) and then waits for a signal to arrive. Upon arrival of a signal, the original signal mask is restored.

# RETURN VALUE

If `sigpause()` returns, it was interrupted by a signal, and the return value is -1 with `errno` set to `EINTR`.

# ATTRIBUTES

- Interface: `sigpause()`
- Thread safety: MT-Safe

# CONFORMING TO

The System V version of `sigpause()` is standardized in POSIX.1-2001. It is also specified in POSIX.1-2008, where it is marked obsolete.

# NOTES

## History

The classical BSD version of this function appeared in 4.2BSD. It sets the process's signal mask to `sigmask`. UNIX 95 standardized the incompatible System V version of this function, which removes only the specified signal `sig` from the process's signal mask.

The unfortunate situation with two incompatible functions with the same name was solved by the [sigsuspend(2)](https://man7.org/linux/man-pages/man2/sigsuspend.2.html) function, which takes a `sigset_t*` argument (instead of an `int`).

## Linux notes

On Linux, this routine is a system call only on the Sparc (sparc64) architecture.

Glibc uses the BSD version if the `_BSD_SOURCE` feature test macro is defined and none of `_POSIX_SOURCE`, `_POSIX_C_SOURCE`, `_XOPEN_SOURCE`, `_GNU_SOURCE`, or `_SVID_SOURCE` is defined. Otherwise, the System V version is used, and feature test macros must be defined as specified in the manual to obtain the declaration.

Since glibc 2.19, only the System V version is exposed by `<signal.h>`; applications that formerly used the BSD `sigpause()` should be amended to use [sigsuspend(2)](https://man7.org/linux/man-pages/man2/sigsuspend.2.html).

# SEE ALSO

[sigsuspend(2)](https://man7.org/linux/man-pages/man2/sigsuspend.2.html), [kill(2)](https://man7.org/linux/man-pages/man2/kill.2.html), [sigaction(2)](https://man7.org/linux/man-pages/man2/sigaction.2.html), [sigprocmask(2)](https://man7.org/linux/man-pages/man2/sigprocmask.2.html), [sigblock(3)](https://man7.org/linux/man-pages/man3/sigblock.3.html), [sigvec(3)](https://man7.org/linux/man-pages/man3/sigvec.3.html), [feature_test_macros(7)](https://man7.org/linux/man-pages/man7/feature_test_macros.7.html)
