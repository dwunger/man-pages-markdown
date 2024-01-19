# `wcsdup(3)` - Duplicate a Wide-Character String

## NAME

`wcsdup` - Duplicate a wide-character string

## SYNOPSIS

```c
#include <wchar.h>

wchar_t *wcsdup(const wchar_t *s);
```

## DESCRIPTION

The `wcsdup` function is the wide-character equivalent of the `strdup(3)` function. It allocates and returns a new wide-character string whose initial contents is a duplicate of the wide-character string pointed to by `s`.

Memory for the new wide-character string is obtained with `malloc(3)`, and should be freed with `free(3)`.

## RETURN VALUE

On success, `wcsdup` returns a pointer to the new wide-character string. On error, it returns NULL, with `errno` set to indicate the error.

## ERRORS

- `ENOMEM`: Insufficient memory available to allocate the duplicate string.

## ATTRIBUTES

- Thread safety: MT-Safe

## CONFORMING TO

POSIX.1-2008. This function is not specified in POSIX.1-2001 and is not widely available on other systems.

## SEE ALSO

- `strdup(3)` - Duplicate a string.
- `wcscpy(3)` - Copy a wide-character string.

