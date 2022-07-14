---
title: Enhanced MetaTag Specification
date: 2022-07-07
author: Brandt Redd
---

Today I have updated the [MetaTag Specification](/MetaTag). The new spec, version 1.1, is backward compatible with version 1.0. It has the following enhancements:

* MetaTags (without prefixes) are explicitly from the [Schema.org](https://schema.org) schema.
* A regular expression is given that can be used to find metatags embedded in text.
* A namespace prefix may be used to specify another schema.
* It explicitly states that repeated MetaTags should be interpreted as a list of values for the same property - but only when the property definition accepts a list.
* Design goals are given along with discussion about why I made certain design decisions.
* An experimental design for hierarchical data models is included.
