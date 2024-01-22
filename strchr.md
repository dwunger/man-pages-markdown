# STRCHR(3) Linux Programmer's Manual STRCHR(3)

## NAME

**strchr**, **strrchr**, **strchrnul** - locate character in string

## SYNOPSIS

```c
#include <string.h>

char *strchr(const char *s, int c);
char *strrchr(const char *s, int c);

#define _GNU_SOURCE   /* Required for strchrnul */
#include <string.h>

char *strchrnul(const char *s, int c);
```

## DESCRIPTION

The **strchr()** function finds the first occurrence of the character **c** in the string **s**.

The **strrchr()** function finds the last occurrence of the character **c** in the string **s**.

The **strchrnul()** function, a GNU extension, operates like **strchr()** but returns a pointer to the null byte at the end of **s** if **c** is not found, instead of returning NULL.

These functions interpret the character **c** as an unsigned char and search for a byte in the string. They are not suitable for wide or multibyte characters.

## EXAMPLES

```c
#include <stdio.h>
#include <string.h>

int main() {
    const char *s = "example.com";
    char *ptr;

    // Using strchr to find the first occurrence of 'e'
    ptr = strchr(s, 'e');
    printf("First occurrence of 'e': %s\n", ptr);

    // Using strrchr to find the last occurrence of 'e'
    ptr = strrchr(s, 'e');
    printf("Last occurrence of 'e': %s\n", ptr);

    // Using strchrnul to find 'x', which is not in the string
    ptr = strchrnul(s, 'x');
    printf("Result of strchrnul with 'x': %s\n", (*ptr) ? ptr : "(null byte)");

    return 0;
}
```

## RETURN VALUE

For **strchr()** and **strrchr()**: Returns a pointer to the found character, or NULL if not found. The search includes the terminating null byte, so searching for `'\0'` will always return a pointer to the string's terminator.

For **strchrnul()**: Returns a pointer to the found character, or to the null byte at the end of the string if the character is not found.

## VERSIONS

**strchrnul()** was introduced in glibc version 2.1.1.

## ATTRIBUTES

See **attributes(7)** for descriptions of the following attributes:

| Interface          | Attribute     | Value          |
|--------------------|---------------|----------------|
| **strchr()**, **strrchr()**, **strchrnul()** | Thread safety | MT-Safe        |

## CONFORMING TO

**strchr()**, **strrchr()**: Conform to POSIX.1-2001, POSIX.1-2008, C89, C99, SVr4, 4.3BSD.

**strchrnul()** is a GNU extension.

## SEE ALSO

- **[index(3)](https://man7.org/linux/man-pages/man3/index.3.html)**
- **[memchr(3)](https://man7.org/linux/man-pages/man3/memchr.3.html)**
- **[rindex(3)](https://man7.org/linux/man-pages/man3/rindex.3.html)**
- **[string(3)](https://man7.org/linux/man-pages/man3/string.3.html)**
- **[strlen(3)](https://man7.org/linux/man-pages/man3/strlen.3.html)**
- **[strpbrk(3)](https://man7.org/linux/man-pages/man3/strpbrk.3.html)**
- **[strsep(3)](https://man7.org/linux/man-pages/man3/strsep.3.html)**
- **[strspn(3)](https://man7.org/linux/man-pages/man3/strspn.3.html)**
- **[strstr(3)](https://man7.org/linux/man-pages/man3/strstr.3.html)**
- **[strtok(3)](https://man7.org/linux/man-pages/man3/strtok.3.html)**
- **[wcschr(3)](https://man7.org/linux/man-pages/man3/wcschr.3.html)**
- **[wcsrchr(3)](https://man7.org/linux/man-pages/man3/wcsrchr.3.html)**
