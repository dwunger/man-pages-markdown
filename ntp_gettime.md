# `ntp_gettime(3)` and `ntp_gettimex(3)` Linux Programmer's Manual
## NAME
`ntp_gettime`, `ntp_gettimex` - get time parameters (NTP daemon interface)
## SYNOPSIS
```c
#include <sys/timex.h>

int ntp_gettime(struct ntptimeval *ntv);
int ntp_gettimex(struct ntptimeval *ntv);
```
## DESCRIPTION
Both of these APIs return information to the caller via the `ntv` argument, a structure of the following type:
```c
struct ntptimeval {
    struct timeval time;    /* Current time */
    long maxerror;          /* Maximum error */
    long esterror;          /* Estimated error */
    long tai;               /* TAI offset */

    /* Further padding bytes allowing for future expansion */
};
```
The fields of this structure are as follows:
- `time`: The current time, expressed as a `timeval` structure:
```c
struct timeval {
    time_t      tv_sec;   /* Seconds since the Epoch */
    suseconds_t tv_usec;  /* Microseconds */
};
```
- `maxerror`: Maximum error, in microseconds. This value can be initialized by `ntp_adjtime(3)`, and is increased periodically (on Linux: each second), but is clamped to an upper limit (the kernel constant `NTP_PHASE_MAX`, with a value of 16,000).
- `esterror`: Estimated error, in microseconds. This value can be set via `ntp_adjtime(3)` to contain an estimate of the difference between the system clock and the true time. This value is not used inside the kernel.
- `tai`: TAI (Atomic International Time) offset.

`ntp_gettime()` returns an `ntptimeval` structure in which the `time`, `maxerror`, and `esterror` fields are filled in.

`ntp_gettimex()` performs the same task as `ntp_gettime()`, but also returns information in the `tai` field.
## RETURN VALUE
The return values for `ntp_gettime()` and `ntp_gettimex()` are as for `adjtimex(2)`. Given a correct pointer argument, these functions always succeed.
## VERSIONS
The `ntp_gettime()` function is available since glibc 2.1. The `ntp_gettimex()` function is available since glibc 2.12.
## ATTRIBUTES
For an explanation of the terms used in this section, see [attributes(7)](http://man7.org/linux/man-pages/man7/attributes.7.html).
## CONFORMING TO
`ntp_gettime()` is described in the NTP Kernel Application Program Interface. `ntp_gettimex()` is a GNU extension.
## SEE ALSO
- [adjtimex(2)](http://man7.org/linux/man-pages/man2/adjtimex.2.html)
- [ntp_adjtime(3)](http://man7.org/linux/man-pages/man3/ntp_adjtime.3.html)
- [time(7)](http://man7.org/linux/man-pages/man7/time.7.html)
- [NTP "Kernel Application Program Interface"](http://www.slac.stanford.edu/comp/unix/package/rtems/src/ssrlApps/ntpNanoclock/api.htm)
