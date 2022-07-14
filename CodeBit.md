---
markdown: gfm
title: CodeBits
layout: default
---
# CodeBit
A CodeBit is an application of [FileMeta Principles](/Manifesto.html) to accomplish lightweight code sharing. The unit of sharing is a single source code file called a CodeBit. Each CodeBit file begins with metadata encapsulated in a comment so that it doesn't interfere with the function of the code itself. The metadata follows the [SoftwareSourceCode](https://schema.org/SoftwareSourceCode) schema from [Schema.org](https://schema.org) and is encoded in [MetaTag](/MetaTag) format.

The metadata includes a keyword indicating that the file is a CodeBit, the name, a URL to the latest version, and a version number. Optional fields include description, copyright, license, and so forth.

This page includes the following sections:

* [CodeBit Specification](#spec): The (very simple) specification for CodeBits
* [Sample Metadata Block](#sample): A sample CodeBit metadata block.
* [Known CodeBits](#directory): A short but growing list of known CodeBits.

Also see the [Blog](/blog) for history and new developments.

Coming soon will be a specification for simple CodeBit repositories, a console application for downloading and validating CodeBits, and my own repository of CodeBits.

## <a name="spec"></a>CodeBit Specification

Specification metadata in MetaTag format

&name="CodeBit Specification" <br/>
&version=2.0 <br/>
&author="Brandt Redd" <br/>
&date=2022-07-14 <br/>
&license=https://creativecommons.org/licenses/by/4.0/ <br/>
&description="A specification for reusable source code that is programming language independent." <br/>
&url=http://www.filemeta.org/CodeBit <br/>

In the following text, ALL CAPS key words should be interpreted per [RFC 2119](https://tools.ietf.org/html/rfc2119).

CodeBits are source code files with a metadata block near the beginning of the file. The metadata is in [MetaTag](/MetaTage) format and uses metadata property definitions from the [SoftwareSourceCode type](http://schema.org/SoftwareSourceCode) of [Schema.org](http://schema.org). At a minimum, the metadata block MUST include the 'url', 'version', and 'keywords' properties and 'CodeBit' MUST appear in the keywords. RECOMMENDED properties include 'name', 'description', and 'license'. Other metadata properties are optional.

The version number SHOULD use [semantic versioning](http://semver.org). 

Typically the metadata block is enclosed by multiline comment delimiters appropriate to the programming language. Single-line comment delimiters may also be used.

## <a name="sample"></a>Sample Metadata Block
Here is a sample metadata block for a C# source code file.

```cs
/*
&name=MySharedCode.cs
&description="Shared code demonstration module"
&url=https://github.com/FileMeta/AcmeIndustries/raw/main/MySharedCode.cs
&version=1.4
&keywords=CodeBit
&dateModified=2017-05-24
&copyrightHolder=Henry Higgins
&copyrightYear=2017
&license=https://opensource.org/licenses/BSD-3-Clause
*/
```
## <a name="directory"></a>Known CodeBits

The following is a short (but growing) list of known CodeBits. Some of these still use the CodeBit 1.0, YAML format for metadata but they will be updated before the end of 2022

* [ConsoleHelper](https://github.com/FileMeta/ConsoleHelper): (C#) A class for making .Net Framework console applications more friendly when invoked from a debugger or a shortcut.
* [DateTag](https://github.com/FileMeta/DateTag): (C#) A class that represents date/time metadata including timezone and precision information often neglected by other date parsers and formatters.
* [ExifToolWrapper](https://github.com/FileMeta/ExifToolWrapper): (C#) A wrapper class for Phil Harvey's excellent [ExifTool](https://www.sno.phy.queensu.ca/~phil/exiftool/).
* [HtmlReader](https://github.com/FileMeta/HtmlReader): (C#) A compact and full-featured HTML parser for .NET that implements the XmlReader interface.
* [IsomCoreMetadata](https://github.com/FileMeta/IsomCoreMetadata) (C#) A class for retrieving and updating core metadata from files in .MP4, .MOV, .M4A, and other ISO Based Media Format files.
* [MetaTag](https://github.com/FileMeta) A class for embedding and extracting metadata tags in free-form text fields according to the proposed MetaTag specification.
* [MicroYaml](https://github.com/FileMeta/MicroYaml): (C#) A simple parser and serializer for the MicroYaml dialect of the YAML file format.
* [TimeZoneTag](https://github.com/FileMeta/TimeZoneTag): (C#) A class for parsing and formatting TimeZone metadata.
* [WinShellPropertyStore](https://github.com/FileMeta/WinShellPropertyStore): (C#) .NET Wrapper classes for the Windows Property System.
