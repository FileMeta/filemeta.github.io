---
markdown: gfm
title: MetaTags
layout: default
date: 2019-05-29
---
# MetaTags

There are times when it would be nice to add arbitrary metadata to text when the medium doesn't make a specific provision for it. Perhaps the needed metadata field hasn't been defined on that medium or perhaps the medium doesn't support metadata at all.

The [hashtag](https://en.wikipedia.org/wiki/Hashtag) already fills this need for keywords, but that's only one of many valuable metadata tags. For example, wouldn't it be nice to specify the source of some content, or the relevant date, or the copyright license?

A metatag is like a hashtag in that it can be embedded wherever text is stored. However, a metatag has a name and a value. Thus, metatags allow custom metadata to be stored in existing text or textual fields such as comments.

## Some Examples

* &author=Brandt
* &subject="MetaTag Format"
* &date=2018-12-17T21:22:05-06:00
* &license=CC-BY
* &references=https://en.wikipedia.org/wiki/Metadata

## Format Definition

A metatag starts with an ampersand - just as a hashtag starts with the hash symbol.

Next comes the name which follows the same standard as a hashtag - it must be composed of letters, numbers, and the underscore character. Rigorous implementations should use the unicode character sets. Specifically Unicode categories: Ll, Lu, Lt, Lo, Lm, Mn, Nd, Pc. For regular expressions this matches the \w chacter class.

Next is an equals sign.

Next is the value which may be in plain or quoted form. In plain form, the value is a series of one or more non-whitespace and non-quote characters. The value is terminated by whitespace or the end of the document.

Quoted form is a quotation mark followed by zero or more non-quote characters and terminated with another quotation mark. Newlines and other whitespace are permitted within the quoted text. A pair of quotation marks in the text is interpreted as a singe quotation mark in the value and does not terminate the value.
