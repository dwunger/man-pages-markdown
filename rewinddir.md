---
title: rewinddir - Linux Programmer's Manual
date: 2021-03-22
categories: [Linux, Manual]
---

# NAME

rewinddir - reset directory stream

# SYNOPSIS

```c
#include <sys/types.h>
#include <dirent.h>

void rewinddir(DIR *dirp);
```

# DESCRIPTION

The `rewinddir()` function resets the position of the directory stream `dirp` to the beginning of the directory.

# RETURN VALUE

The `rewinddir()` function returns no value.

# ATTRIBUTES

The `rewinddir()` function is thread-safe.

# CONFORMING TO

POSIX.1-2001, POSIX.1-2008, SVr4, 4.3BSD.

# SEE ALSO

- `closedir(3)`
- `opendir(3)`
- `readdir(3)`
- `scandir(3)`
- `seekdir(3)`
- `telldir(3)`
