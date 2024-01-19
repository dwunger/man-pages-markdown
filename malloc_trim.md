# `malloc_trim(3)` Linux Programmer's Manual
## NAME
`malloc_trim` - release free memory from the heap
## SYNOPSIS
```c
#include <malloc.h>

int malloc_trim(size_t pad);
```
## DESCRIPTION
The `malloc_trim()` function attempts to release free memory from the heap (by calling `sbrk(2)` or `madvise(2)` with suitable arguments).

The `pad` argument specifies the amount of free space to leave untrimmed at the top of the heap. If this argument is 0, only the minimum amount of memory is maintained at the top of the heap (i.e., one page or less). A nonzero argument can be used to maintain some trailing space at the top of the heap to allow future allocations to be made without having to extend the heap with `sbrk(2)`.

## RETURN VALUE
The `malloc_trim()` function returns 1 if memory was actually released back to the system, or 0 if it was not possible to release any memory.

## ATTRIBUTES
For an explanation of the terms used in this section, see [attributes(7)](http://man7.org/linux/man-pages/man7/attributes.7.html).
```plaintext
Interface  Attribute       Value
malloc_trim() Thread safety MT-Safe
```
## CONFORMING TO
This function is a GNU extension.

## NOTES
Only the main heap (using `sbrk(2)`) honors the `pad` argument; thread heaps do not.

Since glibc 2.8, this function frees memory in all arenas and in all chunks with whole free pages.

Before glibc 2.8, this function only freed memory at the top of the heap in the main arena.

## SEE ALSO
- [`sbrk(2)`](http://man7.org/linux/man-pages/man2/sbrk.2.html)
- [`malloc(3)`](http://man7.org/linux/man-pages/man3/malloc.3.html)
- [`mallopt(3)`](http://man7.org/linux/man-pages/man3/mallopt.3.html)
