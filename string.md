```markdown
# string.h(0p) - Linux manual page

## PROLOG
This manual page is part of the POSIX Programmer's Manual. The Linux
implementation of this interface may differ (consult the corresponding Linux
manual page for details of Linux behavior), or the interface may not be
implemented on Linux.

## NAME
string.h — string operations

## SYNOPSIS
```c
#include <string.h>
```

## DESCRIPTION
Some of the functionality described on this reference page extends the ISO C
standard. Applications shall define the appropriate feature test macro (see the
System Interfaces volume of POSIX.1‐2017, Section 2.2, The Compilation
Environment) to enable the visibility of these symbols in this header.

The `<string.h>` header shall define NULL and `size_t` as described in
`<stddef.h>`.

The `<string.h>` header shall define the `locale_t` type as described in
`<locale.h>`.

The following shall be declared as functions and may also be defined as macros.
Function prototypes shall be provided for use with ISO C standard compilers.


|   Function     |      Parameter 1      |     Parameter 2      |      Parameter 3     |    Parameter 4    |
|:--------------:|:---------------------:|:--------------------:|:--------------------:|:-----------------:|
| **memccpy**    | `void *dest`          | `const void *src`    | `int stop_char`      | `size_t num_bytes`|
| **memchr**     | `const void *buf`     | `int search_char`    | `size_t num_bytes`   |                   |
| **memcmp**     | `const void *buf1`    | `const void *buf2`   | `size_t num_bytes`   |                   |
| **memcpy**     | `void *dest`          | `const void *src`    | `size_t num_bytes`   |                   |
| **memmove**    | `void *dest`          | `const void *src`    | `size_t num_bytes`   |                   |
| **memset**     | `void *buffer`        | `int value`          | `size_t num_bytes`   |                   |
| **stpcpy**     | `char *dest`          | `const char *src`    |                      |                   |
| **stpncpy**    | `char *dest`          | `const char *src`    | `size_t num_chars`   |                   |
| **strcat**     | `char *dest`          | `const char *src`    |                      |                   |
| **strchr**     | `const char *str`     | `int search_char`    |                      |                   |
| **strcmp**     | `const char *str1`    | `const char *str2`   |                      |                   |
| **strcoll**    | `const char *str1`    | `const char *str2`   |                      |                   |
| **strcoll_l**  | `const char *str1`    | `const char *str2`   | `locale_t locale`    |                   |
| **strcpy**     | `char *dest`          | `const char *src`    |                      |                   |
| **strcspn**    | `const char *str`     | `const char *reject` |                      |                   |
| **strdup**     | `const char *src`     |                      |                      |                   |
| **strerror**   | `int errnum`          |                      |                      |                   |
| **strerror_l** | `int errnum`          | `locale_t locale`    |                      |                   |
| **strerror_r** | `int errnum`          | `char *buf`          | `size_t buflen`      |                   |
| **strlen**     | `const char *str`     |                      |                      |                   |
| **strncat**    | `char *dest`          | `const char *src`    | `size_t num_chars`   |                   |
| **strncmp**    | `const char *str1`    | `const char *str2`   | `size_t num_chars`   |                   |
| **strncpy**    | `char *dest`          | `const char *src`    | `size_t num_chars`   |                   |
| **strndup**    | `const char *src`     | `size_t num_chars`   |                      |                   |
| **strnlen**    | `const char *str`     | `size_t max_len`     |                      |                   |
| **strpbrk**    | `const char *str`     | `const char *accept` |                      |                   |
| **strrchr**    | `const char *str`     | `int search_char`    |                      |                   |
| **strsignal**  | `int signal`          |                      |                      |                   |
| **strspn**     | `const char *str`     | `const char *accept` |                      |                   |
| **strstr**     | `const char *haystack`| `const char *needle` |                      |                   |
| **strtok**     | `char *str`           | `const char *delims` |                      |                   |
| **strtok_r**   | `char *str`           | `const char *delims` |                      | `char **saveptr`  |
| **strxfrm**    | `char *dest`          | `const char *src`    | `size_t num_chars`   |                   |
| **strxfrm_l**  | `char *dest`          | `const char *src`    | `size_t num_chars`   | `locale_t locale` |

---
Windows Specific:
```c
// Memory Operations
void    *CopyMemory(VOID *destination, const VOID *source, SIZE_T length); // Linux Equivalent: memcpy
void    *MoveMemory(VOID *destination, const VOID *source, SIZE_T length); // Linux Equivalent: memmove
void    *FillMemory(VOID *destination, SIZE_T length, BYTE fill); // Linux Equivalent: memset with specific byte
void    *ZeroMemory(VOID *destination, SIZE_T length); // Linux Equivalent: memset to zero

// Secure versions of string functions (available in Visual C++ and later)
errno_t  memcpy_s(void *dest, rsize_t destSize, const void *src, rsize_t count); // Enhanced Linux Equivalent: memcpy
errno_t  memmove_s(void *dest, rsize_t destSize, const void *src, rsize_t count); // Enhanced Linux Equivalent: memmove
errno_t  memset_s(void *dest, rsize_t destSize, int ch, rsize_t count); // Enhanced Linux Equivalent: memset

// String Manipulation
errno_t  strcpy_s(char *dest, rsize_t destSize, const char *src); // Linux Equivalent: strcpy
errno_t  strncpy_s(char *dest, rsize_t destSize, const char *src, rsize_t count); // Linux Equivalent: strncpy
errno_t  strcat_s(char *dest, rsize_t destSize, const char *src); // Linux Equivalent: strcat
errno_t  strncat_s(char *dest, rsize_t destSize, const char *src, rsize_t count); // Linux Equivalent: strncat

// String Comparison
int      lstrcmp(LPCSTR lpString1, LPCSTR lpString2); // Linux Equivalent: strcmp
int      lstrcmpi(LPCSTR lpString1, LPCSTR lpString2); // Linux Equivalent: strcasecmp
int      CompareString( // More granular control compared to Linux
  LCID Locale,
  DWORD dwCmpFlags,
  PCNZCH lpString1,
  int cchCount1,
  PCNZCH lpString2,
  int cchCount2
); // Linux Equivalent: strcoll

// String Search
LPSTR    StrChr(LPCSTR lpStart, CHAR ch); // Linux Equivalent: strchr
LPSTR    StrRChr(LPCSTR lpStart, LPCSTR lpEnd, CHAR ch); // Linux Equivalent: strrchr
LPSTR    StrStr(LPCSTR lpFirst, LPCSTR lpSrch); // Linux Equivalent: strstr

// String Length
int      lstrlen(LPCSTR lpString); // Linux Equivalent: strlen

// Secure Zero Memory
void    SecureZeroMemory(VOID *ptr, SIZE_T cnt); // Linux Equivalent: Explicitly setting memory to zero, no direct equivalent
```

Inclusion of the `<string.h>` header may also make visible all symbols from `<stddef.h>`.

### The following sections are informative.

## SEE ALSO
- [locale.h(0p)](https://man7.org/linux/man-pages/man0/locale.h.0p.html)
- [stddef.h(0p)](https://man7.org/linux/man-pages/man0/stddef.h.0p.html)
- [sys_types.h(0p)](https://man7.org/linux/man-pages/man0/sys_types.h.0p.html)
- The System Interfaces volume of POSIX.1‐2017, Section 2.2, The Compilation Environment,
  - [memccpy(3p)](https://man7.org/linux/man-pages/man3/memccpy.3p.html)
  - [memchr(3p)](https://man7.org/linux/man-pages/man3/memchr.3p.html)
  - [memcmp(3p)](https://man7.org/linux/man-pages/man3/memcmp.3p.html)
  - [memcpy(3p)](https://man7.org/linux/man-pages/man3/memcpy.3p.html)
  - [memmove(3p)](https://man7.org/linux/man-pages/man3/memmove.3p.html)
  - [memset(3p)](https://man7.org/linux/man-pages/man3/memset.3p.html)
  - [strcat(3p)](https://man7.org/linux/man-pages/man3/strcat.3p.html)
  - [strchr(3p)](https://man7.org/linux/man-pages/man3/strchr.3p.html)
  - [strcmp(3p)](https://man7.org/linux/man-pages/man3/strcmp.3p.html)
  - [strcoll(3p)](https://man7.org/linux/man-pages/man3/strcoll.3p.html)
  - [strcpy(3p)](https://man7.org/linux/man-pages/man3/strcpy.3p.html)
  - [strcspn(3p)](https://man7.org/linux/man-pages/man3/strcspn.3p.html)
  - [strdup(3p)](https://man7.org/linux/man-pages/man3/strdup.3p.html)
  - [strerror(3p)](https://man7.org/linux/man-pages/man3/strerror.3p.html)
  - [strlen(3p)](https://man7.org/linux/man-pages/man3/strlen.3p.html)
  - [strncat(3p)](https://man7.org/linux/man-pages/man3/strncat.3p.html)
  - [strncmp(3p)](https://man7.org/linux/man-pages/man3/strncmp.3p.html)
  - [strncpy(3p)](https://man7.org/linux/man-pages/man3/strncpy.3p.html)
  - [strpbrk(3p)](https://man7.org/linux/man-pages/man3/strpbrk.3p.html)
  - [strrchr(3p)](https://man7.org/linux/man-pages/man3/strrchr.3p.html)
  - [strsignal(3p)](https://man7.org/linux/man-pages/man3/strsignal.3p.html)
  - [strspn(3p)](https://man7.org/linux/man-pages/man3/strspn.3p.html)
  - [strstr(3p)](https://man7.org/linux/man-pages/man3/strstr.3p.html)
  - [strtok(3p)](https://man7.org/linux/man-pages/man3/strtok.3p.html)
  - [strxfrm(3p)](https://man7.org/linux/man-pages/man3/strxfrm.3p.html)

## COPYRIGHT
Portions of this text are reprinted and reproduced in electronic form from IEEE
Std 1003.1-2017, Standard for Information Technology -- Portable Operating
System Interface (POSIX), The Open Group Base Specifications Issue 7, 2018
Edition, Copyright (C) 2018 by the Institute of Electrical and Electronics
Engineers, Inc and The Open Group. In the event of any discrepancy between this
version and the original IEEE and The Open Group Standard, the original IEEE and
The Open Group Standard is the referee document. The original Standard can be
obtained online at [OpenGroup](http://www.opengroup.org/unix/online.html).

Any typographical or formatting errors that appear in this page are most likely
to have been introduced during the conversion of the source files to man page
format. To report such errors, see:
[this](https://www.kernel.org/doc/man-pages/reporting_bugs.html).

Copyright 2017 IEEE/The Open Group


---

# Windows Development: String Manipulation Cheat Sheet

## Key Considerations

* **Unicode (UTF-16):** Design applications with internationalization in mind
  and embrace Unicode when using Windows APIs.
* **Safety and Security:** Prioritize the use of secure string manipulation
  functions (those ending in `_s` like `strcpy_s`, `strcat_s`) to protect your
  code from buffer overflows and other vulnerabilities.
* **Windows vs. POSIX:** Familiarize yourself with Windows-specific alternatives
  to POSIX-style string functions (see the table below).
* **C++:** For safer and more streamlined string handling in C++ projects,
  leverage `std::string` or `std::wstring`.


## String Manipulation Function Alternatives

| POSIX-like Function | Windows Alternative(s)                 | Notes                                         |
|---------------------|----------------------------------------|-----------------------------------------------|
| `memcpy`, `memmove` | `CopyMemory`, `RtlMoveMemory`          | For copying blocks of memory.                 |
| `memset`            | `FillMemory`, `ZeroMemory`             | Sets memory blocks.                           |
| `strcat`, `strncat` | `StringCchCat`, `strcat_s`             | Concatenates strings (use safe alternatives). |
| `strcmp`, `strncmp` | `CompareString`, `lstrcmp`, `lstrcmpi` | String comparison options.                    |
| `strcpy`, `strncpy` | `StringCchCopy`, `strcpy_s`            | Copies strings (use safe alternatives).       |
| `strlen`            | `lstrlen`                              | Gets string length.                           |
| `strchr`, `strrchr` | `StrChr`, `StrRChr`                    | Finds specific characters in strings.         |
| `strstr`            | `StrStr`                               | Finds a substring within a string.            |

## Essential Windows API Functions

* **`StringCchCat`:** Adds a string to a destination string (handles size constraints).
* **`StringCchCopy`:** Copies a string to a destination  (handles size constraints).
* **`lstrlen`:** Computes a string's length.
* **`CompareString`/`CompareStringEx`:** Offers locale-aware string comparison.
* **`WideCharToMultiByte` / `MultiByteToWideChar`:**  Conversions between  Unicode and ANSI (useful for interoperability).

## Security Awareness

Always prioritize functions designed with security in mind. Safe functions often have an `_s` 
suffix and implement crucial buffer size checks.

## C++ Considerations

Use C++ standard library classes `std::string` and `std::wstring` for improved string 
management, reducing manual memory management risks.  They enhance code readability 
and maintainability.

## Practical Example

```c

#include <Windows.h>
#include <strsafe.h>
#include <stdio.h>

int main() {
    char src[50] = "Hello, ";
    char dest[20] = "World!";
    char final[100];
    size_t destLength = 0;
    HRESULT hr;

    // Securely copy 'src' to 'final'
    hr = StringCchCopy(final, ARRAYSIZE(final), src);
    if (FAILED(hr)) {
        printf("Copy Error: %X\n", hr);
        return -1;
    }

    // Securely concatenate 'dest' onto 'final'
    hr = StringCchCat(final, ARRAYSIZE(final), dest);
    if (FAILED(hr)) {
        printf("Concatenate Error: %X\n", hr);
        return -1;
    }

    // Securely get the length of 'final'
    hr = StringCchLength(final, ARRAYSIZE(final), &destLength);
    if (FAILED(hr)) {
        printf("Length Error: %X\n", hr);
        return -1;
    }

    printf("Result: %s\nLength: %zu\n", final, destLength);

    return 0;
}
    /* > a.out
    * Result: Hello, World!
    * Length: 13
    */
}
```

##  Additional Tips

* **Header Files:** `<string.h>` for C functions, `<Windows.h>` for Windows API.
* **Development Tools:** Utilize the latest Microsoft Visual Studio and Windows SDK.
* **Data types:** Employ Windows data types like `SIZE_T`, `DWORD`.
* **TCHAR Macros:**  `TCHAR` macros aid in ANSI/Unicode compatible code (*use carefully*).
* **Documentation:**  Visit Microsoft Docs for comprehensive API references.
    [Read Docs](https://learn.microsoft.com/en-us/cpp/cpp/string-and-character-literals-cpp?view=msvc-170) 
