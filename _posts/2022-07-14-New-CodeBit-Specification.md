---
title: New CodeBit Specification
date: 2022-07-14
author: Brandt Redd
---
Today, I posted version 2.0 of the [CodeBit Specification](/CodeBit). The major version number is incremented because the new version is not backward compatible with version 1.0. This should cause much trouble because, to my knowledge, I'm the only one so far who has created any CodeBits.

The big change is that I switched from [YAML](https://yaml.org/) to MetaTag as the data format. I originally picked YAML because it's friendly for humans to read and I wanted people to be able to add metadata to their source files without having to study the details of a new language/syntax. At the time I thought of YAML as the data equivalent of Python. Both languages make indentation significant - especially for hierarchically organized structures.

When I began considering enhancements to CodeBit I decided first to enhance my [C# YAML parser](https://github.com/filemeta/yaml) to implement the entire YAML 1.1 specification. That's what got me into the intricacies of YAML. It's far more complicated than it appears on the surface and there are many ways to accomplish the same thing. Plus, it's vulnerable to weird issues such as the [Norway Problem](https://www.bram.us/2022/01/11/yaml-the-norway-problem/) which is tied to the having a strongly typed data format. My personal opinion is that the schema should dictate data types, not the data interchange format. Ultimately, the whole process turned me away from using YAML for CodeBits and I abandoned my parser project.

Since writing the original CodeBit specification I proposed the [MetaTag](/MetaTag) format for embedding metadata in any text. After some pondering I decided that MetaTags are ideal for CodeBit metadata as the are easily embedded in comments of any sort and can be extracted using any Regular Expression parser.

Posting the specification for CodeBit metadata is step 1. Coming soon will be an updated CodeBit console tool that will validate CodeBits and retrieve them for easy installation. This will be accompanied by a specification for self-branded CodeBit repositories so that you can publish your own CodeBit library.