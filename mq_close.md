# `mq_close(3)` Linux Programmer's Manual
## NAME
`mq_close` - close a message queue descriptor
## SYNOPSIS
```c
#include <mqueue.h>

int mq_close(mqd_t mqdes);
```
Link with `-lrt`.
## DESCRIPTION
`mq_close()` closes the message queue descriptor `mqdes`.
## RETURN VALUE
On success `mq_close()` returns 0; on error, -1 is returned, with `errno` set to indicate the error.
## ERRORS
- `EBADF`: The message queue descriptor specified in `mqdes` is invalid.
## ATTRIBUTES
For an explanation of the terms used in this section, see [attributes(7)](http://man7.org/linux/man-pages/man7/attributes.7.html).
```plaintext
Interface            Attribute     Value
mq_close()           Thread safety MT-Safe
```
## CONFORMING TO
POSIX.1-2001, POSIX.1-2008.
## NOTES
All open message queues are automatically closed on process termination, or upon [execve(2)](http://man7.org/linux/man-pages/man2/execve.2.html).
## SEE ALSO
- [`mq_getattr(3)`](http://man7.org/linux/man-pages/man3/mq_getattr.3.html)
- [`mq_notify(3)`](http://man7.org/linux/man-pages/man3/mq_notify.3.html)
- [`mq_open(3)`](http://man7.org/linux/man-pages/man3/mq_open.3.html)
- [`mq_receive(3)`](http://man7.org/linux/man-pages/man3/mq_receive.3.html)
- [`mq_send(3)`](http://man7.org/linux/man-pages/man3/mq_send.3.html)
- [`mq_unlink(3)`](http://man7.org/linux/man-pages/man3/mq_unlink.3.html)
- [`mq_overview(7)`](http://man7.org/linux/man-pages/man7/mq_overview.7.html)
