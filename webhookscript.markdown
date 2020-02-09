---
title: WebhookScript
nav_order: 500
has_children: true
---

# WebhookScript

WebhookScript is an easy to use scripting language designed for executing Web-related actions on incoming requests. 

While the other actions like Extract Regex and Send Email allows you to create flows in a visual editor, WebhookScript makes it quicker to create more advanced logic.

## Syntax

The syntax is very similar to PHP and JavaScript. See also the [full language specification](/webhookscript/reference.html).

## Types

Take a look at the language reference described above for more information about the properties of the different types.

* Bool
* Number
* String
* Regex
* Array
* Function


Returns the type name of value, e.g. `string`.

## Editor

!["WebhookScript" Custom Action screenshot](/webhookscript-action.png)

The editor has syntax highlighting, and below it is data relating to the current and previous actions:

* **Debug outputs** shows the outputs of all the actions, with the current action being edited or created marked in blue.

* **Response** shows details of the response of the URL formatted in JSON.

* **Variables** is a table of all current available variables for use in the script with the `var()` function or `variables` array.