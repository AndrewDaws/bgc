<!-- Beej's guide to C

# vim: ts=4:sw=4:nosi:et:tw=72
-->

# `<ctype.h>` Character Classification and Conversion {#ctype}

This collection of macros is good for testing characters to see if
they're of a certain class, such as alphabetic, numeric, control
characters, etc.

Surprisingly, they take `int` arguments instead of some kind of `char`.
This is so you can feed `EOF` in for convenience if you have an integer
representation of that. If not `EOF`, the value passed in has to be
representable in an `unsigned char`. Otherwise it's (dun dun DUUNNNN)
undefined behavior. So you can forget about passing in your UTF-8
multibyte characters.

Also, the behavior of these functions varies based on locale.

In many of the pages in this section, I give some examples. These are
from the "C" locale, and might vary if you've set a different locale.

Note that wide characters have their own set of classification
functions, so don't try to use these on `wchar_t`s. Or _else_!

[[pagebreak]]
## `isalnum()` {#man-isalnum}

Tests if a character is alphabetic or is a digit

### Synopsis {.unnumbered .unlisted}

``` {.c}
#include <ctype.h>

int isalnum(int c);
```

### Description {.unnumbered .unlisted}

Tests if a character is alphabetic (`A`-`Z` or `a`-`z`) or a digit
(`0`-`9`).

Is equivalent to:

``` {.c}
isalpha(c) || isdigit(c)
```

### Return Value {.unnumbered .unlisted}

Returns true if a character is alphabetic (`A`-`Z` or `a`-`z`) or a
digit (`0`-`9`).

### Example {.unnumbered .unlisted}

``` {.c .numberLines}
//             testing this char
//                      v
printf("%s\n", isalnum('a')? "yes": "no");  // yes
printf("%s\n", isalnum('B')? "yes": "no");  // yes
printf("%s\n", isalnum('5')? "yes": "no");  // yes
printf("%s\n", isalnum('?')? "yes": "no");  // no
```

### See Also {.unnumbered .unlisted}

[`isalpha()`](#man-isalpha),
[`isdigit()`](#man-isdigit)


[[pagebreak]]
## `isalpha()` {#man-isalpha}

Returns true if a character is alphabetic

### Synopsis {.unnumbered .unlisted}

``` {.c}
#include <ctype.h>

int isalpha(int c);
```

### Description {.unnumbered .unlisted}

Returns true for alphabetic characters (`A`-`Z` or `a`-`z`).

Technically (and in the "C" locale) equivalent to:

``` {.c}
isupper(c) || islower(c)
```

Extra super technically, because I know you're dying for this to be
extra unnecessarily complex, it can also include some locale-specific
characters for which this is true:

``` {.c}
!iscntrl(c) && !isdigit(c) && !ispunct(c) && !isspace(c)
```

and this is true:

``` {.c}
isupper(c) || islower(c)
```

### Return Value {.unnumbered .unlisted}

Returns true for alphabetic characters (`A`-`Z` or `a`-`z`).

Or for any of the other crazy stuff in the description, above.

### Example {.unnumbered .unlisted}

``` {.c .numberLines}
//             testing this char
//                      v
printf("%s\n", isalpha('a')? "yes": "no");  // yes
printf("%s\n", isalpha('B')? "yes": "no");  // yes
printf("%s\n", isalpha('5')? "yes": "no");  // no
printf("%s\n", isalpha('?')? "yes": "no");  // no
```

### See Also {.unnumbered .unlisted}

[`isalnum()`](#man-isalnum)

[[pagebreak]]
## `isblank()` {#man-isblank}

### Synopsis {.unnumbered .unlisted}

``` {.c}
#include <ctype.h>

int isblank(int c);
```

### Description {.unnumbered .unlisted}

True if the character is a whitespace character used to separate words
in a single line.

For example, space (`' '`) or horizontal tab (`'\t'`). Other locales
might define other blank characters.

### Return Value {.unnumbered .unlisted}

Returns true if the character is a whitespace character used to separate
words in a single line.

### Example {.unnumbered .unlisted}

``` {.c .numberLines}
//             testing this char
//                      v
printf("%s\n", isblank(' ')? "yes": "no");   // yes
printf("%s\n", isblank('\t')? "yes": "no");  // yes
printf("%s\n", isblank('\n')? "yes": "no");  // no
printf("%s\n", isblank('a')? "yes": "no");   // no
printf("%s\n", isblank('?')? "yes": "no");   // no
```

### See Also {.unnumbered .unlisted}

[`isspace()`](#man-isspace)

[[pagebreak]]
## `iscntrl()` {#man-iscntrl}

Test if a character is a control character

### Synopsis {.unnumbered .unlisted}

``` {.c}
#include <ctype.h>

int iscntrl(int c);
```

### Description {.unnumbered .unlisted}

A _control character_ is a local-specific non-printing character.

For the "C" locale, this means control characters are in the range 0x00
to 0x1F (the character right before SPACE) and 0x7F (the DEL character).

Basically if it's not an ASCII (or Unicode less than 128) printable
character, it's a control character in the "C" locale.
 
### Return Value {.unnumbered .unlisted}

Returns true if `c` is a control character.

### Example {.unnumbered .unlisted}

``` {.c .numberLines}
//             testing this char
//                      v
printf("%s\n", iscntrl('\t')? "yes": "no");  // yes (tab)
printf("%s\n", iscntrl('\n')? "yes": "no");  // yes (newline)
printf("%s\n", iscntrl('\r')? "yes": "no");  // yes (return)
printf("%s\n", iscntrl('\a')? "yes": "no");  // yes (bell)
printf("%s\n", iscntrl(' ')? "yes": "no");   // no
printf("%s\n", iscntrl('a')? "yes": "no");   // no
printf("%s\n", iscntrl('?')? "yes": "no");   // no
```

### See Also {.unnumbered .unlisted}

[`isgraph()`](#man-isgraph)


[[pagebreak]]
## `example()` {#man-example}

Tests if a character is a digit

### Synopsis {.unnumbered .unlisted}

``` {.c}
#include <ctype.h>

int isdigit(int c);
```

### Description {.unnumbered .unlisted}

Tests if `c` is a digit in the range `0`-`9`.

### Return Value {.unnumbered .unlisted}

Returns true if the character is a digit, unsurprisingly.

### Example {.unnumbered .unlisted}

``` {.c .numberLines}
//             testing this char
//                      v
printf("%s\n", isdigit('0')? "yes": "no");   // yes
printf("%s\n", isdigit('5')? "yes": "no");   // yes
printf("%s\n", isdigit('a')? "yes": "no");   // no
printf("%s\n", isdigit('B')? "yes": "no");   // no
printf("%s\n", isdigit('?')? "yes": "no");   // no
```

### See Also {.unnumbered .unlisted}

[`isalnum()`](#man-isalnum)

<!--
[[pagebreak]]
## `example()` {#man-example}

### Synopsis {.unnumbered .unlisted}

``` {.c}
```

### Description {.unnumbered .unlisted}

### Return Value {.unnumbered .unlisted}

### Example {.unnumbered .unlisted}

``` {.c .numberLines}
```

### See Also {.unnumbered .unlisted}

[`example()`](#man-example),
-->