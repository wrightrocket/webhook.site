---
title: WebhookScript
nav_order: 500
has_children: true
---

# WebhookScript

WebhookScript is an easy to use scripting language designed for executing Web-related actions on incoming requests. 

While the other actions like Extract Regex and Send Email allows you to create flows in a visual editor, WebhookScript makes it quicker to create more advanced logic.

## Syntax

WebhookScript is based on [Primi](https://github.com/smuuf/primi) with a few extra additions and functions that are relevant to the flow of requests at Webhook.site. 

See also the [full language specification](https://github.com/smuuf/primi/blob/master/docs/language_reference.md).

The syntax is very similar to PHP and JavaScript.

## Types

Take a look at the language reference described above for more information about the properties of the different types.

* Bool
* Number
* String
* Regex
* Array
* Function


Returns the type name of value, e.g. `string`.

