% Tempest Version 0.1 | A really ridiculously stupidly simple template engine. 
 
NAME
====

**tempest** — performs template substitutions.  

SYNOPSIS
========

| **tempest** template [-D DEFS]  [-i INCLS] [-I INCL_DIRS] [-o OUT] [-h]

DESCRIPTION
===========

Tempest is a really ridiculously stupidly simple template engine.
It is designed to make it easy to build complext text files using simple templates. 
Tempest templates have only two options 1) replace a string or 2) insert a file contents. 
Data for populating the templates is supplied in a simple flat JSON file and/or command line arguments. 

Options
-------
Tempest is designed to have familiar arguments to C compiler (e.g. GCC, CLANG). 
This shuold make it easy to integrate into Makefile environments. 


temp

:    Template file to peform replacements on

-h, --help

:   Prints brief usage information.

-D DEFS

:   Manual definitions in the form "name=value"
  
-i INCLS      

:     One or more JSON dictoaries defining insertions/replacements map


-I INCL_DIRS  

:    Direcotry to look in for inclusion files 


-o OUT

:   Outputs the result to the given filename.

    The file must be an **open(2)**able and **write(2)**able file.

-v, --version

:   Prints the current version number.

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

If the subsitution pattern is contined in another string (e.g. "$names") then the escaped pattern `${name}` may be used instead. 

## File Insertion
File insetion is performed using the `##` special symbol. For example
```
##config
```
With the command line option
```
-Dconfig=/etc/config.conf
```
Or with the JOSN file
```
{
    "confif":"/etc/config.conf"
}
```

BUGS
====

See GitHub Issues: <https://github.com/mgrosvenor/tempest/issues>

AUTHOR
======
Copyright (C) 2019 Matthew P. Grovenor 











