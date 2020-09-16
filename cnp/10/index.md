---
template: ../_cnp.html.jinja
title: cnp10
...

## Variants

<ul class="variants">
<li>
Always curly braces after if/else (major variant):
```c
if (foo) {
    bar();
    baz();
}
```
</li>
<li>
Single statements without curly braces after if/else (minor variant):
```c
if (foo)
    bar();
    baz();
```
</li>
</ul>

## Likely Pathogenic

Minor variant easily misinterpreted in environment of lots Python programming:

```python
if ((1 + 1) + (-1 + 1) != 2):
    print("This line of Python won't run")
    print("and this added line also won't run")
```

```c
if ((1 + 1) + (-1 + 1) != 2)
    printf("But after programming mostly in Python")
    printf("is it obvious this added line of C will ALWAYS run?")
```

## Related Links

[Use braces for the body of an if, for, or while statement](https://wiki.sei.cmu.edu/confluence/display/c/EXP19-C.+Use+braces+for+the+body+of+an+if%2C+for%2C+or+while+statement)

