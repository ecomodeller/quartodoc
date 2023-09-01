# Overview

quartodoc lets you quickly generate python package documentation, using
markdown and [quarto](https://quarto.org). It is designed as an
alternative to Sphinx.

Check out the screencast below for a full walkthrough of creating a
documentation site, or read on for instructions on installation and use.

<br>

<div style="position: relative; padding-bottom: 64.5933014354067%; height: 0;">

<iframe src="https://www.loom.com/embed/fb4eb736848e470b8409ba46b514e2ed?sid=31db7652-43c6-4474-bab3-19dea2170775" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;">
</iframe>

</div>

<br> <br>

## Installation

``` bash
python -m pip install quartodoc

# or from github
python -m pip install git+https://github.com/machow/quartodoc.git
```

<div>

> **Note**
>
> In order to install quarto, see the quarto [get started
> page](https://quarto.org/docs/get-started/).

</div>

## Basic use

Getting started with quartodoc takes two steps: configuring a quarto
website, and generating documentation pages for your library.

First, create a `_quarto.yml` file with the following:

``` yaml
project:
  type: website

# tell quarto to read the generated sidebar
metadata-files:
  - _sidebar.yml


quartodoc:
  # the name used to import the package
  package: quartodoc

  # write sidebar data to this file
  sidebar: _sidebar.yml

  sections:
    - title: Some functions
      desc: Functions to inspect docstrings.
      contents:
        # the functions being documented in the package.
        # you can refer to anything: class methods, modules, etc..
        - get_object
        - preview
```

Next, run this command to generate your API pages:

``` bash
quartodoc build
```

This should create a `reference/` directory with an `index.qmd` and
documentation pages for listed functions, like `get_object` and
`preview`.

Finally, preview your website with quarto:

``` bash
quarto preview
```

## Rebuilding site

You can preview your `quartodoc` site using the following commands:

First, watch for changes to the library you are documenting so that your
docs will automatically re-generate:

``` bash
quartodoc build --watch
```

Second, preview your site:

``` bash
quarto preview
```

## Looking up objects

Finding python objects to document involves two pieces of configuration:

- the package name.
- a list of objects for content.

Note that quartodoc can look up anything—whether functions, modules,
classes, attributes, or methods.

``` yaml
quartodoc:
  package: quartodoc
  sections:
    - title: Some section
      desc: ""
      contents:
        - get_object        # function: quartodoc.get_object
        - ast.preview       # submodule func: quartodoc.ast.preview
        - MdRenderer        # class: quartodoc.MdRenderer
        - MdRenderer.render # method: quartodoc.MDRenderer.render
        - renderers         # module: quartodoc.renderers
```

The functions listed in `contents` are assumed to be imported from the
package.

## Learning more

Go [to the next page](./get-started/basic-docs.qmd) to learn how to
configure quartodoc sites, or check out these handy pages:

- [Examples page](./examples/index.qmd): sites using quartodoc.
- [Tutorials page](./tutorials/index.qmd): screencasts of building a
  quartodoc site.
- [Docstring issues and examples](./get-started/docstring-examples.qmd):
  common issues when formatting docstrings.
- [Programming, the big picture](./get-started/dev-big-picture.qmd): the
  nitty gritty of how quartodoc works, and how to extend it.
