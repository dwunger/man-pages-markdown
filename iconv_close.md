---
title: ICONV_CLOSE 3
section: 3
date: 2021-03-22
source: Linux Programmer's Manual
---

# NAME

**iconv_close** - deallocate descriptor for character set conversion

# SYNOPSIS

```c
#include <iconv.h>

int iconv_close(iconv_t cd);
```

# DESCRIPTION

The **iconv_close()** function deallocates a conversion descriptor **cd** previously allocated using **iconv_open(3)**.

# RETURN VALUE

On success, **iconv_close()** returns 0; otherwise, it returns -1 and sets **errno** to indicate the error.

# VERSIONS

This function is available in glibc since version 2.1.

# ATTRIBUTES

In the above table, **MT-Safe** for **iconv_close()** signifies that this function is thread-safe.

# CONFORMING TO

POSIX.1-2001, POSIX.1-2008, SUSv2.

# SEE ALSO

**iconv(3)**, **iconv_open(3)**
 