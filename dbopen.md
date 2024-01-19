# DBOPEN(3) Linux Programmer's Manual
## NAME
dbopen - database access methods
## SYNOPSIS
```c
#include <sys/types.h>
#include <limits.h>
#include <db.h>
#include <fcntl.h>

DB *dbopen(const char *file, int flags, int mode, DBTYPE type, const void *openinfo);
```
## DESCRIPTION
**Note well:** This page documents interfaces provided in glibc up until version 2.1. Since version 2.2, glibc no longer provides these interfaces. Probably, you are looking for the APIs provided by the `libdb` library instead.

`dbopen()` is the library interface to database files. The supported file formats are btree, hashed, and UNIX file oriented. The btree format is a representation of a sorted, balanced tree structure. The hashed format is an extensible, dynamic hashing scheme. The flat-file format is a byte stream file with fixed or variable length records. The formats and file-format-specific information are described in detail in their respective manual pages `btree(3)`, `hash(3)`, and `recno(3)`.

`dbopen()` opens `file` for reading and/or writing. Files never intended to be preserved on disk may be created by setting the `file` argument to NULL.

The `flags` and `mode` arguments are as specified to the `open(2)` routine, however, only the `O_CREAT`, `O_EXCL`, `O_EXLOCK`, `O_NONBLOCK`, `O_RDONLY`, `O_RDWR`, `O_SHLOCK`, and `O_TRUNC` flags are meaningful. (Note, opening a database file `O_WRONLY` is not possible.)

The `type` argument is of type `DBTYPE` (as defined in the `<db.h>` include file) and may be set to `DB_BTREE`, `DB_HASH`, or `DB_RECNO`.

The `openinfo` argument is a pointer to an access-method-specific structure described in the access method's manual page. If `openinfo` is NULL, each access method will use defaults appropriate for the system and the access method.

`dbopen()` returns a pointer to a `DB` structure on success and NULL on error. The `DB` structure is defined in the `<db.h>` include file and contains at least the following fields:

```c
typedef struct {
    DBTYPE type;
    int (*close)(const DB *db);
    int (*del)(const DB *db, const DBT *key, unsigned int flags);
    int (*fd)(const DB *db);
    int (*get)(const DB *db, DBT *key, DBT *data, unsigned int flags);
    int (*put)(const DB *db, DBT *key, const DBT *data, unsigned int flags);
    int (*sync)(const DB *db, unsigned int flags);
    int (*seq)(const DB *db, DBT *key, DBT *data, unsigned int flags);
} DB;
```

These elements describe a database type and a set of functions performing various actions. These functions take a pointer to a structure as returned by `dbopen()`, and sometimes one or more pointers to key/data structures and a flag value.

- `type`: The type of the underlying access method (and file format).
- `close`: A pointer to a routine to flush any cached information to disk, free any allocated resources, and close the underlying file(s).
- `del`: A pointer to a routine to remove key/data pairs from the database.
- `fd`: A pointer to a routine which returns a file descriptor representative of the underlying database.
- `get`: A pointer to a routine which is the interface for keyed retrieval from the database.
- `put`: A pointer to a routine to store key/data pairs in the database.
- `sync`: A pointer to a routine to flush any cached information to disk.
- `seq`: A pointer to a routine which is the interface for sequential retrieval from the database.

### Key/data pairs
Access to all file types is based on key/data pairs. Both keys and data are represented by the following data structure:

```c
typedef struct {
    void *data;
    size_t size;
} DBT;
```

- `data`: A pointer to a byte string.
- `size`: The length of the byte string.

Key and data byte strings may reference strings of essentially unlimited length, although any two of them must fit into available memory at the same time. It should be noted that the access methods provide no guarantees about byte string alignment.

## ERRORS
The `dbopen()` routine may fail and set `errno` for any of the errors specified for the library routines `open(2)` and `malloc(3)` or the following:

- `EFTYPE`: A file is incorrectly formatted.
- `EINVAL`: A parameter has been specified (hash function, pad byte, etc.) that is incompatible with the current file specification or which is not meaningful for the function (for example, use of the cursor without prior initialization) or there is a mismatch between the version number of file and the software.

The `close` routines may fail and set `errno` for any of the errors specified for the library routines `close(2)`, `read(2)`, `write(2)`, `free(3)`, or `fsync(2)`.

The `del`, `get`, `put`, and `seq` routines may fail and set `errno` for any of the errors specified for the library routines `read(2)`, `write(2)`, `free(3)`, or `malloc(3)`.

The `fd` routines will fail and set `errno` to `ENOENT` for in-memory databases.

The `sync` routines may fail and set `errno` for any of the errors specified for the library routine `fsync(2)`.

## BUGS
The typedef `DBT` is a mnemonic for "data base thang" and was used because no one could think of a reasonable name that wasn't already used.

The file descriptor interface is a kludge and will be deleted in a future version of the interface.

None of the access methods provide any form of concurrent access, locking, or transactions.

## SEE ALSO
`btree(3)`, `hash(3)`, `mpool(3)`, `recno(3)`, "LIBTP: Portable, Modular Transactions for UNIX" by Margo Seltzer and Michael Olson, USENIX proceedings, Winter 1992.

