---
title: rand, rand_r, srand - Linux Programmer's Manual
date: 2021-03-22
categories: [Linux, Manual]
---

# NAME

rand, rand_r, srand - pseudo-random number generator

# SYNOPSIS

```c
#include <stdlib.h>

int rand(void);
int rand_r(unsigned int *seedp);
void srand(unsigned int seed);
```

# DESCRIPTION

The `rand()` function returns a pseudo-random integer in the range 0 to `RAND_MAX` inclusive.

The `srand()` function sets its argument as the seed for a new sequence of pseudo-random integers to be returned by `rand()`. These sequences are repeatable by calling `srand()` with the same seed value. If no seed value is provided, the `rand()` function is automatically seeded with a value of 1.

The function `rand()` is not reentrant, so it's not suitable for threaded applications. To achieve reproducible behavior in a threaded application, you can use the reentrant function `rand_r()`.

The `rand_r()` function, like `rand()`, returns a pseudo-random integer in the range [0, `RAND_MAX`]. It uses an integer pointed to by `seedp` to store state between calls. If `rand_r()` is called with the same initial value for `seedp`, and that value is not modified between calls, then the same pseudo-random sequence will result.

# RETURN VALUE

The `rand()` and `rand_r()` functions return a value between 0 and `RAND_MAX` (inclusive). The `srand()` function returns no value.

# ATTRIBUTES

For an explanation of the terms used in this section, see `attributes(7)`.

- Interface Attribute: Value
  - `rand()`, `rand_r()`, `srand()`: Thread safety MT-Safe

# CONFORMING TO

The functions `rand()` and `srand()` conform to SVr4, 4.3BSD, C89, C99, POSIX.1-2001. The function `rand_r()` is from POSIX.1-2001. POSIX.1-2008 marks `rand_r()` as obsolete.

# NOTES

The quality of randomness provided by `rand()` may vary depending on the implementation. It is not suitable for applications that require high-quality randomness. Consider using `random(3)` instead.

# EXAMPLES

POSIX.1-2001 provides an example implementation of `rand()` and `srand()` for use when the same sequence is needed on two different machines.

Here is a program that displays the pseudo-random sequence produced by `rand()` with a given seed:

```c
#include <stdlib.h>
#include <stdio.h>

int main(int argc, char *argv[]) {
    int r, nloops;
    unsigned int seed;

    if (argc != 3) {
        fprintf(stderr, "Usage: %s <seed> <nloops>\n", argv[0]);
        exit(EXIT_FAILURE);
    }

    seed = atoi(argv[1]);
    nloops = atoi(argv[2]);

    srand(seed);
    for (int j = 0; j < nloops; j++) {
        r = rand();
        printf("%d\n", r);
    }

    exit(EXIT_SUCCESS);
}
```

# SEE ALSO

- `drand48(3)`
- `random(3)`
