# STRDUP(3) Linux Programmer's Manual STRDUP(3)

## NAME

**strdup**, **strndup**, **strdupa**, **strndupa** - duplicate a string

## SYNOPSIS

```c
#include <string.h>

char *strdup(const char *s);
char *strndup(const char *s, size_t n);
char *strdupa(const char *s);
char *strndupa(const char *s, size_t n);
```

## DESCRIPTION

The **strdup()** function returns a pointer to a new string which is a duplicate of the string **s**. Memory for the new string is obtained with **malloc(3)** and can be freed with **free(3)**.

The **strndup()** function is similar, but copies at most **n** bytes. If **s** is longer than **n**, only **n** bytes are copied, and a terminating null byte ('\0') is added.

**strdupa()** and **strndupa()** are similar, but use **alloca(3)** to allocate the buffer. They are available only when using the GNU GCC suite and suffer from the same limitations described in **alloca(3)**.

## RETURN VALUE

On success, the **strdup()** function returns a pointer to the duplicated string. It returns NULL if insufficient memory was available, with **errno** set to indicate the error.

## ERRORS

- **ENOMEM**: Insufficient memory available to allocate a duplicate string.

## ATTRIBUTES

For an explanation of the terms used in this section, see **attributes(7)**.

| Interface | Attribute     | Value   |
|-----------|---------------|---------|
| **strdup()**, **strndup()**, **strdupa()**, **strndupa()** | Thread safety | MT-Safe |

## CONFORMING TO

- **strdup()**: SVr4, 4.3BSD, POSIX.1-2001.
- **strndup()**: POSIX.1-2008.
- **strdupa()** and **strndupa()**: GNU extensions.

## SEE ALSO

**alloca(3)**, **calloc(3)**, **free(3)**, **malloc(3)**, **realloc(3)**, **string(3)**, **wcsdup(3)**
