# `wmemcpy(3)` - Copy an Array of Wide-Characters

## NAME

`wmemcpy` - copy an array of wide-characters

## SYNOPSIS

```c
#include <wchar.h>

wchar_t *wmemcpy(wchar_t *restrict dest, const wchar_t *restrict src, size_t n);
```

## DESCRIPTION

The `wmemcpy` function is the wide-character equivalent of the `memcpy(3)` function. It copies `n` wide characters from the array starting at `src` to the array starting at `dest`.

The arrays may not overlap; use `wmemmove(3)` to copy between overlapping arrays.

The programmer must ensure that there is room for at least `n` wide characters at `dest`.

## RETURN VALUE

`wmemcpy` returns `dest`.

## ATTRIBUTES

- Thread safety: MT-Safe

## CONFORMING TO

POSIX.1-2001, POSIX.1-2008, C99.

## SEE ALSO

- `memcpy(3)` - Copy memory area.
- `wcscpy(3)` - Copy a wide-character string.
- `wmemmove(3)` - Copy wide characters between overlapping arrays.
- `wmempcpy(3)` - Copy an array of wide-characters and return a pointer to the end.

