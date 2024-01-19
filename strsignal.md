# STRSIGNAL(3) Linux Programmer's Manual STRSIGNAL(3)

## NAME

**strsignal**, **sigabbrev_np**, **sigdescr_np**, **sys_siglist** - return string describing signal

## SYNOPSIS

```c
#include <string.h>

char *strsignal(int sig);
const char *sigdescr_np(int sig);
const char *sigabbrev_np(int sig);

extern const char *const sys_siglist[];
```

## DESCRIPTION

The **strsignal()** function returns a string describing the signal number passed in the argument **sig**. The string can be used only until the next call to **strsignal()**. The string returned by **strsignal()** is localized according to the **LC_MESSAGES** category in the current locale.

The **sigdescr_np()** function returns a string describing the signal number passed in the argument **sig**. Unlike **strsignal()**, this string is not influenced by the current locale.

The **sigabbrev_np()** function returns the abbreviated name of the signal, **sig**. For example, given the value **SIGINT**, it returns the string "INT".

The (deprecated) array **sys_siglist** holds the signal description strings indexed by signal number. The **strsignal()** or the **sigdescr_np()** function should be used instead of this array; see also VERSIONS.

## RETURN VALUE

The **strsignal()** function returns the appropriate description string, or an unknown signal message if the signal number is invalid. On some systems (but not on Linux), NULL may instead be returned for an invalid signal number.

The **sigdescr_np()** and **sigabbrev_np()** functions return the appropriate description string. The returned string is statically allocated and valid for the lifetime of the program. These functions return NULL for an invalid signal number.

## VERSIONS

**sigdescr_np()** and **sigabbrev_np()** first appeared in glibc 2.32.

Starting with version 2.32, the **sys_siglist** symbol is no longer exported by glibc.

## ATTRIBUTES

For an explanation of the terms used in this section, see **attributes(7)**.

| Interface | Attribute | Value |
|----------------|---------------|-----------|
| **strsignal()** | Thread safety | MT-Unsafe race:strsignal locale |
| **sigdescr_np()**, **sigabbrev_np()** | Thread safety | MT-Safe |

## CONFORMING TO

**strsignal()**: POSIX.1-2008. Present on Solaris and the BSDs.

**sigdescr_np()** and **sigabbrev_np()** are GNU extensions.

**sys_siglist** is nonstandard but present on many other systems.

## NOTES

**sigdescr_np()** and **sigabbrev_np()** are thread-safe and async-signal-safe.

## SEE ALSO

**psignal(3)**, **strerror(3)**
