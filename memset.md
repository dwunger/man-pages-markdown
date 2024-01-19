# `memset(3)` Linux Programmer's Manual
## NAME
`memset` - fill memory with a constant byte
## SYNOPSIS
```c
#include <string.h>

void *memset(void *s, int c, size_t n);
```
## DESCRIPTION
The `memset()` function fills the first `n` bytes of the memory area pointed to by `s` with the constant byte `c`.
## RETURN VALUE
The `memset()` function returns a pointer to the memory area `s`.
## ATTRIBUTES
For an explanation of the terms used in this section, see [attributes(7)](http://man7.org/linux/man-pages/man7/attributes.7.html).
```plaintext
Interface            Attribute     Value
memset()             Thread safety MT-Safe
```
## CONFORMING TO
POSIX.1-2001, POSIX.1-2008, C89, C99, SVr4, 4.3BSD.
## SEE ALSO
- [`bstring(3)`](http://man7.org/linux/man-pages/man3/bstring.3.html)
- [`bzero(3)`](http://man7.org/linux/man-pages/man3/bzero.3.html)
- [`swab(3)`](http://man7.org/linux/man-pages/man3/swab.3.html)
- [`wmemset(3)`](http://man7.org/linux/man-pages/man3/wmemset.3.html)
