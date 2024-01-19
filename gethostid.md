---
title: GETHOSTID 3
section: 3
date: 2021-03-22
source: Linux Programmer's Manual
---

# NAME

**gethostid**, **sethostid** - get or set the unique identifier of the current host

# SYNOPSIS

```c
#include <unistd.h>

long gethostid(void);
int sethostid(long hostid);
```

# DESCRIPTION

**gethostid()** and **sethostid()** respectively get or set a unique 32-bit identifier for the current machine. The 32-bit identifier was intended to be unique among all UNIX systems in existence. This normally resembles the Internet address for the local machine, as returned by **gethostbyname(3)**, and thus usually never needs to be set.

The **sethostid()** call is restricted to the superuser.

# RETURN VALUE

**gethostid()** returns the 32-bit identifier for the current host as set by **sethostid()**.

On success, **sethostid()** returns 0; on error, -1 is returned, and **errno** is set to indicate the error.

# ERRORS

**sethostid()** can fail with the following errors:

- **EACCES**: The caller did not have permission to write to the file used to store the host ID.
- **EPERM**: The calling process's effective user or group ID is not the same as its corresponding real ID.

# ATTRIBUTES

- Interface: Thread safety
  - **gethostid()**: MT-Safe hostid env locale
  - **sethostid()**: MT-Unsafe const:hostid

# CONFORMING TO

4.2BSD; these functions were dropped in 4.4BSD. SVr4 includes **gethostid()** but not **sethostid()**. POSIX.1-2001 and POSIX.1-2008 specify **gethostid()** but not **sethostid()**.

# NOTES

In the glibc implementation, the **hostid** is stored in the file **/etc/hostid**. In glibc versions before 2.2, the file **/var/adm/hostid** was used.

In the glibc implementation, if **gethostid()** cannot open the file containing the host ID, then it obtains the hostname using **gethostname(2)**, passes that hostname to **gethostbyname_r(3)** in order to obtain the host's IPv4 address, and returns a value obtained by bit-twiddling the IPv4 address. (This value may not be unique.)

# BUGS

It is impossible to ensure that the identifier is globally unique.

# SEE ALSO

**hostid(1)**, **gethostbyname(3)**
