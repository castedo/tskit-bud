---
template: ../_cnp.html.jinja
title: cnp11
...

## Variants

<ul class="variants">
<li>
"const before type" (ancestral):
```c
const int *p
const int **p
```
</li>
<li>
"const after type" (mutant):
```c
int const *p
int const **pp
```
</li>
</ul>


## Compilation

Synonymous


## Frequency

`const int` variant found in
[GNU Scientific Library](https://www.gnu.org/software/gsl/).

`int const` variant found in [Wikipedia page about
const](https://en.wikipedia.org/wiki/Const_(computer_programming)).

Both variants found in the
[CMU Software Engineering Institute CERT C Coding
 Standard](https://wiki.sei.cmu.edu/confluence/display/c/STR05-C.+Use+pointers+to+const+when+referring+to+string+literals).


## Human Interpretation

Possible incorrect interpretation of
`int const *` as compiling to something different than the
historically more common `const int *`.

`const int` is intuitively read as "constant integer" by English speakers.

`int const` is intuitively read as "intero constante" in Italian, or similarly
for other "noun-then-adjective" languages (e.g. all romance languages).


## Examples


[Spanglish](https://en.wikipedia.org/wiki/Spanglish) interpretation
of "const after type" variant in complex cases:

```c
int const * p1;  // 'int constante pointer'
int * const p2;  // 'int pointer constante'
```

```c
int const * * pp1;  // 'int constante pointer pointer'
int * const * pp2;  // 'int pointer constante pointer'
int * * const pp3;  // 'int pointer pointer constante'
```


Code example thanks to Kevin R. Thornton:

```c
#include <stdlib.h>
#include <stdio.h>
#include <assert.h>

/* This is what we really mean most of 
 * the time--complete immutability.
 * However, I find it close to unreadable,
 */
void
foo(const double * const x)
{
}

/* The most common form in my experience.
 * Also what the GSL docs use.
 * Arguably better-aligned w/intuition
 * about what we want.
 */
void
foo2(const double *x)
{
    // Fails to compile:
    // x[0] = 1;
    double *y = calloc(10, sizeof(double));
    y[0] = 1;
    x = y; // Dang, that feels wrong, but the caller won't see it.
    fprintf(stdout, "%lf\n", x[0]);
    free(y);
}

/* 
 * This is the "the pointer is constant" version.
 * However, this isn't what the goal is most 
 * of the time.  Here, the input data
 * are still mutable!
 */
void
foo3(double *const x)
{
    x[0] = 1;
    // Fails to compile:
    // double * y = NULL;
    // x = y;
}

int
main(int argc, char **argv)
{
    double *x = calloc(10, sizeof(double));

    foo(x);
    foo2(x); /* the calling environment's x in unaffected */
    assert(x != NULL);
    assert(x[0] == 0);
    fprintf(stdout, "%lf\n", x[0]);
    foo3(x); 
    assert(x[0] == 1);
}
```

# Related Links

[Const-qualify immutable objects](https://wiki.sei.cmu.edu/confluence/display/c/DCL00-C.+Const-qualify+immutable+objects)

