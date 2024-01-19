# SCANF(3) GNU Linux Programmer's Manual
## NAME
`scanf`, `fscanf`, `sscanf`, `vscanf`, `vsscanf`, `vfscanf` - input format conversion
## SYNOPSIS
```c
#include <stdio.h>

int scanf(const char *restrict format, ...);
int fscanf(FILE *restrict stream, const char *restrict format, ...);
int sscanf(const char *restrict str, const char *restrict format, ...);

#include <stdarg.h>

int vscanf(const char *restrict format, va_list ap);
int vfscanf(FILE *restrict stream, const char *restrict format, va_list ap);
int vsscanf(const char *restrict str, const char *restrict format, va_list ap);
```
## DESCRIPTION
The `scanf` family of functions scans input according to `format` as described below. This format may contain conversion specifications; the results from such conversions, if any, are stored in the locations pointed to by the pointer arguments that follow `format`. Each pointer argument must be of a type that is appropriate for the value returned by the corresponding conversion specification.

If the number of conversion specifications in `format` exceeds the number of pointer arguments, the results are undefined. If the number of pointer arguments exceeds the number of conversion specifications, then the excess pointer arguments are evaluated but are otherwise ignored.

- `scanf` reads input from the standard input stream `stdin`.
- `fscanf` reads input from the stream pointer `stream`.
- `sscanf` reads its input from the character string pointed to by `str`.

The `vscanf`, `vfscanf`, and `vsscanf` functions are analogous to their respective counterparts but use a variable argument list of pointers.

The `format` string consists of a sequence of directives that describe how to process the sequence of input characters. If processing of a directive fails, no further input is read, and the function returns. A "failure" can be either an input failure, meaning that input characters were unavailable, or a matching failure, meaning that the input was inappropriate.

A directive can be:
- A sequence of white-space characters (space, tab, newline, etc.).
- An ordinary character (other than white space or `%`).
- A conversion specification, beginning with a `%` character.

Each conversion specification begins with either the character `%` or the character sequence `%n$` (see below for the distinction).

The format of a conversion specification is:
```
%[*][width][length]specifier
```

- An optional `*` indicates assignment suppression, where input is discarded.
- An optional `width` is a maximum field width.
- An optional `length` is a type modifier character.
- The `specifier` specifies the type of input conversion.

The conversion specifications in `format` correspond to successive pointer arguments. In the `%n$` form, `n` is a decimal integer that specifies which pointer argument to use. Mixing the two forms in the same format string is possible.

### Conversions

Type modifier characters:
- `h`: Indicates that the conversion will be one of `d`, `i`, `o`, `u`, `x`, `X`, or `n`, and the next pointer is a pointer to a `short` or `unsigned short`.
- `hh`: Similar to `h` but uses `signed char` or `unsigned char`.
- `j`: The next pointer is a pointer to `intmax_t` or `uintmax_t`.
- `l`: Indicates either that the conversion will be one of `d`, `i`, `o`, `u`, `x`, `X`, or `n`, and the next pointer is a pointer to a `long` or `unsigned long`, or that the conversion will be `e`, `f`, or `g`, and the next pointer is a pointer to `double`.
- `L`: Indicates that the conversion will be `e`, `f`, or `g` with the next pointer pointing to `long double` or the conversion will be `d`, `i`, `o`, `u`, or `x` with the next pointer pointing to `long long`.
- `q`: Equivalent to `L`. (This specifier does not exist in ANSI C.)
- `t`: Similar to `h` but uses `ptrdiff_t`.
- `z`: Similar to `h` but uses `size_t`.

Conversion specifiers:
- `%`: Matches a literal `%`.
- `d`: Matches an optionally signed decimal integer.
- `i`: Matches an optionally signed integer.
- `o`: Matches an unsigned octal integer.
- `u`: Matches an unsigned decimal integer.
- `x`, `X`: Matches an unsigned hexadecimal integer.
- `f`, `e`, `g`, `E`, `a` (C99): Matches an optionally signed floating-point number.
- `s`: Matches a sequence of non-white-space characters.
- `c`: Matches a sequence of characters with a specified length.
- `[set]`: Matches a non-empty sequence of characters from a specified set.
- `p`: Matches a pointer value.
- `n`: Stores the number of characters consumed from the input.

## RETURN VALUE
On success, these functions return the number of input items successfully matched and assigned. This can be fewer than provided for, or even zero, in the event of an early matching failure.

The value `EOF` is returned if the end of input is reached before either the first successful conversion or a matching failure occurs. `EOF` is also returned if a read error occurs, in which case the error indicator for the stream is set, and `errno` is set to indicate the error.

## ERRORS
- `EAGAIN`: The file descriptor underlying `stream` is marked nonblocking, and the read operation would block.
- `EBADF`: The file descriptor underlying `stream` is invalid or not open for reading.
- `EILSEQ`: Input byte sequence does not form a valid character.
- `EINTR`: The read operation was interrupted by a signal.
- `EINVAL`: Not enough arguments, or `format` is NULL.
- `ENOMEM`: Out of memory.
- `ERANGE`: The result of an integer conversion would exceed the size that can be stored in the corresponding integer type.

## ATTRIBUTES
- Thread safety: MT-Safe locale

## CONFORMING TO
The functions `fscanf`, `scanf`, and `sscanf` conform to C89 and C99 and POSIX.1-2001. These standards do not specify the `ERANGE` error.

## NOTES
### The 'a' Assignment-Allocation Modifier
Originally, the GNU C library supported dynamic allocation for string inputs (as a nonstandard extension) via the `a` character. This feature is present at least as far back as glibc 2.0. To use dynamic allocation, specify `m` as a length modifier (e.g., `%ms` or `%m[range]`). The caller must `free` the returned string when it is no longer required.

Support for the `m` modifier was added to glibc starting with version 2.7, and new programs should use that modifier instead of `a`.

### Bugs
All functions are fully C89 conformant but provide additional specifiers (`q` and `a`) and an additional behavior of the

 `L` and `l` specifiers. Some combinations of type modifiers and conversion specifiers defined by ANSI C do not make sense (e.g., `%Ld`).

## EXAMPLES
To use the dynamic allocation conversion specifier, specify `m` as a length modifier (e.g., `%ms`). The caller must `free` the returned string, as in the following example:
```c
char *p;
int n;

errno = 0;
n = scanf("%m[a-z]", &p);
if (n == 1) {
    printf("read: %s\n", p);
    free(p);
} else if (errno != 0) {
    perror("scanf");
} else {
    fprintf(stderr, "No matching characters\n");
}
```

## SEE ALSO
`getc(3)`, `printf(3)`, `setlocale(3)`, `strtod(3)`, `strtol(3)`, `strtoul(3)`
