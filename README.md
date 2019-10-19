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
Tempest templates have only two options 1) replace a string or 2) insert a file contents.
Data for populating the templates is supplied in a simple flat JSON file and/or command line arguments.

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
Or with the JOSN file
```
{
    "name":"Matthew"
}
```

The `$$` literal can be escaped using `$$$$`.

If the substitution pattern is contained in another string (e.g. "$names") then the escaped pattern `${name}` may be used instead.

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
My name is $$online.name
```

While compiling with `tempest tempalte.txt -i config.json` produces:
```
My name is mgrosvnor
```

## Builtins
A number of variables are predefined. These cannot be overridden.

`$$__OUT__`
The output file name as given by the -o "out" command line

`$$__FILE__`
The current file name that the template engine is working on (e.g. from includes)

`$$__TEMPLATE__`
The template name that the engine is working on on

`$$__DICTS__`
Sources of dictionaries for the template engine

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
