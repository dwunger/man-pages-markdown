---
title: remove - Linux Programmer's Manual
date: 2021-03-22
categories: [Linux, Manual]
---

# NAME

remove - remove a file or directory

# SYNOPSIS

```c
#include <stdio.h>

int remove(const char *pathname);
```

# DESCRIPTION

The `remove()` function deletes a name from the filesystem. It calls `unlink(2)` for files and `rmdir(2)` for directories.

If the removed name was the last link to a file and no processes have the file open, the file is deleted, and the space it was using is made available for reuse.

If the name was the last link to a file, but any processes still have the file open, the file will remain in existence until the last file descriptor referring to it is closed.

If the name referred to a symbolic link, the link is removed.

If the name referred to a socket, FIFO, or device, the name is removed, but processes that have the object open may continue to use it.

# RETURN VALUE

On success, zero is returned. On error, -1 is returned, and `errno` is set to indicate the error.

# ERRORS

The errors that occur are those for `unlink(2)` and `rmdir(2)`.

# ATTRIBUTES

The `remove()` function is thread-safe.

# CONFORMING TO

POSIX.1-2001, POSIX.1-2008, C89, C99, 4.3BSD.

# BUGS

Infelicities in the protocol underlying NFS can cause the unexpected disappearance of files that are still being used.

# SEE ALSO

- `rm(1)`
- `unlink(1)`
- `link(2)`
- `mknod(2)`
- `open(2)`
- `rename(2)`
- `rmdir(2)`
- `unlink(2)`
- `mkfifo(3)`
- `symlink(7)`
