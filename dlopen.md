# DLOPEN(3) Linux Programmer's Manual

## NAME
dlclose, dlopen, dlmopen - open and close a shared object

## SYNOPSIS
```c
#include <dlfcn.h>

void *dlopen(const char *filename, int flags);
int dlclose(void *handle);

#define _GNU_SOURCE
#include <dlfcn.h>

void *dlmopen(Lmid_t lmid, const char *filename, int flags);
```

Link with `-ldl`.

## DESCRIPTION

### dlopen()
The function `dlopen()` loads the dynamic shared object (shared library) file named by the null-terminated string `filename` and returns an opaque "handle" for the loaded object. This handle is employed with other functions in the dlopen API, such as `dlsym(3)`, `dladdr(3)`, `dlinfo(3)`, and `dlclose()`.

If `filename` is NULL, then the returned handle is for the main program. If `filename` contains a slash ("/"), then it is interpreted as a (relative or absolute) pathname. Otherwise, the dynamic linker searches for the object according to certain rules.

One of the following two values must be included in `flags`:
- `RTLD_LAZY`: Perform lazy binding.
- `RTLD_NOW`: Resolve symbols before `dlopen()` returns.

Zero or more of the following values may also be ORed in `flags`:
- `RTLD_GLOBAL`: Symbols defined by this shared object will be made available for symbol resolution of subsequently loaded shared objects.
- `RTLD_LOCAL`: Symbols defined in this shared object are not made available to resolve references in subsequently loaded shared objects.
- `RTLD_NODELETE` (since glibc 2.2): Do not unload the shared object during `dlclose()`.
- `RTLD_NOLOAD` (since glibc 2.2): Don't load the shared object.
- `RTLD_DEEPBIND` (since glibc 2.3.4): Place the lookup scope of the symbols in this shared object ahead of the global scope.

### dlmopen()
The `dlmopen()` function is similar to `dlopen()`, but it accepts an additional argument, `lmid`, which specifies the link-map list (namespace) in which the shared object should be loaded. The `lmid` argument is an opaque handle that refers to a namespace. The special values for `lmid` are `LM_ID_BASE` and `LM_ID_NEWLM`.

### dlclose()
The `dlclose()` function decrements the reference count on the dynamically loaded shared object referred to by `handle`. If the reference count drops to zero and no symbols in this object are required by other objects, then the object is unloaded after calling any destructors defined for the object.

## RETURN VALUE
- `dlopen()` and `dlmopen()` return a non-NULL handle on success, NULL on error.
- `dlclose()` returns 0 on success, nonzero on error.

## VERSIONS
- `dlopen()` and `dlclose()` are present in glibc 2.0 and later.
- `dlmopen()` first appeared in glibc 2.3.4.

## ATTRIBUTES
- Thread safety: MT-Safe

## CONFORMING TO
POSIX.1-2001 describes `dlclose()` and `dlopen()`. The `dlmopen()` function is a GNU extension.

## NOTES
### dlmopen() and namespaces
A link-map list defines an isolated namespace for the resolution of symbols by the dynamic linker. The `dlmopen()` function permits object-load isolation, allowing the loading of a shared object in a new namespace without exposing the rest of the application to the symbols made available by the new object.

### Initialization and finalization functions
Shared objects may export functions using the `__attribute__((constructor))` and `__attribute__((destructor))` function attributes. Constructor functions are executed before `dlopen()` returns, and destructor functions are executed before `dlclose()` returns.

### History
These functions are part of the dlopen API, derived from SunOS.

## BUGS
As at glibc 2.24, specifying the `RTLD_GLOBAL` flag when calling `dlmopen()` generates an error. Furthermore, specifying `RTLD_GLOBAL` when calling `dlopen()` results in a program crash if the call is made from any object loaded in a namespace other than the initial namespace.

## EXAMPLES
Below is an example program that loads the math library, looks up the address of the `cos` function, and prints the cosine of 2.0.
```c
#include <stdio.h>
#include <stdlib.h>
#include <dlfcn.h>
#include <gnu/lib-names.h>

int main(void) {
    void *handle;
    double (*cosine)(double);
    char *error;

    handle = dlopen(LIBM_SO, RTLD_LAZY);
    if (!handle) {
        fprintf(stderr, "%s\n", dlerror());
        exit(EXIT_FAILURE);
    }

    dlerror(); /* Clear any existing error */

    cosine = (double (*)(double))dlsym(handle, "cos");

    error = dlerror();
    if (error != NULL) {
        fprintf(stderr, "%s\n", error);
        exit(EXIT_FAILURE);
    }

    printf("%f\n", (*cosine)(2.0));
    dlclose(handle);
    exit(EXIT_SUCCESS);
}
```

## SEE ALSO
- ld(1), ldd(1), pldd(1), dl_iterate_phdr(3), dladdr(3), dlerror(3), dlinfo(3), dlsym(3), rtld-audit(7), ld.so(8), ldconfig(8)
