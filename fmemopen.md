# FMEMOPEN(3) Linux Programmer's Manual
## NAME
fmemopen - open memory as a stream
## SYNOPSIS
```c
#include <stdio.h>

FILE *fmemopen(void *buf, size_t size, const char *mode);
```

## DESCRIPTION
The `fmemopen()` function opens a stream that permits the access specified by `mode`. The stream allows I/O to be performed on the string or memory buffer pointed to by `buf`.

The `mode` argument specifies the semantics of I/O on the stream and can be one of the following:
- `r`: The stream is opened for reading.
- `w`: The stream is opened for writing.
- `a`: Append; open the stream for writing, with the initial buffer position set to the first null byte.
- `r+`: Open the stream for reading and writing.
- `w+`: Open the stream for reading and writing. The buffer contents are truncated (i.e., `'\0'` is placed in the first byte of the buffer).
- `a+`: Append; open the stream for reading and writing, with the initial buffer position set to the first null byte.

The stream maintains a current position that can be updated by I/O operations, explicitly using `fseek(3)` or determined using `ftell(3)`. In all modes other than append, the initial position is set to the start of the buffer. In append mode, if no null byte is found within the buffer, then the initial position is `size+1`.

If `buf` is specified as NULL, `fmemopen()` allocates a buffer of `size` bytes, which is useful for writing data to a temporary buffer and then reading it back. The initial position is set to the start of the buffer, and the buffer is automatically freed when the stream is closed. Note that the caller has no way to obtain a pointer to the temporary buffer allocated by this call.

If `buf` is not NULL, it should point to a buffer of at least `size` bytes allocated by the caller.

Write operations either occur at the current position (for modes other than append) or at the current size of the stream (for append modes). Attempts to write more than `size` bytes to the buffer result in an error.

## RETURN VALUE
Upon successful completion, `fmemopen()` returns a `FILE` pointer. Otherwise, it returns NULL, and `errno` is set to indicate the error.

## VERSIONS
`fmemopen()` was already available in glibc 1.0.x.

## ATTRIBUTES
For an explanation of the terms used in this section, see `attributes(7)`.

```
Interface    Attribute    Value
fmemopen()    Thread safety    MT-Safe
```

## CONFORMING TO
POSIX.1-2008.

## NOTES
There is no file descriptor associated with the file stream returned by this function (i.e., `fileno(3)` will return an error if called on the returned stream).

Binary mode ("b") was supported in versions of glibc before 2.22, but it was removed in glibc 2.22.

## BUGS
- In versions of glibc before 2.22, specifying `size` as zero causes `fmemopen()` to fail with `EINVAL`.
- In versions of glibc before 2.22, specifying append mode ("a" or "a+") and not covering a null byte in `buf` resulted in incorrect initial buffer position.
- In versions of glibc before 2.22, `fseek(3)` with `SEEK_END` behaved incorrectly when used on a stream created by `fmemopen()`.

## EXAMPLES
The program below uses `fmemopen()` to open an input buffer and `open_memstream(3)` to open a dynamically sized output buffer. It reads integers from the input string and writes the squares of these integers to the output buffer.

```c
#define _GNU_SOURCE
#include <string.h>
#include <stdio.h>
#include <stdlib.h>

#define handle_error(msg) \
    do { perror(msg); exit(EXIT_FAILURE); } while (0)

int main(int argc, char *argv[])
{
    FILE *out, *in;
    int v, s;
    size_t size;
    char *ptr;

    if (argc != 2) {
        fprintf(stderr, "Usage: %s <num>...\n", argv[0]);
        exit(EXIT_FAILURE);
    }

    in = fmemopen(argv[1], strlen(argv[1]), "r");
    if (in == NULL)
        handle_error("fmemopen");

    out = open_memstream(&ptr, &size);
    if (out == NULL)
        handle_error("open_memstream");

    for (;;) {
        s = fscanf(in, "%d", &v);
        if (s <= 0)
            break;

        s = fprintf(out, "%d ", v * v);
        if (s == -1)
            handle_error("fprintf");
    }

    fclose(in);
    fclose(out);

    printf("size=%zu; ptr=%s\n", size, ptr);

    free(ptr);
    exit(EXIT_SUCCESS);
}
```

## SEE ALSO
`fopen(3)`, `fopencookie(3)`, `open_memstream(3)`
