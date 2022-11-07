
# quartodoc

Generate python API documentation for quarto.

## Install

## Basic use

``` python
from quartodoc import get_function, MdRenderer

# get function object ---
f_obj = get_function("quartodoc", "get_function")

# render ---
renderer = MdRenderer(header_level = 1)
print(
    renderer.to_md(f_obj)
)
```

    # get_function

    `get_function(module: str, func_name: str, parser: str = 'numpy')`

    Fetch a function.

    ## Parameters

    | Name        | Type   | Description                | Default   |
    |-------------|--------|----------------------------|-----------|
    | `module`    | str    | A module name.             | required  |
    | `func_name` | str    | A function name.           | required  |
    | `parser`    | str    | A docstring parser to use. | `'numpy'` |

    ## Examples

    ```python
    >>> get_function("quartodoc", "get_function")
    <Function('get_function', ...

## How it works

quartodoc consists of two pieces:

- collection: using the library
  [griffe](https://github.com/mkdocstrings/griffe) to statically collect
  information about functions and classes in a program.
- docstring parsing: also handled by griffe, which breaks it into a tree
  structure.
- docstring rendering: use plum-dispatch on methods like
  MdRenderer.to_md to decide how to visit and render each piece of the
  tree (e.g. the examples section, a parameter, etc..).

Here is a quick example of how you can grab a function from griffe and
walk through it.

``` python
from griffe.loader import GriffeLoader
from griffe.docstrings.parsers import Parser

griffe = GriffeLoader(docstring_parser = Parser("numpy"))
mod = griffe.load_module("quartodoc")

f_obj = mod._modules_collection["quartodoc.get_function"]
```

``` python
f_obj.name
```

    'get_function'

``` python
docstring = f_obj.docstring.parsed
docstring
```

    [<griffe.docstrings.dataclasses.DocstringSectionText at 0x127ed2700>,
     <griffe.docstrings.dataclasses.DocstringSectionParameters at 0x127ed2580>,
     <griffe.docstrings.dataclasses.DocstringSectionExamples at 0x127ed2af0>]

Note that quartodoc’s MdRenderer can be called on any part of the parsed
docstring.

``` python
from quartodoc import MdRenderer

renderer = MdRenderer()

print(
    renderer.to_md(docstring[1])
)
```

    | Name        | Type   | Description                | Default   |
    |-------------|--------|----------------------------|-----------|
    | `module`    | str    | A module name.             | required  |
    | `func_name` | str    | A function name.           | required  |
    | `parser`    | str    | A docstring parser to use. | `'numpy'` |
