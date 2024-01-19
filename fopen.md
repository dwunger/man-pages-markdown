# FOPEN(3) Linux Programmer's Manual
## NAME
fopen, fdopen, freopen - stream open functions
## SYNOPSIS
```c
#include <stdio.h>

FILE *fopen(const char *restrict pathname, const char *restrict mode);
FILE *fdopen(int fd, const char *mode);
FILE *freopen(const char *restrict pathname, const char *restrict mode, FILE *restrict stream);
```

## DESCRIPTION
The `fopen()` function opens the file whose name is the string pointed to by `pathname` and associates a stream with it. The argument `mode` points to a string beginning with one of the following sequences:

- `r`: Open text file for reading. The stream is positioned at the beginning of the file.
- `r+`: Open for reading and writing. The stream is positioned at the beginning of the file.
- `w`: Truncate file to zero length or create a text file for writing. The stream is positioned at the beginning of the file.
- `w+`: Open for reading and writing. The file is created if it does not exist, otherwise, it is truncated. The stream is positioned at the beginning of the file.
- `a`: Open for appending (writing at the end of the file). The file is created if it does not exist. The stream is positioned at the end of the file.
- `a+`: Open for reading and appending (writing at the end of the file). The file is created if it does not exist. Output is always appended to the end of the file.

The `mode` string can also include the letter 'b' either as a last character or between the characters in any of the two-character strings described above. This 'b' has no effect on POSIX conforming systems, including Linux.

Any created file will have the mode `S_IRUSR | S_IWUSR | S_IRGRP | S_IWGRP | S_IROTH | S_IWOTH` (0666), as modified by the process's umask value.

Reads and writes may be intermixed on read/write streams in any order. It is good practice to put an `fseek(3)` or `fgetpos(3)` operation between write and read operations on such a stream to meet ANSI C requirements.

Opening a file in append mode ('a' as the first character of `mode`) causes all subsequent write operations to occur at the end-of-file.

The file descriptor associated with the stream is opened as if by a call to `open(2)` with specific flags, depending on the mode.

### fdopen()
The `fdopen()` function associates a stream with the existing file descriptor `fd`. The `mode` of the stream must be compatible with the mode of the file descriptor. The file position indicator of the new stream is set to that of `fd`, and the error and end-of-file indicators are cleared.

### freopen()
The `freopen()` function opens the file whose name is the string pointed to by `pathname` and associates the stream pointed to by `stream` with it. The original stream (if it exists) is closed, and the `mode` argument is used similarly to `fopen()`.

If the `pathname` argument is a null pointer, `freopen()` changes the mode of the stream to that specified in `mode`, reopening the pathname associated with the stream.

## RETURN VALUE
Upon successful completion, `fopen()`, `fdopen()`, and `freopen()` return a `FILE` pointer. Otherwise, NULL is returned, and `errno` is set to indicate the error.

## ERRORS
- `EINVAL`: The `mode` provided to `fopen()`, `fdopen()`, or `freopen()` was invalid.
- The functions may also fail and set `errno` for errors specified in `malloc(3)` and `open(2)` for `fopen()`, `fcntl(2)` for `fdopen()`, and `open(2)`, `fclose(3)`, and `fflush(3)` for `freopen()`.

## ATTRIBUTES
For an explanation of the terms used in this section, see `attributes(7)`.

```
Interface    Attribute    Value
fopen(), fdopen(), freopen()    Thread safety    MT-Safe
```

## CONFORMING TO
- `fopen()`, `freopen()`: POSIX.1-2001, POSIX.1-2008, C89, C99.
- `fdopen()`: POSIX.1-2001, POSIX.1-2008.

## NOTES
### Glibc notes
The GNU C library allows the following extensions for the string specified in `mode`:
- `c`: Do not make the open operation or subsequent read and write operations thread cancellation points (ignored for `fdopen()`).
- `e`: Open the file with the `O_CLOEXEC` flag (ignored for `fdopen()`).
- `m`: Attempt to access the file using `mmap(2)` rather than I/O system calls (ignored for `fdopen()`).
- `x`: Open the file exclusively (ignored for `fdopen()`).

Additionally, `fopen()` and `freopen()` support the following syntax in `mode`:
- `,ccs=string`: The given `string` is taken as the name of a coded character set, and the stream is marked as wide-oriented.

## BUGS
When parsing for individual flag characters in `mode`, the glibc implementation of `fopen()` and `freopen()` limits the number of characters examined in `mode` to 7 (or, in glibc versions before 2.14, to 6).

## SEE ALSO
`open(2)`, `fclose(3)`, `fileno(3)`, `fmemopen(3)`, `fopencookie(3)`, `open_memstream(3)`
