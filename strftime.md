# STRFTIME(3) Linux Programmer's Manual STRFTIME(3)

## NAME

**strftime** - format date and time

## SYNOPSIS

```c
#include <time.h>

size_t strftime(char *restrict s, size_t max,
                const char *restrict format,
                const struct tm *restrict tm);
```

## DESCRIPTION

The **strftime()** function formats the broken-down time **tm** according to the format specification **format** and places the result in the character array **s** of size **max**. The broken-down time structure **tm** is defined in **<time.h>**. See also **ctime(3)**.

The format specification is a null-terminated string and may contain special character sequences called "conversion specifications," each of which is introduced by a '%' character and terminated by some other character known as a "conversion specifier character." All other character sequences are "ordinary character sequences."

The characters of ordinary character sequences (including the null byte) are copied verbatim from **format** to **s**. However, the characters of conversion specifications are replaced as shown in the list below. In this list, the field(s) employed from the **tm** structure are also shown.

- **%a**: The abbreviated name of the day of the week according to the current locale. (Calculated from **tm_wday**.)
- **%A**: The full name of the day of the week according to the current locale. (Calculated from **tm_wday**.)
- **%b**: The abbreviated month name according to the current locale. (Calculated from **tm_mon**.)
- **%B**: The full month name according to the current locale. (Calculated from **tm_mon**.)
- **%c**: The preferred date and time representation for the current locale. (The specific format used in the current locale can be obtained by calling **nl_langinfo(3)** with **D_T_FMT** as an argument for the **%c** conversion specification, and with **ERA_D_T_FMT** for the **%Ec** conversion specification.) (In the POSIX locale, this is equivalent to "%a %b %e %H:%M:%S %Y".)
- **%C**: The century number (year/100) as a 2-digit integer. (Calculated from **tm_year**.)
- **%d**: The day of the month as a decimal number (range 01 to 31). (Calculated from **tm_mday**.)
- **%D**: Equivalent to **%m/%d/%y**. (Americans should note that in other countries, **%d/%m/%y** is rather common. This means that in an international context this format is ambiguous and should not be used.) (SU)
- **%e**: Like **%d**, the day of the month as a decimal number, but a leading zero is replaced by a space. (SU) (Calculated from **tm_mday**.)
- **%E**: Modifier: use alternative ("era-based") format, see below. (SU)
- **%F**: Equivalent to **%Y-%m-%d** (the ISO 8601 date format). (C99)
- **%G**: The ISO 8601 week-based year with century as a decimal number. The 4-digit year corresponding to the ISO week number. This has the same format and value as **%Y**, except that if the ISO week number belongs to the previous or next year, that year is used instead. (TZ) (Calculated from **tm_year**, **tm_yday**, and **tm_wday**.)
- **%g**: Like **%G**, but without century, that is, with a 2-digit year (00-99). (TZ) (Calculated from **tm_year**, **tm_yday**, and **tm_wday**.)
- **%h**: Equivalent to **%b**. (SU)
- **%H**: The hour as a decimal number using a 24-hour clock (range 00 to 23). (Calculated from **tm_hour**.)
- **%I**: The hour as a decimal number using a 12-hour clock (range 01 to 12). (Calculated from **tm_hour**.)
- **%j**: The day of the year as a decimal number (range 001 to 366). (Calculated from **tm_yday**.)
- **%k**: The hour (24-hour clock) as a decimal number (range 0 to 23); single digits are preceded by a blank. (See also **%H**.) (Calculated from **tm_hour**.) (TZ)
- **%l**: The hour (12-hour clock) as a decimal number (range 1 to 12); single digits are preceded by a blank. (See also **%I**.) (Calculated from **tm_hour**.) (TZ)
- **%m**: The month as a decimal number (range 

01 to 12). (Calculated from **tm_mon**.)
- **%M**: The minute as a decimal number (range 00 to 59). (Calculated from **tm_min**.)
- **%n**: A newline character. (SU)
- **%O**: Modifier: use alternative numeric symbols, see below. (SU)
- **%p**: Either "AM" or "PM" according to the given time value, or the corresponding strings for the current locale. Noon is treated as "PM" and midnight as "AM". (Calculated from **tm_hour**.) (The specific string representations used for "AM" and "PM" in the current locale can be obtained by calling **nl_langinfo(3)** with **AM_STR** and **PM_STR**, respectively.)
- **%P**: Like **%p** but in lowercase: "am" or "pm" or a corresponding string for the current locale. (Calculated from **tm_hour**.) (GNU)
- **%r**: The time in a.m. or p.m. notation. (SU) (The specific format used in the current locale can be obtained by calling **nl_langinfo(3)** with **T_FMT_AMPM** as an argument.) (In the POSIX locale, this is equivalent to "%I:%M:%S %p".)
- **%R**: The time in 24-hour notation (%H:%M). (SU) For a version including the seconds, see **%T** below.
- **%s**: The number of seconds since the Epoch, 1970-01-01 00:00:00 +0000 (UTC). (TZ) (Calculated from **mktime(tm)**.)
- **%S**: The second as a decimal number (range 00 to 60). (The range is up to 60 to allow for occasional leap seconds.) (Calculated from **tm_sec**.)
- **%t**: A tab character. (SU)
- **%T**: The time in 24-hour notation (%H:%M:%S). (SU)
- **%u**: The day of the week as a decimal, range 1 to 7, Monday being 1. See also **%w**. (Calculated from **tm_wday**.) (SU)
- **%U**: The week number of the current year as a decimal number, range 00 to 53, starting with the first Sunday as the first day of week 01. See also **%V** and **%W**. (Calculated from **tm_yday** and **tm_wday**.)
- **%V**: The ISO 8601 week number of the current year as a decimal number, range 01 to 53, where week 1 is the first week that has at least 4 days in the new year. See also **%U** and **%W**. (Calculated from **tm_year**, **tm_yday**, and **tm_wday**.) (SU)
- **%w**: The day of the week as a decimal, range 0 to 6, Sunday being 0. See also **%u**. (Calculated from **tm_wday**.) (SU)
- **%W**: The week number of the current year as a decimal number, range 00 to 53, starting with the first Monday as the first day of week 01. (Calculated from **tm_yday** and **tm_wday**.)
- **%x**: The preferred date representation for the current locale without the time. (The specific format used in the current locale can be obtained by calling **nl_langinfo(3)** with **D_FMT** as an argument for the **%x** conversion specification, and with **ERA_D_FMT** for the **%Ex** conversion specification.) (In the POSIX locale, this is equivalent to "%m/%d/%y".)
- **%X**: The preferred time representation for the current locale without the date. (The specific format used in the current locale can be obtained by calling **nl_langinfo(3)** with **T_FMT** as an argument for the **%X** conversion specification, and with **ERA_T_FMT** for the **%EX** conversion specification.) (In the POSIX locale, this is equivalent to "%H:%M:%S".)
- **%y**: The year as a decimal number without a century (range 00 to 99). (The **%Ey** conversion specification corresponds to the year since the beginning of the era denoted by the **%EC** conversion specification.) (Calculated from **tm_year**.)
- **%Y**: The year as a decimal number including the century. (The **%EY** conversion specification corresponds to the full alternative year representation.) (Calculated from **tm_year**.)
- **%z**: The +hhmm or -hhmm numeric timezone (that is, the hour and minute offset from UTC). (SU)
- **%Z**: The timezone name or abbreviation.
- **%+**: The date and time in **date(1)** format. (TZ) (Not supported in glibc2.)
- **%%**: A literal '%' character.

Some conversion specifications can be modified by preceding the conversion specifier character by the **E** or **O** modifier to indicate that an alternative format should be used. If the alternative format or specification does not exist for the current locale, the format is used as if the **E** or **O** modifier was not specified.

The following **E** and **O** modifiers are supported:

- **%Ec**: The alternative format for the preferred date and time representation for the current locale. (The specific format used in the current locale can be obtained by calling **nl_langinfo(3)** with **ERA_D_T_FMT** as an argument for the **%Ec** conversion specification.) (C99)
- **%EC**: The name of the base year (period) in the alternative era notation. (The specific format used in the current locale can be obtained by calling **nl_langinfo(3)** with **ERA_YEAR** as an argument for the **%EC** conversion specification.) (C99)
- **%Ex**: The alternative format for the preferred date representation for the current locale without the time. (The specific format used in the current locale can be obtained by calling **nl_langinfo(3)** with **ERA_D_FMT** as an argument for the **%Ex** conversion specification.) (C99)
- **%EX**: The alternative format for the preferred time representation for the current locale without the date. (The specific format used in the current locale can be obtained by calling **nl_langinfo(3)** with **ERA_T_FMT** as an argument for the **%EX** conversion specification.) (C99)
- **%Ey**: The year as a decimal number without a century (range 0 to 99) with an alternative representation. (C99)
- **%EY**: The year as a decimal number including the century with an alternative representation. (C99)
- **%Od**: The day of the month as a decimal number (range 01 to 31) with an alternative numeric symbol. (C99)
- **%Oe**: Like **%Od**, the day of the month as a decimal number, but a leading zero is replaced by a space. (C99)
- **%OH**: The hour as a decimal number using a 24-hour clock (range 00 to 23) with an alternative numeric symbol. (C99)
- **%OI**: The hour as a decimal number using a 12-hour clock (range 01 to 12) with an alternative numeric symbol. (C99)
- **%Om**: The month as a decimal number (range 01 to 12) with an alternative numeric symbol. (C99)
- **%OM**: The minute as a decimal number (range 00 to 59) with an alternative numeric symbol. (C99)
- **%OS**: The second as a decimal number (range 00 to 60) with an alternative numeric symbol. (C99)
- **%Ou**: The day of the week as a decimal, range 1 to 7, Monday being 1, with an alternative numeric symbol. (C99)
- **%OU**: The week number of the current year as a decimal number, range 00 to 53, with an alternative numeric symbol. (C99)
- **%OV**: The ISO 8601 week number of the current year as a decimal number, range 01 to 53, with an alternative numeric symbol. (C99)
- **%Ow**: The day of the week as a decimal, range 0 to 6, Sunday being 0, with an alternative numeric symbol. (C99)
- **%OW**: The week number of the current year as a decimal number, range 00 to 53, starting with the first Monday as the first day of week 01, with an alternative numeric symbol. (C99)
- **%Oy**: The year as a decimal number without a century (range 00 to 99) with an alternative numeric symbol. (C99)

If **max** is not zero, the result string is stored in the character array **s**. A character array is simply a byte array with a **'\0'** (NUL) byte at the end. When **max** is zero, **s** is not used and strftime() just returns the number of characters that would be generated for the given input, without writing anything to the output array. This can be used to determine the necessary buffer size.

If the length of the result string (including the terminating NUL byte) would exceed **max**, strftime() returns 0, and the contents of **s** are undefined.

## RETURN VALUE

The strftime() function returns the number of characters placed in the array **s**, not including the terminating NUL character, provided that **max** is not exceeded. If **max** is zero, strftime() returns the number of characters that would have been written if **max** had been sufficiently large.

If an error occurs, 0 is returned.

## ERRORS

The following error may be encountered:

- **ENOMEM**: Insufficient storage was available to complete the operation.

## CONFORMING TO

POSIX.1-2001, POSIX.1-2008, C99.

In C89 and C99 mode, there are no error returns. In C11 mode, it is an error if the argument is not null and not large enough to store the resulting string. In all three versions, it is an error if the format string contains more than one conversion specification.

## NOTES

In most cases, the **fmt** argument to **strftime()** does not contain the conversion specifier **%%**; two **%%** characters are converted to a single **%** character.

The **%p**, **%P**, and **%r** specifiers behave locale-specifically. For example, **%p** might be either AM or am in the C locale. In some locales, there is no AM or PM symbol corresponding to a specific time. In the C locale, **%p** and **%P** are equivalent to **%a** and **%A**, respectively.

The **tm** structure is defined as follows:

```c
struct tm {
    int tm_sec;     /* seconds */
    int tm_min;     /* minutes */
    int tm_hour;    /* hours */
    int tm_mday;    /* day of the month */
    int tm_mon;     /* month */
    int tm_year;    /* year */
    int tm_wday;    /* day of the week */
    int tm_yday;    /* day in the year */
    int tm_isdst;   /* daylight saving time */
};
```

## SEE ALSO

**ctime(3)**, **localtime(3)**, **mktime(3)**, **strptime(3)**, **time(3)**

## COLOPHON

This page is part of release 5.14 of the Linux man-pages project. A description of the project, information about reporting bugs, and the latest version of this page, can be found at https://www.kernel.org/doc/man-pages/.

## NOTES

The C standard has an additional conversion specification **%t** (with the **E** modifier for alternative format). This is not supported by glibc and leads to **EINVAL**. The kernel used to support it (in a very minimal way), but this has been removed.

The *c* and *x* format specifiers are not part of the C standard, but are described in the Single Unix Specification.

The **%F** format specifier

 is from POSIX.1-2001.

The **%z** format specifier is from ISO 8601:2000(E).

POSIX.1-2001 specifies the alternative format sequences for the **%A**, **%a**, **%B**, **%b**, and **%x** format specifiers.

The **O** modifier for some format specifiers was added by C99 and the **E** modifier was added by POSIX.1-2001. Neither are supported by glibc2.

The **%r**, **%R**, **%T**, **%X**, and **%Z** format specifiers are from SVr4.

The **%g** format specifier is from POSIX.2.

The **%E** and **%O** modifiers are from SUSv2 and POSIX.1-2001. They were not present in earlier versions of these standards.

The **%s** format specifier is from C89. The same format specifier character is used by **strftime(3)** (with different semantics).

The **tm** structure is defined by POSIX.1-2001. The **tm_gmtoff** member is not a standard, but is expected to be present by POSIX.1-2008.

Index

## NAME

strftime, strftime_l - format date and time

## SYNOPSIS

```c
#include <time.h>

size_t strftime(char *s, size_t max, const char *format, const struct tm *tm);
```

```c
#include <xlocale.h>

size_t strftime_l(char *s, size_t max, const char *format, const struct tm *tm, locale_t loc);
```

## DESCRIPTION

The strftime() function formats the broken-down time tm according to the format specification format and places the result in the character array s of size max. The format specification is a null-terminated string and may contain special character sequences called conversion specifications, each of which is introduced by a '%' character and terminated by some other character known as a conversion specifier character. The conversion specifier character specifies the type of data to include in the output string. The strftime() function replaces each conversion specification in format with the appropriate data as described in the following list. The appropriate data is stored in the result string with the following format:

- Ordinary characters (i.e., characters other than '%' and the conversion specifier character) are copied to the result string unchanged.
- Conversion specification: a sequence starting with '%' and ending with a conversion specifier character.
- Conversion specifier: a character that specifies the type of data to include in the result string.

The following conversion specifiers are defined:

- **%a**: The abbreviated weekday name according to the current locale.
- **%A**: The full weekday name according to the current locale.
- **%b**: The abbreviated month name according to the current locale.
- **%B**: The full month name according to the current locale.
- **%c**: The preferred date and time representation for the current locale. (C99)
- **%C**: The century number (year/100) as a 2-digit integer.
- **%d**: The day of the month as a decimal number (range 01 to 31).
- **%D**: Equivalent to **%m/%d/%y**. (Yecch—for Americans only. Americans should note that in other countries **%d/%m/%y** is rather common. This means that in international context this format is ambiguous and should not be used.) (SU)
- **%e**: Like **%d**, the day of the month as a decimal number, but a leading zero is replaced by a space.
- **%F**: Equivalent to **%Y-%m-%d**. (C99)
- **%g**: The last 2 digits of the week-based year (see **%G**). (SU)
- **%G**: The week-based year (see **%V**) as a 4-digit number. (SU)
- **%h**: Equivalent to **%b**. (SU)
- **%H**: The hour as a decimal number using a 24-hour clock (range 00 to 23).
- **%I**: The hour as a decimal number using a 12-hour clock (range 01 to 12).
- **%j**: The day of the year as a decimal number (range 001 to 366).
- **%k**: The hour (24-hour clock) as a decimal number (range 0 to 23); single digits are preceded by a blank. (See also **%H**.) (TZ)
- **%l**: The hour (12-hour clock) as a decimal number (range 1 to 12); single digits are preceded by a blank. (See also **%I**.) (TZ)
- **%m**: The month as a decimal number (range 01 to 12).
- **%M**: The minute as a decimal number (range 00 to 59).
- **%n**: A newline character. (SU)
- **%O**: Modifier: use alternative numeric symbols, see below. (SU)
- **%p**: Either "AM" or "PM" according to the given time value, or the corresponding strings for the current locale. Noon is treated as "PM" and midnight as "AM". (Calculated from **tm_hour**.) (The specific string representations used for "AM" and "PM" in the current locale can be obtained by calling **nl_langinfo(3)** with **AM_STR** and **PM_STR**, respectively.)
- **%P**: Like **%p** but in lowercase: "am" or "pm" or a corresponding string for the current locale. (Calculated from **tm_hour**.) (GNU)
- **%r**: The time in a.m. or p.m. notation. (SU) (The specific format used in the current locale can be obtained by calling **nl_langinfo(3)** with **T_FMT_AMPM** as an argument.) (In the POSIX locale, this is equivalent to "%I:%M:%S %p".)
- **%R**: The time in 24-hour notation (%H:%M). (SU) For a version including the seconds, see **%T** below.
- **%s**: The number of seconds since the Epoch, 1970-01-01 00:00:00 +0000 (UTC). (TZ) (Calculated from **mktime(tm)**.)
- **%S**: The second as a decimal number (range 00 to 60). (The range is up to 60 to allow for occasional leap seconds.) (Calculated from **tm_sec**.)
- **%t**: A tab character. (SU)
- **%T**: The time in 24-hour notation (%H:%M:%S). (SU)
- **%u**: The day of the week as a decimal, range 1 to 7, Monday being 1. See also **%w**. (Calculated from **tm_wday**.) (SU)
- **%U**: The week number of the current year as a decimal number, range 00 to 53, starting with the first Sunday as the first day of week 01. See also **%V** and **%W**. (Calculated from **tm_yday** and **tm
- **V**: The ISO 8601:1988 week number of the current year as a decimal number, range 01 to 53, where week 1 is the first week that has at least 4 days in the new year. (Calculated from **tm_year**, **tm_mon**, and **tm_mday**.)
- **w**: The day of the week as a decimal, range 0 to 6, Sunday being 0. See also **%u**. (Calculated from **tm_wday**.) (SU)
- **W**: The week number of the current year as a decimal number, range 00 to 53, starting with the first Monday as the first day of week 01. (Calculated from **tm_yday** and **tm_wday**.) (SU)
- **x**: The preferred date representation for the current locale without the time. (C99)
- **X**: The preferred time representation for the current locale without the date. (C99)
- **y**: The year without a century (range 00 to 99).
- **Y**: The year with century as a decimal number (for example, 1970). (C99)
- **z**: The time-zone name or abbreviation, or no bytes if no time zone information exists. The associated value is a string that describes the time zone. (C99)
- **Z**: The time zone abbreviation, or no bytes if no time zone information exists. (SU)

The strftime() function formats the time in the struct tm according to the formatting rules specified by format and stores the result in the character array s. The max argument specifies the size of the s array. If max is not large enough to store the formatted string, the function will not write more than max characters to s.

The strftime_l() function is similar to strftime(), but it allows you to specify a locale for the formatting. The locale is specified using the loc argument, which should be a locale_t object. This allows you to format dates and times according to the conventions of a specific locale.

If the formatted string exceeds the size of the s array (max characters), the function will return 0, indicating that the output was truncated. If an error occurs during formatting, strftime() and strftime_l() return 0 as well.

The strftime() and strftime_l() functions are part of the C standard library and provide a convenient way to format dates and times according to various formatting rules. They are commonly used in C and C++ programs for date and time handling.

## RETURN VALUE

The strftime() and strftime_l() functions return the number of characters written to the s array, not including the null-terminating character. If the formatted string exceeds the size of the s array, they return 0. If an error occurs, they also return 0.

## ERRORS

The strftime() and strftime_l() functions do not set errno. Instead, they return 0 to indicate an error.

## EXAMPLES

Here are some examples of using the strftime() function to format dates and times:

```c
#include <stdio.h>
#include <time.h>

int main() {
    struct tm t;
    time_t now = time(NULL);
    localtime_r(&now, &t);

    char buf[100];
    size_t len = strftime(buf, sizeof(buf), "%Y-%m-%d %H:%M:%S", &t);
    if (len > 0) {
        printf("Formatted date and time: %s\n", buf);
    } else {
        printf("Error formatting date and time\n");
    }

    return 0;
}
```

In this example, we first obtain the current time using time() and then convert it to a struct tm using localtime_r(). We then use strftime() to format the date and time in a specific format ("%Y-%m-%d %H:%M:%S") and store the result in the buf array. Finally, we check the return value of strftime() to see if the formatting was successful.

## SEE ALSO

- ctime(3)
- localtime(3)
- mktime(3)
- strptime(3)
- time(3)

## CONFORMING TO

The strftime() and strftime_l() functions are specified in the C standard (ISO C99) and are also available in POSIX.1-2001 and later.

## NOTES

The strftime() and strftime_l() functions do not provide support for formatting time zones. Time zone information is system-dependent, and you may need to use additional functions or libraries to obtain and format time zone information.

The format strings used with strftime() and strftime_l() can be quite complex and may require careful handling to produce the desired output. It's important to consult the documentation for these functions and understand the format specifiers and conversion specifier characters used in the format string.

## COLOPHON

This page is part of release 5.14 of the Linux man-pages project. A description of the project, information about reporting bugs, and the latest version of this page, can be found at https://www.kernel.org/doc/man-pages/.
