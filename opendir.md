# `opendir(3)` Linux Programmer's Manual
## NAME
`opendir`, `fdopendir` - open a directory
## SYNOPSIS
```c
#include <sys/types.h>
#include <dirent.h>

DIR *opendir(const char *name);
DIR *fdopendir(int fd);
```
## DESCRIPTION
The `opendir()` function opens a directory stream corresponding to the directory `name`, and returns a pointer to the directory stream. The stream is positioned at the first entry in the directory.

The `fdopendir()` function is like `opendir()`, but returns a directory stream for the directory referred to by the open file descriptor `fd`. After a successful call to `fdopendir()`, `fd` is used internally by the implementation and should not otherwise be used by the application.
## RETURN VALUE
The `opendir()` and `fdopendir()` functions return a pointer to the directory stream. On error, NULL is returned, and `errno` is set to indicate the error.
## ERRORS
- `EACCES`: Permission denied.
- `EBADF`: `fd` is not a valid file descriptor opened for reading.
- `EMFILE`: The per-process limit on the number of open file descriptors has been reached.
- `ENFILE`: The system-wide limit on the total number of open files has been reached.
- `ENOENT`: Directory does not exist, or `name` is an empty string.
- `ENOMEM`: Insufficient memory to complete the operation.
- `ENOTDIR`: `name` is not a directory.
## VERSIONS
`fdopendir()` is available in glibc since version 2.4.
## ATTRIBUTES
For an explanation of the terms used in this section, see [attributes(7)](http://man7.org/linux/man-pages/man7/attributes.7.html).
## CONFORMING TO
`opendir()` is present on SVr4, 4.3BSD, and specified in POSIX.1-2001. `fdopendir()` is specified in POSIX.1-2008.
## NOTES
- Filename entries can be read from a directory stream using `readdir(3)`.
- The underlying file descriptor of the directory stream can be obtained using `dirfd(3)`.
- The `opendir()` function sets the close-on-exec flag for the file descriptor underlying the `DIR*`. The `fdopendir()` function leaves the setting of the close-on-exec flag unchanged for the file descriptor `fd`. POSIX.1-200x leaves it unspecified whether a successful call to `fdopendir()` will set the close-on-exec flag for the file descriptor `fd`.
## SEE ALSO
- [`open(2)`](http://man7.org/linux/man-pages/man2/open.2.html)
- [`closedir(3)`](http://man7.org/linux/man-pages/man3/closedir.3.html)
- [`dirfd(3)`](http://man7.org/linux/man-pages/man3/dirfd.3.html)
- [`readdir(3)`](http://man7.org/linux/man-pages/man3/readdir.3.html)
- [`rewinddir(3)`](http://man7.org/linux/man-pages/man3/rewinddir.3.html)
- [`scandir(3)`](http://man7.org/linux/man-pages/man3/scandir.3.html)
- [`seekdir(3)`](http://man7.org/linux/man-pages/man3/seekdir.3.html)
- [`telldir(3)`](http://man7.org/linux/man-pages/man3/telldir.3.html)
