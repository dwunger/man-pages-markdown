---
title: FOPEN 3
section: 3
date: 2021-03-22
source: Linux
manual: Linux Programmer's Manual
---

# NAME

**fopen**, **fdopen**, **freopen** - stream open functions

# SYNOPSIS

```c
#include <stdio.h>

FILE *fopen(const char *restrict pathname, const char *restrict mode);
FILE *fdopen(int fd, const char *mode);
FILE *freopen(const char *restrict pathname, const char *restrict mode, FILE *restrict stream);
```

# DESCRIPTION

The **fopen()** function opens the file whose name is the string pointed to by `pathname` and associates a stream with it.

The argument `mode` points to a string beginning with one of the following sequences (possibly followed by additional characters, as described below):

- `r`: Open text file for reading. The stream is positioned at the beginning of the file.
- `r+`: Open for reading and writing. The stream is positioned at the beginning of the file.
- `w`: Truncate file to zero length or create a text file for writing. The stream is positioned at the beginning of the file.
- `w+`: Open for reading and writing. The file is created if it does not exist, otherwise, it is truncated. The stream is positioned at the beginning of the file.
- `a`: Open for appending (writing at the end of the file). The file is created if it does not exist. The stream is positioned at the end of the file.
- `a+`: Open for reading and appending (writing at the end of the file). The file is created if it does not exist. Output is always appended to the end of the file. The initial read position varies between different systems.

The `mode` string can also include the letter "b" either as a last character or as a character between the characters in any of the two-character strings described above. This is strictly for compatibility with C89 and has no effect on POSIX conforming systems, including Linux.

Any created file will have the mode `S_IRUSR | S_IWUSR | S_IRGRP | S_IWGRP | S_IROTH | S_IWOTH` (0666), as modified by the process's umask value.

Reads and writes may be intermixed on read/write streams in any order. It is good practice to use an fseek or fgetpos operation between write and read operations on such a stream to ensure proper file positioning.

Opening a file in append mode ("a" as the first character of mode) causes all subsequent write operations to this stream to occur at the end of the file. The file descriptor associated with the stream is opened as if by a call to open(2) with specific flags depending on the mode.

## fdopen()

The **fdopen()** function associates a stream with the existing file descriptor `fd`. The `mode` of the stream must be compatible with the mode of the file descriptor. The file position indicator of the new stream is set to that belonging to `fd`, and the error and end-of-file indicators are cleared. If `mode` is "w" or "w+", it does not cause truncation of the file. The file descriptor is not duplicated, and it will be closed when the stream created by **fdopen()** is closed.

## freopen()

The **freopen()** function opens the file whose name is the string pointed to by `pathname` and associates the stream pointed to by `stream` with it. The original stream (if it exists) is closed. The `mode` argument is used just as in **fopen()**.

If the `pathname` argument is a null pointer, **freopen()** changes the mode of the stream to that specified in `mode`. The behavior of this change of mode is implementation-defined.

# RETURN VALUE

Upon successful completion, **fopen()**, **fdopen()**, and **freopen()** return a `FILE` pointer. Otherwise, NULL is returned, and `errno` is set to indicate the error.

# ERRORS

- **EINVAL**: The `mode` provided to **fopen()**, **fdopen()**, or **freopen()** was invalid.
- **fopen()**: It may also fail and set `errno` for any of the errors specified for the routine open(2).
- **fdopen()**: It may also fail and set `errno` for any of the errors specified for the routine fcntl(2).
- **freopen()**: It may also fail and set `errno` for any of the errors specified for the routines open(2), fclose(3), and fflush(3).

# ATTRIBUTES

- Interface: MT-Safe

# CONFORMING TO

- **fopen()**, **freopen()**: POSIX.1-2001, POSIX.1-2008, C89, C99.
- **fdopen()**: POSIX.1-2001, POSIX.1-2008.

# NOTES

## Glibc notes

The GNU C library allows the following extensions for the `mode` string specified in `mode`:

- `c` (since glibc 2.3.3): Do not make the open operation or subsequent read and write operations thread cancellation points. This flag is ignored for **fdopen()**.
- `e` (since glibc 2.7): Open the file with the `O_CLOEXEC` flag. This flag is ignored for **fdopen()**.
- `m` (since glibc 2.3): Attempt to access the file using **mmap(2)** rather than I/O system calls (read(2), write(2)). Currently, use of **mmap(2)** is attempted only for a file opened for reading.
- `x`: Open the file exclusively (like the `O_EXCL` flag of open(2)). If the file already exists, **fopen()** fails, and sets `errno` to `EEXIST`. This flag is ignored for **fdopen()**.

In addition to the above characters, **fopen()** and **freopen()** support the following syntax in `mode`:

- `,ccs=` string: The given `string` is taken as the name of a coded character set, and the stream is marked as wide-oriented. If the `,ccs=` string syntax is not specified, then the wide-orientation of the stream is determined by the first file operation.

# SEE ALSO

- [open(2)](https://man7.org/linux/man-pages/man2/open.2.html)
- [fclose(3)](https://man7.org/linux/man-pages/man3/fclose.3.html)
- [fileno(3)](https://man7.org/linux/man-pages/man3/fileno.3.html)
- [fmemopen(3)](https://man7.org/linux/man-pages/man3/fmemopen.3.html)
- [fopencookie(3)](https://man7.org/linux/man-pages/man3/fopencookie.3.html)
- [open_memstream(3)](https://man7.org/linux/man-pages/man3/open_memstream.3.html)
