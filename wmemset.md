# WMEMSET(3) GNU Library Functions Manual WMEMSET(3)

## NAME

**wmemset** - fill an array of wide-characters with a constant wide character

## SYNOPSIS

```c
#include <wchar.h>

wchar_t *wmemset(wchar_t *wcs, wchar_t wc, size_t n);
```

## DESCRIPTION

The **wmemset()** function is the wide-character equivalent of the **memset(3)** function. It fills the array of **n** wide-characters starting at **wcs** with **n** copies of the wide character **wc**.

## RETURN VALUE

**wmemset()** returns **wcs**.

## ATTRIBUTES

For an explanation of the terms used in this section, see **attributes(7)**.

- **Thread safety**: MT-Safe

## CONFORMING TO

POSIX.1-2001, POSIX.1-2008, C99.

## SEE ALSO

**memset(3)**
