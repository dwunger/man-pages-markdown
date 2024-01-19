# NAME
printf, fprintf, dprintf, sprintf, snprintf, vprintf, vfprintf, vdprintf,
vsprintf, vsnprintf - formatted output conversion

# SYNOPSIS
```c
#include <stdio.h>

int printf(const char *restrict format, ...);
int fprintf(FILE *restrict stream, const char *restrict format, ...);
int dprintf(int fd, const char *restrict format, ...);
int sprintf(char *restrict str, const char *restrict format, ...);
int snprintf(char *restrict str, size_t size, const char *restrict format, ...);

#include <stdarg.h>

int vprintf(const char *restrict format, va_list ap);
int vfprintf(FILE *restrict stream, const char *restrict format, va_list ap);
int vdprintf(int fd, const char *restrict format, va_list ap);
int vsprintf(char *restrict str, const char *restrict format, va_list ap);
int vsnprintf(char *restrict str, size_t size, const char *restrict format, va_list ap);
```

## Feature Test Macro Requirements for glibc
- `snprintf()`, `vsnprintf()`: `_XOPEN_SOURCE >= 500 || _ISOC99_SOURCE || _BSD_SOURCE`
- `dprintf()`, `vdprintf()`: 
    - Since glibc 2.10: `_POSIX_C_SOURCE >= 200809L`
    - Before glibc 2.10: `_GNU_SOURCE`

# DESCRIPTION
The functions in the `printf()` family produce output according to a format as described below. These functions are used for formatted output conversion. The functions are categorized as follows:

- `printf()`: Write output to `stdout`, the standard output stream.
- `fprintf()`: Write output to the given output stream.
- `dprintf()`: Output to a file descriptor.
- `sprintf()`: Write to a character string.
- `snprintf()`: Write to a character string with a specified size.

Additionally, there are functions with 'v' prefixes (e.g., `vprintf()`, `vfprintf()`) that are equivalent to their non-'v' counterparts but accept a `va_list` instead of variable arguments.

All of these functions use a format string that specifies how subsequent arguments are converted for output.

## Format of the format string
The format string consists of zero or more directives and ordinary characters. Directives start with the `%` character and specify how to convert subsequent arguments for output. The overall syntax of a conversion specification is: `%[$][flags][width][.precision][length modifier]conversion`.

- `$`: Specifies which argument is taken explicitly.
- Flags: Modify the output format.
- Width: Specifies a minimum field width.
- Precision: Specifies the number of digits or characters.
- Length modifier: Specifies the size of the argument.
- Conversion: Specifies the type of conversion to apply.

## Flag characters
Flag characters modify the output format. Some commonly used flag characters include:
- `#`: Alternate form.
- `0`: Zero padding.
- `-`: Left-justify.
- ` ` (space): A blank should be left before a positive number.
- `+`: Always place a sign (+ or -) before a number.

## Field width
Field width specifies a minimum field width for the converted value. It can be an optional decimal digit string or specified using `*` to get the width from the argument.

## Precision
Precision specifies the number of digits or characters for certain conversions. It can be specified with a period (`.`) followed by a digit string or using `*` to get the precision from the argument.

## Length modifier
The length modifier specifies the size of the argument. Common length modifiers include:
- `hh`: `signed char` or `unsigned char`.
- `h`: `short` or `unsigned short`.
- `l`: `long` or `unsigned long`.
- `ll`: `long long` or `unsigned long long`.
- `L`: `long double`.
- `j`: `intmax_t` or `uintmax_t`.
- `z`: `size_t` or `ssize_t`.
- `t`: `ptrdiff_t`.

## Conversion specifiers
Conversion specifiers define the type of conversion to be applied. Some commonly used conversion specifiers include:
- `d`, `i`: Signed decimal notation.
- `o`, `u`, `x`, `X`: Unsigned octal, decimal, or hexadecimal.
- `e`, `E`: Exponential notation.
- `f`, `F`: Decimal notation.
- `g`, `G`: Decimal or exponential notation.
- `a`, `A`: Hexadecimal floating-point.
- `c`: Character.
- `s`: String.
- `p`: Pointer.
- `n`: Number of characters written.

# RETURN VALUE
Upon successful return, these functions return the number of characters printed (excluding the null byte used to end output to strings).

The functions `snprintf()` and `vsnprintf()` do not write more than the specified size bytes

 (including the terminating null byte) to the string.

# ERRORS
These functions may fail if:
- An output error occurs.
- An encoding error occurs.

# EXAMPLES
```c
#include <stdio.h>

int main() {
    int value = 42;
    char buffer[50];
    
    // Use sprintf to format a string
    sprintf(buffer, "The answer is %d\n", value);
    
    // Print the formatted string
    printf("%s", buffer);
    
    return 0;
}
```

# SEE ALSO
`va_start`(3), `va_arg`(3), `va_copy`(3), `va_end`(3), `printf`(1), `wprintf`(3)
