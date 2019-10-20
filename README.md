% Tempest Version 1.0 | A really ridiculously stupidly simple template engine.

NAME
====

**tempest** — performs template substitutions.  

SYNOPSIS
========

| **tempest** TEMPLATE [-D DEFS]  [-i INCLS] [-I INCL_DIRS] [-C CTXTS] [-o OUT] [-W WARN] [-h]

DESCRIPTION
===========

Tempest is a really ridiculously stupidly simple template engine.
It is designed to make it easy to build complex text files using simple templates.
Tempest templates have only two options 1) replace a string or 2) insert a file.
String replacement or insertion can be performed in a given "context", which can be specified either on the command line, or inline.
String replacement, within a context, turns out to be a simple, but powerful way to construct large text files (like web pages).
Data for populating the templates is supplied in a simple JSON file and/or command line arguments.

Options
-------
Tempest is designed to have familiar arguments to C compiler (e.g. GCC, CLANG).
This should make it easy to integrate into Makefile environments.

TEMPLATE
Template file to perform replacements on.

-D DEFS
Manual definitions in the form "name=value"

-i INCLS      
One or more JSON dictionaries defining insertions/replacements map

-I INCL_DIRS  
Directory to look in for inclusion files

-C CTXT
Global context identifier

-o OUT
Outputs the result to the given filename.

-W WARN
Changes warning parameters. Supported options are
* error - turn all warnings into errors
* no-overwrite - ignore multiple definitions in dictionaries that will overwite each other
* no-overwrite-context - as above, but when contexts are used
* no-file-not-found - ignore file not found warnings
* no-define-fail - ignore poor definitions on the command line (using -D)

-v, --version
Prints the current version number.

-h, --help
Prints brief usage information.


USAGE
=========
## String replacement
String replacement is performed using the `$$` special symbol. For example
```
My name is $$name
```
With the command line option
```
-Dname=Matthew
```
Or with the JOSN configuration file
```
{
    "name":"Matthew"
}
```

The `$$` literal can be escaped using `$$$$`.

If the substitution pattern is contained in another string (e.g. "$$names") then the escaped pattern `$${name}` may be used instead.

Replacement is lazy, so replacement strings can be contained in the JSON file, as long as the replacement string is defined at the time of evaluation. e.g. The following configuration is valid:
```
{
    "full_name": "$$first_name $$last_name",
    "first_name":"Matthew",
    "last_name":"Grosvenor"
}
```


## File Insertion
File insertion is performed using the `##` special symbol.
To denote file replacements, prepend the file name with `##` in your mapping. For example, using the command line option:
```
-Dconfig=##/etc/config.conf
```
Or with the JOSN file
```
{
    "config":"##/etc/config.conf"
}
```

## Context
String replacement/file insertion can be performed in a specific context. This allows the same template to be used for many different outputs. For example

template.txt
```
My name is $$name
```

with config.json file
```
{
    "real":
    {
        "name":"Matthew"
    },
    "online":
    {
        "name":"mgrosvenor"
    }
}
```

Compling with `tempest tempalte.txt -i config.json -C real` produces:
```
My name is Matthew
```

While compiling with `tempest tempalte.txt -i config.json -C online` produces:
```
My name is mgrosvnor
```

Context information can also be added inline, for example
```
My real name is $$name.real
My online name is $$name.online

```

While compiling with `tempest tempalte.txt -i config.json` produces:
```
My real name is Matthew
My online name is mgrosvnor

```

## Builtins
A number of variables are predefined. These cannot be overridden.

`$$__OUT__`
The output file name as given by the -o "out" command line

`$$__FILE__`
The current file name that the template engine is working on (e.g. from includes)

`$$__TEMPLATE__`
The template name that the engine is working on, given by the input argument TEMPLATE

`$$__DICTS__`
Sources of dictionaries for the template engine (e.g. -i)

`$$__FILES__`
Sources of files for this compilation

`$$__CMD__`
The complete command line given to Tempest

`$$__DATE__`
Today's date / time

`$$__TE__`
Name of the template engine

`$$__TE_MAJOR__`
Major version of the template engine

`$$__TE_MINOR__`
Minor version number of the template engine

`$$__TEVER__`
Version string for the template engine


BUGS
====

See GitHub Issues: <https://github.com/mgrosvenor/tempest/issues>

AUTHOR
======
Copyright (C) 2019 Matthew P. Grosvenor
