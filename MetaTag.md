---
markdown: gfm
title: MetaTag
layout: default
date: 2019-05-29
---
&name="MetaTag Specification" <br/>
&version=1.2-beta.2 <br/>
&author="Brandt Redd" <br/>
&datePublished=2022-10-17 <br/>
&license=https://creativecommons.org/licenses/by/4.0/ <br/>
&description="A specification for embedding metadata in any text stream much as hashtags are used in social media." <br/>
&url=http://www.filemeta.org/MetaTag

# MetaTags

There are times when it is helpful to add arbitrary metadata to text when the medium doesn't make a specific provision for it. Perhaps the needed metadata field hasn't been defined on that medium or perhaps the medium doesn't support metadata at all.

The [hashtag](https://en.wikipedia.org/wiki/Hashtag) already fills this need for keywords, but that's only one of many valuable metadata tags. For example, it may be appropriate to specify the source of some content, or the relevant date, or the copyright license in a form that can be interpreted and indexed by computer.

A metatag is like a hashtag in that it can be embedded wherever text is stored. However, a metatag has both a name and a value. Thus, metatags allow custom metadata properties to be stored in existing text or textual fields such as comments.

An important use case is embedding metadata in source code. MetaTags can be placed in a comment at the beginning of a source code file. Some programming languages have a metadata syntax. For example, C# has attributes that can be applied to classes, methods, properties, etc. However, metatags represent metadata about the source code *file* rather than the code structure. Thus, they are the right way to represent authorship, copyright info, licensing, and related information. (See the [CodeBit Specification](/CodeBit).)

## Example Metatags

```
&author=Brandt
&subject="MetaTag Specification"
&comment="The term ""MetaTag"" is defined in this document."
&datePublished=2018-12-17T21:22:05-06:00
&license=CC-BY
&references=https://en.wikipedia.org/wiki/Metadata
```

## Basic Format Definition

MetaTags may be embedded in any text string or stream with any character encoding. This specification treats the text as a series of Unicode characters without regard to the underlying encoding (UTF-8, UTF-16, US-ASCII, etc.).

A metatag starts with an ampersand (&) - just as a hashtag starts with the hash (#) symbol.

An **optional** namespace prefix immediately follows the amperstand. See the [Namespaces](#namespaces) section for details on the use of namepace references.

Next comes the name which follows the same standard as a hashtag - it must be composed of letters, numbers, and the underscore character. Specifically Unicode categories: Ll, Lu, Lt, Lo, Lm, Mn, Nd, Pc. For regular expressions this matches the \w character class.

Next is an equals sign.

Next is the value which may be in plain or quoted form.

In **plain form**, the value is a series of one or more non-whitespace and non-reserved characters. The value is terminated by whitespace, a reserved character, or by the end of the document. **Reserved characters** are the following: " (quote) ; (semicolon) [] (brackets) {} (braces) & (ampersand) , (comma)

Example: &name=Abundance

In **quoted form**, the value is given by a full quotation mark (") followed by zero or more non-quote characters and terminated with another quotation mark. Newlines and other whitespace are permitted within the quoted text. A pair of quotation marks in the text, with nothing between, is interpreted as a single quotation mark in the value and does not terminate the value.

Example: &name="Doug Jones"

All text that doesn't match the metatag pattern is the content of the text stream and is *not* metadata. Therefore, it should be ignored by a metatag extractor.

## Embedded in Text
All text other than metatags is the *content* of the subject. As with hashtags, metatags may appear anywhere and in any order.

Here's an example of mixed text and metatags.

```
&headline="Water Discovered on mars"
by &author="Doug Jones"
&datePublished="2022-07-15"

One of the goals of the Mars rovers has been
to find evidence of current and past water
on the Mars surface. &keywords=Mars
```

## Multivalued Properties

Some properties, by their definition, may have multiple values. A [Schema.org](https://schema.org) example is [accessibilityFeature](https://schema.org/accessibilityFeature). In metatag format you create a list by using multiple tags with the same name but different values.

In this example, two values are specified for `accessibilityFeature`.
```
&accessibilityFeature=largePrint/CSSEnabled
&accessibilityFeature=displayTransformability
```

For certain properties, multiple values may be separated by a delimiter. For example, the [keywords](https://schema.org/keywords), may have multiple values separated by commas. 

Depending on the property, the order of the values may or may not be relevant. When order matters, it is the order in which the properties are listed.

If multiple values are specified for a single-valued property then the *first* value should be used. The others may be reported as errors.

Note that it is the definition of a metadata property that indicates whether it is single-valued or multi-valued. Thus, a general purpose metatag extractor does not have information about whether a property is single or multi-valued. The API for the extractor should be designed accordingly.

## Extracting MetaTags using Regular Expressions

The following regular expression will match any metatag in a text stream separating out the prefix, name, and value.

```
&((\w+:)?\w+)=([^\s";\[\]\{\}&,]+|(?:"[^"]*")+)
```

If the value is quoted, the quotes are included in the regular expression match. Therefore, the calling code must remove the beginning and ending quotes and convert double embedded quotes to single quotes.

## Programmatically Setting MetaTags

Humans can embed metatags wherever they like. When metatags are added programmatically, the best practice is to first search for an existing tag with the matching name. If found, update the tag in-place. If not found, place the new tag at the end of the text string or stream. For multi-valued tags keep all entries together.

## Schema

When a namespace prefix is not present (which is the majority of the time) metadata elements belong to the [Schema.org](https://schema.org) namespace and should use the property definitions in that standard. **Schema.org** has very broad coverage so it should support a majority of needed properties.

## Data Types
The data type of a value is interpreted by the application, guided by the schema, not by the source. For example:

```
&commentCount=4
&name=4
```

In [Schema.org](https://schema.org), [commentCount](https://schema.org/commentCount) is defined as an integer so the value is interpreted as the integer 4. However, [name](https://schema.org/name) is defined as text so the value is interpreted as the string "4". When the type is not known or doesn't matter to the application then values should be handled as strings.

## Namespaces

Namespaces are used to reference schemas other than [Schema.org](https://schema.org). You may need properties from another schema or you may need to define a custom schema for specialized properties.

The namespace syntax is inspired by XML. A namespace prefix consists of one or more "word characters" followed by a colon. Word characters are the Unicode categories: Ll, Lu, Lt, Lo, Lm, Mn, Nd, Pc. For regular expressions this matches the \w character class.

The "ns:" prefix is reserved for associating a namespace prefix with the corresponding URL. This is called a *namespace declaration*. For Example:

```
&ns:dc=http://purl.org/dc/terms/
```

This declaration is not part of the metadata. Rather, it indicates that the `dc:` prefix will reference the [Dublin Core Metadata Initiative](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/) Terms schema.

Elsewhere in the text stream the prefix may be used. For example:

```
&dc:description="A specification for embedding metadata in raw textual streams."
```

The scope of a namespace declaration is the entire text string or stream where the metatags appear. Order does not matter. A namespace declaration may come before or after the prefix is used.

## RDF Type and Subject

[Resource Description Framework (RDF)](https://en.wikipedia.org/wiki/Resource_Description_Framework) is W3C standard data model for metadata. There are multiple bindings for RDF. That is, there are multiple ways to represent RDF metadata including [RDFa](https://en.wikipedia.org/wiki/RDFa), [Microdata](https://en.wikipedia.org/wiki/Microdata_(HTML)), and [JSON-LD](https://en.wikipedia.org/wiki/JSON-LD).

Despite referencing [Schema.org](https://schema.org), which is an RDF schema, metatags do not qualify as an RDF binding. That's because the metatag data model is flat and RDF schemas require hierarchical structure to the metadata. (See [Experimental Features](#experimental-features) for the prospect of adding hierarchy.)

Other requirements of RDF are that the subject of the metadata be indicated and the object type of the subject be indicated.

For metatags, the **subject** is the document or text stream in which the metatags are embedded. The **type** is optional and may be specified using the "_type" property. For example:

```
&_type=Book
&name="The Fellowship of the Ring"
```

A `_type` value without a namespace prefix references types defined by the Schema.org context. A `_type` value with a namespace prefix references types defined by the corresponding namespace.

## Design Goals and Discussion

In creating the MetaTag Specification I, ([Brandt Redd](https://brandtredd.org)), started with the following goals:

* The syntax should be sufficiently simple and intuitive that someone can compose valid metadata simply by viewing examples.
* The metadata should be both human-readable and machine-readable.
* For applications that need the rigor of RDF it should be achievable, but that should not detract from the simple and intuitive goal.
* There should be one obvious way to do each thing.

I believe that this specification mostly achieves these goals but I had to make a few compromises.

I debated whether to require quotes on all values. Allowing the unquoted format violates the "one obvious way" rule. Quoted format is required since some values will have embedded spaces. Unquoted format is not a requirement. But it is more succinct for simple values and is more reminiscent of the hashtags that inspired metatags. Ultimately, it seemed reasonable to follow the precedent of command-line syntax in supporting both formats.

I considered whether to use doubled-quotes or a backslash escape for embedding quotation marks in a quoted value. Backslash escapes would also enable other escaped values such as \n for linefeed and even [Unicode](https://unicode.org) characters like \u1F643. But the goal of human-readability guided me toward keeping the quote escape simple and requiring all special characters to be literally embedded and not encoded.

The specification would be complicated by a data type syntax. And default interpretation risks problems like [YAML has with Norway](https://www.bram.us/2022/01/11/yaml-the-norway-problem/). Therefore, string is the default and type interpretation is made by the application.

Multivalued properties, or those with a list type, are handled by repeating the property once for each value. This is a simple solution that doesn't require any additional syntax.

This specification has not yet achieved the goal of full compatibility with RDF. But I am experimenting with that. (See [Experimental Features](#experimental-features).)

## Usage of the Term, "MetaTag"

When capitalized, such as at the beginning of a sentence or in a title, "MetaTag" should use [upper camel case](https://en.wikipedia.org/wiki/Camel_case).

When not-capitalized, such as in a sentence, "metatag" should be all lower case.

## Experimental Features

*The following is an experimental concept and is not guaranteed to remain in the specification. Use at your own risk.*

To fully represent the RDF data model, certain properties must be "objects" (borrowing a term from [JSON](https://www.json.org/json-en.html)). That is, the value of a property may be a sub-collection of names and values. This results in a hierarchical structure to the data.

Adding an "object" capacity or hierarchical data structure would make metatags more compatible with RDF and would make the data model equivalent to JSON. That is, data could be converted between JSON and metatag format with no data loss.

Here is a proposed syntax:

A property with an object value is introduced with a left brace ({) as the value (with no quotes). The property is closed with a standalone right brace (}). Between the braces, only metatags and whitespace may appear.

In this example, indentation is not meaningful to the metatag parser, it's for convenience to the human reader:

```
&_type=Movie
&name="The Fellowship of the Ring"
&isBasedOn={
    &_type=Book
    &name="The Fellowship of the Ring"
    &author="J.R.R. Tolkien"
}
```
