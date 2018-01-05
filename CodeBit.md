---
markdown: gfm
title: CodeBits
layout: default
---
# CodeBits
CodeBits are an application of [FileMeta Principles](/Manifesto.html) to accomplish lightweight code sharing. The unit of sharing is a single source code file called a CodeBit. Each CodeBit file begins with YAML metadata encapsulated in a comment so that it doesn't interfere with the function of the code itself.

The metadata includes a keyword indicating that the file is CodeBit, name,  URL to the master copy, and version number. Optional fields include description, copyright, license, and so forth.

This page includes the following sections:

* [FMCodeBit Application](#application): A description of the FMCodeBit utility application.
* [CodeBit Specification](#spec): The (very simple) specification for  CodeBits
* [Sample Metadata Block](#sample): A sample CodeBit metadata block.
* [Known CodeBits](#directory): A short but growing list of known CodeBits. 

## <a name="application"></a>FMCodeBit Application
The [FMCodeBit application](https://github.com/FileMeta/FMCodeBit) is used to keep CodeBits up to date. It checks source files and reads the metadata to determine which are CodeBits. For each CodeBit it finds, the application checks the master copy of that CodeBit on the web to determine whether it is up to date. If a newer copy is available, the application prompts the user for confirmation and then updates the CodeBit.

## <a name="spec"></a>CodeBit Specification
In the following text, ALL CAPS key words should be interpreted per [RFC 2119](https://tools.ietf.org/html/rfc2119).

CodeBits are source code files with a metadata block near the beginning of the file. The metadata is in [MicroYaml](https://github.com/FileMeta/MicroYaml) format (a subset of [YAML](http://www.yaml.org)) and uses metadata property definitions from the [SoftwareSourceCode type](http://schema.org/SoftwareSourceCode) of [Schema.org](http://schema.org). At a minimum, the metadata block MUST include the 'url', 'version', and 'keywords' properties and 'CodeBit' MUST appear in the keywords. RECOMMENDED properties include 'name', 'description', and 'license'. Other metadata properties are optional.

The version number SHOULD use [semantic versioning](http://semver.org). 

It MUST begin with a YAML 'Begin Document' indicator which is a line with just three dashes. It MUST end with a YAML 'End Document' indicator which is a line with just three dots.

Typically the metadata block is enclosed by multiline comment delimiters appropriate to the programming language. Presently, programming languages that lack multiline comments are not supported.

## <a name="sample"></a>Sample Metadata Block
Here is a sample metadata block for a C# source code file.

```cs
/*
---
# Metadata in YAML format (This is a YAML comment)
name: MySharedCode.cs
description: Shared code demonstration module
url: https://github.com/FileMeta/AcmeIndustries/raw/master/MySharedCode.cs
version: 1.4
keywords: CodeBit
dateModified: 2017-05-24
copyrightHolder: Henry Higgins
copyrightYear: 2017
license: https://opensource.org/licenses/BSD-3-Clause
...
*/
```
## Known CodeBits

The following is a short (but growing) list of known CodeBits.

* [ConsoleHelper](https://github.com/FileMeta/ConsoleHelper): (C#) A class for making console applications more friendly when invoked from a debugger or a shortcut.
* [HtmlReader](https://github.com/FileMeta/HtmlReader): (C#) A compact and full-featured HTML parser for .NET that implements the XmlReader interface.
* [MicroYaml](https://github.com/FileMeta/MicroYaml): (C#) A simple parser for the MicroYaml dialect of the YAML file format.
* [WinShellPropertyStore](https://github.com/FileMeta/WinShellPropertyStore): (C#) .NET Wrapper classes for the Windows Property System.
