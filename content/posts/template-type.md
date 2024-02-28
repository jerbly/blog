---
title: "Semantic Convention Template Types"
date: 2024-02-26
description: "Understand template types and visualize them in honey-explore"
tags: ["observability", "open-telemetry", "semantic-conventions", "honey-explore", "honeycomb.io"]
type: post
weight: 25
showTableOfContents: true
---

## OpenTelemetry Semantic Convention Template Type

[Template Types](https://github.com/open-telemetry/build-tools/blob/main/semantic-conventions/syntax.md#template-type) are described in the syntax document as:
> A template type attribute represents a dictionary of attributes with a common attribute name prefix.

The idea is that you may have a set of unknown key value pairs that belong to a common prefix. This is used for the **stable** semantic convention `http.response.header.<key>` for example.
In this case the `<key>` is the HTTP Header name and the value is the header values. This allows you to capture arbitrary headers with a well known prefix without them being defined beforehand.

The `http.response.header.<key>` type is defined as `template[string[]]` - this means: _a template type with an array of strings as the value_.

Template types are permissible for all the
primitive types, making the full set: `template[string]`, `template[int]`, `template[double]`, `template[boolean]`, `template[string[]]`, `template[int[]]`, `template[double[]]`, `template[boolean[]]`.

What this all means is you'll see attributes like `http.response.header.content_type` = `text/plain` or `http.response.header.access_control_allow_credentials` = `true` in Honeycomb.

## Tools

### Honey Explore

The [honey-explore](https://github.com/jerbly/honey-explore) tool has had support for template types since version 0.1.6:

![honey-explore screenshot of a template type](/images/template-type/template-type.png)

At the bottom of the screenshot you can see the keys that honey-explore has discovered from the honeycomb datasets. Just like normal attributes there are links taking you to a live query in honeycomb showing the last week of data.

### Honey Health

Since version 0.3.1 [honey-health](https://github.com/jerbly/honey-health) has had support for template types. There's not much to see here but previously it would incorrectly treat the prefix like `http.response.header` as a normal attribute and therefore would incorrectly highlight `http.response.header.content_type` as a missing attribute.

## Summary

You may be tempted to use template types to avoid explicitly defining a set of attributes. This is not the intention and if you do this you'll be missing out on defining rich semantic information for your attributes. Template types should **only** be used where you need to represent a dictionary and don't have prior knowledge of the complete set of keys.
