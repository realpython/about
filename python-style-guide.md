# Python Style Guide

- [PEP8](#pep8)
- [General conventions](#general-conventions)
- [Python version support](#python-version-support)
    - [Python version 2 and version 3](#python-version-2-and-version-3)
- [Line wrapping](#line-wrapping)
- [Private interfaces](#private-interfaces)
- [Docstrings](#docstrings)
- [Logging](#logging)
- [pylint](#pylint)
- [References](#references)

## PEP8

[PEP8 - Style Guide for Python Code][PEP8] is THE guide for programming in python. You should pretty much adhere to everything there; the next sections mainly just clarifies certain parts of it.

## General conventions

- Indentation is 4 spaces, no tabs
- Remove all trailing spaces including on blank lines
- Don't use `from modulename import *` unless it's by convention. For example, fabric assumes you are going to `import *`.
- Use a `if __name__ = '__main__'` block which then calls a main function unless you are 100% sure this module will never be called directly. You can also turn off the top line from the script that's usually there to invoke the python interpreter if it's going to only be imported
- Use modules from the standard library versus third party to have as few dependencies as possible.
- 'Constants' (like [Private interfaces](#private-interfaces) this is by convention only) should be at top of the file and in all caps. The use of script wide constants are generally discouraged as they pollute the global name space. If you have options that are used in several other functions then perhaps you should make a class.

## Python version support

Minimum version should be 2.6 - because CentOS 6 uses 2.6. If this code is running on just about anything else (Ubuntu, Debian, OS X) then can use 2.7.x.

### Python version 2 and version 3

Where possible code should be made to run in both version. At a minimum the following `__future__` imports should be used:

```python
from __future__ import (absolute_import, print_function, unicode_literals, division)
```

> Both `print_function` and `unicode_literals` were added in 2.6 thus this line would break in version 2.5 and older.

There are a couple good resources to help with writing code that's both compatible in 2 and 3. See [Porting to Python 3][Port2Python] and [python3porting.com][python3porting].

## Line wrapping

For continuing long lines use the second of these two:

```python
# Aligned with opening delimiter
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# More indentation included to distinguish this from the rest.
def long_function_name(var_one, var_two, var_three,
        var_four):
    print(var_one)
```

The former method is more popular in other projects but if you're already calling a long function there's going to be a lot of wasted space, so just use a double-indent and gain the vertical space.

## Private interfaces

Unfortunately, python doesn't support a way to declare something is officially public or private. Instead a convention has come up: Anything prefixed with a single underscore should be considered private. Nothing is stopping you from importing it other than you *shouldn't*. There's also the option of using two underscores but this does some mangling of the actual name to avoid any collisions - so it's good to avoid this approach.

Err towards making objects private rather than public. You can always make a private object public in the future but making a public object private could cause issues.

## Docstrings

The top of the file, all functions, and all classes should have a docstring.

Example docstring for top of file:

```python
"""Nifty Title.

This is a nifty module. Here I would put a longer description wrapped at 80
characters.

Requirements
    Python v2.6 or higher: Makes use of several functions introduced in 2.6

Environment Variables
    LOGLEVEL: overrides the level specified here. Choices are debug, info,
        warning, error, and critical. (default: 'warning')

"""
```

Example of a function docstring:

```python
def sample_def(meh=None):
    """
    This is a sample function.

    Long description would go here.

    Args:
        meh: I guess this would have something here

    Returns:
        The string 'hello'

    """
    return 'hello'
```

Example of a class docstring:

```python
class SampleClass():
    """
    This is a sample class.

    Just illustrating how a docstring would look.

    Attributes:
        blah: This stores some number of blahs

    """
    def __init__(self):
        """Inits SampleClass."""
        self._log = logging.getLogger(__name__)
        self.blah = 3

    def get_blah(self):
        """Returns the value of blah."""
        self._log.debug('Returning blah')
        return self.blah
```

These were all taken pretty much verbatim from [google's python style guide](http://google-styleguide.googlecode.com/svn/trunk/pyguide.html#Comments).

## Logging

Logs MUST include a time stamp in ISO8601 format. Suggested format string:

```python
_LOGFORMAT = "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
logging.basicConfig(level=logging.WARNING, format=_LOGFORMAT)
log = logging.getLogger(__name__)
```

Log levels should follow the same conventions laid out in the [python documentation](http://docs.python.org/2/howto/logging.html#logging-basic-tutorial). These are in order from most verbose to least. So a log level of `ERROR` would only show `ERROR` and `CRITICAL` log messages:

- **DEBUG** - Information that is only of use to developers and possibly for troubleshooting issues. Should only be enabled during development.
- **INFO** - Information that may be of use to end users. An example of this is when contacting an external site. At `INFO` level just say "Now contacting (insert just the hostname here)" and then at `DEBUG` level it would change to "calling (full url with request parameters)".
- **WARNING** - For reporting on items that are not configured properly. Deprecation notices could go in this layer too. If whatever caused the message needs attention but didn't actually prevent anything - the python docs give an example of disk space starting to get low - this is the default log level.
- **ERROR** - When something did not work (e.g., invalid parameter to a function)
- **CRITICAL** - A serious error has happened that may cause the program to close. It's analogous to 'fatal' in other logging libraries.

## Pylint

Use [pylint](http://www.pylint.org/) to help find common errors. The only thing that needs modified from a base install is to add `fh` as a valid variable name. To do this add `fh` to the end of `good-names` in your `.pylintrc` file. With default values this makes the final line `good-names=i,j,k,ex,Run,_,fh`.

## References

- [Style Guide for Python Code (PEP0008)][PEP8]
- [Porting to Python 3][Port2Python]
- [python3porting.com][python3porting] - This has more detail than other sites
- [Code Like a Pythonista: Idiomatic Python](http://python.net/~goodger/projects/pycon/2007/idiomatic/handout.html) - Good info into the getchas in python
- [Best Practices for Python Scripting](http://cdn.oreillystatic.com/en/assets/1/event/27/Best%20practices%20for%20_scripting_%20with%20Python%203%20Paper.pdf) (pdf) - quick reference of sorts
- [Python Project Howto](http://infinitemonkeycorps.net/docs/pph/) - How to structure a project for publishing
- [Google Python Style Guide](http://google-styleguide.googlecode.com/svn/trunk/pyguide.html) - One of the few style guides to be explicit in exactly how the docstring for modules and classes should be structured.


 [PEP8]: http://www.python.org/dev/peps/pep-0008/ "Style Guide for Python Code"
 [Port2Python]: http://docs.pythonsprints.com/python3_porting/index.html "Porting to Python 3"
 [python3porting]: http://python3porting.com/ "Python 3 Porting"
