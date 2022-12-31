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
* [Versioning](#versioning): A note on how to handle versioning of CodeBits.
* [Known CodeBits](#directory): A short but growing list of known CodeBits.

Also see the [Blog](/blog) for history and new developments.

Coming soon will be a specification for simple CodeBit repositories, a console application for downloading and validating CodeBits, and my own repository of CodeBits.

## <a name="spec"></a>CodeBit Specification

Metadata for this specification in MetaTag format

&name="CodeBit Specification" <br/>
&version=2.1-beta.2 <br/>
&author="Brandt Redd" <br/>
&datePublished=2022-11-09 <br/>
&license=https://creativecommons.org/licenses/by/4.0/ <br/>
&description="A specification for reusable source code that is programming language independent." <br/>
&url=http://www.filemeta.org/CodeBit <br/>

In the following text, ALL CAPS key words should be interpreted per [RFC 2119](https://tools.ietf.org/html/rfc2119).

CodeBits are source code files with a metadata block near the beginning of the file. The metadata are in [MetaTag](/MetaTag) format and uses metadata property definitions from the [SoftwareSourceCode type](http://schema.org/SoftwareSourceCode) of [Schema.org](http://schema.org). At a minimum, the metadata block MUST include the `name`, `url`, `version`, and `keywords` properties and "CodeBit" MUST appear in the keywords. RECOMMENDED properties include `datePublished`, `author`, `description`, `license`, and `dependencies`. Other metadata properties are optional and should be selected from [Schema.org](https://schema.org).


The metadata block SHOULD be enclosed by multiline comment delimiters appropriate to the programming language. Single-line comment delimiters MAY be used.

## <a name="sample"></a>Sample Metadata Block
Here is a sample metadata block for a C# source code file.

```cs
/*
&name=example.com/MySharedCode.cs
&description="Shared code demonstration module"
&url=https://github.com/FileMeta/AcmeIndustries/raw/main/MySharedCode.cs
&version=1.4.0
&keywords=CodeBit
&datePublished=2017-05-24
&author="Adam Smith"
&copyrightHolder="ACME Industries"
&copyrightYear=2017
&license=https://opensource.org/licenses/BSD-3-Clause
*/
```

## Properties
Properties follow the definitions in [schema.org](https://schema.org). These are specific details on how they are interpreted for codebits.

### name
The name SHOULD be the registered domain name of the publisher followed by a slash, followed by the preferred filename of the codebit. The filename SHOULD include the extension associated withthe programming language (e.g. "example.com/peacemaker.js"). Additional slashes MAY separate components of the name (e.g. "example.com/category/dwim.js") There SHOULD be no spaces in the name and there SHOULD NOT be consecutive slashes.

### url
The `url` property SHOULD be the URL of the most recent release of the CodeBit. That is, if a CodeBit is updated, the new version is placed at same URL where the prior version was located. Older versions may exist at other URLs. See the [Versioning](#versioning) section for details.

### version
The `version` value SHOULD use [semantic versioning](semver.org). Per that standard, the simplest form includes three numbers separated by periods.

### keywords
A codebit is distinguished from other source code files by the presence of the `CodeBit` keyword. Per the [MetaTag specification]() a multi-valued property like `keywords` includes a separate tag for each keyword. So, if the keywords "CodeBit" and "Collection" are both to be included, the code would be `&keywords=CodeBit &keywords=Collection`.

### datePublished
All dates, including `datePublished` MUST use [RFC 3339](https://www.rfc-editor.org/rfc/rfc3339) format which is a profile of [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). `datePublished` SHOULD be just a date. For example, "1986-12-25".

### dependencies
Names and optional version numbers of one or more codebits on which this codebit depends. The version is appended to the name using URL query string format. For example, "example.com/peacemaker.js?version=2.1.0"

Per the [MetaTag Specification](MetaTag) multiple dependencies may specified by using a semicolon-delimited list or by repeating the `dependencies` tag once per dependendency.

Here are some examples:

* dependencies=example.com/peacemaker.js?version=2.1.0;example.com/sorter.js?version=1.0.0
* dependencies="example.com/peacemaker.js?version=2.1.0";"example.com/sorter.js?version=1.0.0"
* dependencies=example.com/peacemaker.js<br/>
dependencies=example.com?version=1.0.0

Note that [dependencies](https://schema.org/dependencies) is defined by Schema.org but it is not defined for the [SoftwareSourceCode type](http://schema.org/SoftwareSourceCode). Nevertheless we use it for codebits.

## <a name="versioning"></a>A Note on Versioning

The `url` attribute of a CodeBit SHOULD NOT change when a new version is posted. That is, a new codebit version is posted at the same URL as prior versions.

When included, `datePublished` SHOULD be the date that this *version* was released for use.

CodeBits are incorporated into other software applications *by value*, not *by reference*. That is, the whole CodeBit file is included with an application's other source files and stored in the application's code repository. Thus, if a CodeBit is updated, it takes deliberate action by users of a CodeBit to update their software to a more recent release.

Prior versions of a CodeBit are located using the [CodeBit Directory](#codebit-directory).

## CodeBit Directory

*Work in Progress*
Directories are used to locate codebits by name and to find different versions of a codebit. The directory is in JSON-LD format and follows the [Open Competency Framework Collaborative Directory Data Model](https://docs.google.com/document/d/1EMdHnYsiXJiAfRRBx85q0oclj04sVtkuRr0ynSIowcg/edit).

A publisher of CodeBits creates a directory and publishes it with open access on the web. The URL of the directory MUST be provided in a DNS TXT record. If the directory is for `sample.com` then the TXT record should be defined on `_dir.sample.com`. The contents of the TXT record are "dir=" plus the URL of the directory. Thus, a directory located at "https://www.sample.com/codebits.json" would have the following in the TXT record: "dir=https://www.sample.com/codebits.json".

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
