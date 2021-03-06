[BACK TO INDEX](../README.md)
__________
### Styling Standards

>Code is read much more often than it is written.
>     - A Wise Pythonista

 
We're focusing on Python development on this workshop. Python has its own styling
guide and patterns (which can actually also be applied to most of the existing
programming languages). These guide is called [PEP8](http://www.python.org/dev/peps/pep-0008/), 
and we will be establishing as standard at SciLifeLab the following subset
of these rules.:

####4 spaces per indentation level
Use 4 spaces per indentation level -- don't use tabs.
Spaces are preferred over tabs for the following reason: spaces are spaces on every
editor on every operating system. Tabs can be configured to act as 2, 4, 8 or whichever
number of *"spaces"* and this can make code unreadable when being shared.

This does not mean that you can't use the tab button. You can usually set your
editor to insert four spaces in place of a tab; in vim, you can add this code to your .vimrc:

```
filetype plugin indent on
autocmd FileType python set shiftwidth=4 | set expandtab | set tabstop=4 | set softtabstop=4 | set autoindent
```

You can enable similar functionality in [Xcode](http://stackoverflow.com/a/16263490/1256058) or in Eclipse by using PyDev.

####Indentation of Multi-Line Statements

When using a hanging indent, the following considerations should be applied:
* there should be no arguments on the first line; and
* further indentation should be used to clearly distinguish continuation lines from normal code.

An example to clarify:

Yes!
```python
# Continuation line aligned with opening delimiter
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# Indentation used to distinguish the continuation from the next lines of code:
def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)
```
No!
```python
# Arguments should not be used on the first line
#   when not using vertical alignment of continuation lines
#   (compare to first example, above):
foo = long_function_name(var_one, var_two,
    var_three, var_four)

# Further indentation required as indentation is not distinguishable from the
#   next lines of code:
def long_function_name(
    var_one, var_two, var_three,
    var_four):
    print(var_one)
```

####Maximum line length _(soft rule)_
Limit all lines to a maximum of 79 characters. Break down the line if it exceeds
the maximum length. For example:

```python
with open('/path/to/some/file/you/want/to/read') as file_1, \
        open('/path/to/some/file/being/written', 'w') as file_2:
    file_2.write(file_1.read())
```

**Tip:** Set up your editor to show you a line on the column 80, so you can 
easily see when your line is becoming too long:

<p align="center">
  <img src="https://raw2.github.com/guillermo-carrasco/BestPracticesWorkshop/master/images/vim_maxlength.png"
       alt="VIM configured to show maximum column length"/>
</p>

To do this in vim, you can add the following to your .vimrc:

```
if exists('+colorcolumn')
      let &colorcolumn="80"
      highlight ColorColumn ctermbg=235 guibg=#2c2d27
else
      au BufWinEnter * let w:m2=matchadd('ErrorMsg', '\%>80v.\+', -1)
endif
```

####Imports
Imports should usually be on separate lines, e.g.:

Yes:
```python
import os
import sys
```
No:
```python
import os, sys
```

Multiple submodule imports on the same line are okay:
```python
from subprocess import Popen, PIPE
```

Wildcard imports (```from <module> import *```) should be avoided, as they make
it unclear which names are present in the namespace, confusing both readers and
many automated tools. In case that the name of the module is very long and even
difficult readability, you can always make an alias:
```
import very_long_and_weird_module_name as long_module
```

It is also nice to alphabetize your imports though I doubt this is a convention.
Also, compartmentalize "from"-style imports separate from basic ones:
```
import collections
import numpy as np
import shutil

from matplotlib import pyplot as plt
from subprocess import check_output
```

####Whitespace in Expressions and Statements
Avoid extraneous whitespace in the following situations:

* Immediately inside parentheses, brackets or braces.

```
Yes: spam(ham[1], {eggs: 2})
No:  spam( ham[ 1 ], { eggs: 2 } )
```

* Immediately before a comma, semicolon, or colon:

```
Yes: if x == 4: print x, y; x, y = y, x
No:  if x == 4 : print x , y ; x , y = y , x
```

* Immediately before the open parenthesis that starts the argument list of a function call:

```
Yes: spam(1)
No:  spam (1)
```

* Immediately before the open parenthesis that starts an indexing or slicing:

```
Yes: dict['key'] = list[index]
No:  dict ['key'] = list [index]
```

* More than one space around an assignment (or other) operator to align it with another.

Yes:
```
x = 1
y = 2
long_variable = 3
```
No:
```
x             = 1
y             = 2
long_variable = 3
```

####Comments

#####How to Think About Comments
Always remember that the point of your comments is to help people reading it
understand two things:

* What the code does
* Why it does it

You should attempt to make the code "self-documenting" by using descriptive
names for functions and variables; when the reason for doing something
cannot be made clear, however, you can add a comment to explain the reason
for a piece of code.

For example:

No:
```python
def c_disp(v=0, t):
    g = 9.81
    return v*t + (1/2) * g * (t^2)
```

Yes:
```python
def calculate_displacement(velocity=0, time=0, acceleration=9.81):
    # We're using Newtonian physics here because quantum computing is too expensive
    return velocity*time + (1/2)(acceleration * time^2)
```

Yes!! Docstrings!
```python
def calculate_displacement(velocity=0, time=0, acceleration=9.81):
    """Calculate displacement of an object using the Newtonian equation
        x = vₒt + ½at²

    Default value for accleration is that for Earth near the surface.

    Keyword arguments:
      velocity: starting velocity
      time: elapsed time

    Returns a float of the displacement.

    """
    # We're using Newtonian physics here because quantum computing is too expensive
    return float(velocity*time + (1/2)(acceleration) * time^2))
```

Comments that contradict the code are worse than no comments; however,
**you should always make it a priority to keep the comments up-to-date
when the code changes!**

#####Comment Formatting
Comments should be complete sentences. If a comment is short, the period at the
end can be omitted. Block comments generally consist of one or more paragraphs
built out of complete sentences, and each sentence should end in a period.

Python coders from non-English speaking countries: please write your comments in
English, unless you are 120% sure that the code will never be read by people who
don't speak your language. Python's author is Dutch, and if he can do it,
you can do it.

#####Inline comments
Use inline comments infrequently.

An inline comment is a comment on the same line as a statement. Inline comments
should be separated by at least two spaces from the statement. They should start
with a `#` and a single space.

Inline comments are rarely necessary and in fact distracting if they state the obvious.

Don't do this:
```python
x = x + 1                 # Increment x
```

But this is useful:
```python
x = x + 1                 # Compensate for border
```

#####Documentation strings (Docstrings)
A docstring is a string literal that occurs as the first statement in a module,
function, class, or method definition. Such a docstring becomes the ```__doc__```
 special attribute of that object.

Write docstrings for all public modules, functions, classes, and methods.
Docstrings are not necessary for non-public methods, but you should have a comment
that describes what the method does. This comment should appear after the def line.

Use the docstrings to document the parameters of your function, so other people
can easily understand what the function does:
```python
def complex(real=0.0, imag=0.0):
    """Form a complex number.

    Keyword arguments:
      real -- the real part (default 0.0)
      imag -- the imaginary part (default 0.0)

    Returns a float of the complex number.

    """
    if imag == 0.0 and real == 0.0: return complex_zero
    ...
```

__Suggestion__: Use [sphinx](http://sphinx-doc.org/)-style docstrings to generate 
readable documentation that can also be used to auto-generate full web-based docs.

####Naming conventions
There are a lot of different naming styles. It helps to be able to recognize what 
naming style is being used, independently from what they are used for.

In order to have an homogeneous-looking code, we will follow some simple naming
conventions:
* _Package and Module Names_: Modules should have short, all-lowercase names. Underscores
can be used in the module name if it improves readability. Since module names are mapped 
to file names, and some file systems are case insensitive and truncate long names, 
it is important that module names be chosen to be fairly short.
* _Class names_: Class names should normally use the CapWords convention (also called CamelCase).

```python
class MyNewClass(object):
```

* _Exception names_: Because exceptions should be classes, the class naming convention 
applies here.

```python
class MyNewException(Exception):
```

* _Function Names_: Function names should be lowercase, with words separated by underscores 
as necessary to improve readability.

```python
def my_new_function():
```

* _Function and method arguments_:
    * Always use ```self``` for the first argument to instance methods.
    * Always use ```cls``` for the first argument to class methods.
    * If a function argument's name clashes with a reserved keyword, it is generally 
    better to append a single trailing underscore rather than use an abbreviation or 
    spelling corruption. Thus class_ is better than clss. (Perhaps better is to avoid 
    such clashes by using a synonym.)
* _Method Names and Instance Variables_: 
    * Use the function naming rules: lowercase with words separated by underscores 
    as necessary to improve readability. 
    * Use one leading underscore only for non-public methods and instance variables.

```python
class MyNewClass(object):
    def _my_new_class_method(cls):
        ...

    def my_new_instance_method(self):
       ...
```

* _Constants_: Constants are usually defined on a module level and written in all 
capital letters with underscores separating words. Examples include MAX_OVERFLOW and TOTAL.

####Other programming recommendations
* Comparisons to singletons like `None` should always be done with `is` or `is not`, never 
the equality operators. _NOTE_: Also, beware of writing `if x` when you really mean
`if x is not None` -- e.g. when testing whether a variable or argument that defaults 
to `None` was set to some other value.
* When catching exceptions, mention specific exceptions whenever possible instead 
of using a bare `except`: clause. For example:
```python
try:
    import platform_specific_module
except ImportError:
    platform_specific_module = None
```
Better than:
```python
try:
    import platform_specific_module
except:
    platform_specific_module = None
```
A bare `except:` clause will catch `SystemExit` and `KeyboardInterrupt` exceptions, 
making it harder to interrupt a program with Control-C, and can disguise other problems.
* Use `''.startswith()` and `''.endswith()` instead of string slicing to check for prefixes or 
suffixes. `startswith()` and `endswith()` are cleaner and less error prone. For example:
```
Yes: if foo.startswith('bar'):
No:  if foo[:3] == 'bar':
```
* For sequences, (strings, lists, tuples), use the fact that empty sequences are false.
```
Yes: if not seq:
     if seq:

No: if len(seq)
    if not len(seq)
```
* Don't compare boolean values to True or False using ==:
```
Yes:   if greeting:
No:    if greeting == True:
Worse: if greeting is True:
```
__________
[BACK TO INDEX](../README.md)
