---
title: FSEEKO 3
section: 3
date: 2021-03-22
source: Linux Programmer's Manual
---

# NAME

**fseeko**, **ftello** - seek to or report file position

# SYNOPSIS

```c
#include <stdio.h>

int fseeko(FILE *stream, off_t offset, int whence);
off_t ftello(FILE *stream);
```

# DESCRIPTION

The **fseeko()** and **ftello()** functions are identical to **fseek(3)** and **ftell(3)** (see **fseek(3)**), respectively, except that the **offset** argument of **fseeko()** and the return value of **ftello()** is of type **off_t** instead of **long**.

On some architectures, both **off_t** and **long** are 32-bit types, but defining **_FILE_OFFSET_BITS** with the value 64 (before including any header files) will turn **off_t** into a 64-bit type.

# RETURN VALUE

On successful completion, **fseeko()** returns 0, while **ftello()** returns the current offset. Otherwise, -1 is returned, and **errno** is set to indicate the error.

# ERRORS

See the ERRORS in **fseek(3)**.

# VERSIONS

These functions are available under glibc since version 2.1.

# ATTRIBUTES

- Interface: MT-Safe

# CONFORMING TO

POSIX.1-2001, POSIX.1-2008, SUSv2.

# NOTES

The declarations of these functions can also be obtained by defining the obsolete **_LARGEFILE_SOURCE** feature test macro.

# SEE ALSO

**fseek(3)**
