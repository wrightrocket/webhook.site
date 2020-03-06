---
title: Custom Actions
nav_order: 400
---

# Custom Actions
{: .no_toc }

1. TOC
{:toc}

With Custom Actions, it is possible to string together a set of actions that are executed whenever a Webhook.site URL receives a request or email. The actions can extract data from the request and use it to send another request as well as modify the response. 

With this, you can connect APIs that aren't compatible, convert a request to an email, and much more.

![Custom actions screenshot](/custom-actions.png)

## About Variables

Variables are an important part of Custom Actions, and are characterized by a name surrounded by two dollar signs: `$example$`. Variables can be used in any field that has a ⓥ icon in the editor. They act as placeholders that are replaced by dynamic content as the request or email is received.

Each request or email has a set of base variables that contain information like the request IP, method, headers, query string values, form values and the request content. To see a list of variables, click the Variables button in the editor. Clicking on a variable copies it to the clipboard.

![Variables Menu](/variables.png)

Almost all of the available Custom Actions can register a variable during the runtime of the actions, so for example you can register the result of a JSONPath query and use it in a "Modify Response" action to make the response dynamic, or even use it to send a request to another HTTP address, and then use the response of that.

This works since Custom Actions are executed synchronously in a chain, sharing data as they're being executed.

The format of variables are dollar signs surrounded by a word, for example: `$example$`. 



## WebhookScript

!["WebhookScript" Custom Action screenshot](/webhookscript-action.png)

Executes custom scripts using a scripting language that's very similar to JavaScript and PHP. [More information here](/webhookscript.html)

## Extract JSONPath

This action runs a JSONPath query on the contents of a request. With it, you can extract any data from a JSON document and store it in a variable, which can then be used in a downstream action.

JSONPath is very similar to the `jq` commandline utility.

##### JSONPath Examples

JSONPath                  | Result
--------------------------|-------------------------------------
`$.store.books[*].author` | the authors of all books in the store
`$..author`                | all authors
`$.store..price`           | the price of everything in the store.
`$..books[2]`              | the third book
`$..books[(@.length-1)]`   | the last book in order.
`$..books[-1:]`            | the last book in order.
`$..books[0,1]`            | the first two books
`$..books[:2]`             | the first two books
`$..books[::2]`            | every second book starting from first one
`$..books[1:6:3]`          | every third book starting from 1 till 6
`$..books[?(@.isbn)]`      | filter all books with isbn number
`$..books[?(@.price<10)]`  | filter all books cheapier than 10
`$..*`                     | all elements in the data (recursively extracted)


##### JSONPath Syntax

Symbol                | Description
----------------------|-------------------------
`$`                   | The root object/element (not strictly necessary)
`@`                   | The current object/element
`.` or `[]`           | Child operator
`..`                  | Recursive descent
`*`                   | Wildcard. All child elements regardless their index.
`[,]`                 | Array indices as a set
`[start:end:step]`    | Array slice operator borrowed from ES4/Python.
`?()`                 | Filters a result set by a script expression
`()`                  | Uses the result of a script expression as the index


For more details on what's possible with JSONPath, [take a look at the docs](https://github.com/FlowCommunications/JSONPath#jsonpath-examples).

As you start entering a JSONPath, the results are validated and shown next to the input field.

## Extract Regex

This action runs a Regex (regular expression) query on the contents of a request. With it, you can extract any data from a text document and store it in a variable, which can then be used in a downstream action.

As you start entering a Regex, the results are validated and shown next to the input field.

## Extract XPath

Similar to the Extract JSONPath Custom Action, Extract XPath lets you extract values from an XML or HTML document and save the result as a variable.

##### XPath Examples

The following examples are based on this XML document:

```xml
<?xml version="1.0"?>
<organization name="ExampleCo">
  <employees>
    <employee id="1">Jack</employee>
    <employee id="2">Ann</employee>
  </employees>
</organization>
```

Example XPath                                     | Notes                                                       | Result
--------------------------------------------------|------------------------------------------------------------------------------
`/organization`                                   | Finds all content within the organization element           | Jack<br>Ann
`//employee[@id != 1]`                            | `//` finds all <employee> elements in document              | Jack
`/organization/@name`                             | `@name` to get the "name" property of the element           | ExampleCo
`/organization/employees/employee[2]`             | `[2]` specifies 2nd element                                 | Ann
`/organization/employees/employee[2]/@id`         | Get the "id" property of second employee element            | 2
`/organization/employees/employee[@id=1]`         | Employee element with id property equal to "1"              | Jack
`/organization/employees/employee[last()]`        | Last employee element                                       | Ann
`//employee[contains(@id, "2")]`                  | Employee within any parent element where id contains "2"    | Ann

For more examples, see [W3CSchools](https://www.w3schools.com/xml/xml_xpath.asp) or [XPath Cheatsheet](https://devhints.io/xpath)

## Send Request

This will send a request with variable contents from the Webhook.site cloud. Per default, the request contents will be identical to what was sent to the URL originally.

Variables extracted previously can be used.

The response of the request is stored in a series of variable names prefixed with a value of your choosing. The following variables are set after the request has been fired:

* `$your_prefix.content$` - response body content
* `$your_prefix.status$` - response status code
* `$your_prefix.headers$` - response headers
* `$your_prefix.url$` - the URL the request was sent to

## Send Email

This will send a email with variable contents from the Webhook.site cloud. Variables extracted previously can be used.

## Modify Response

This action can be used to modify the response of the Webhook.site URL based on the input.

## Condition

!["Condition" Custom Action screenshot](/condition-action.png)

Useful if you need to validate that the request does or does not conform to certain criteria, the Condition action will either stop or continue based on a condition.

In both the *input* and the *value* fields, variables will be replaced (including Global Variables from the Control Panel), so you can compare e.g. JSONPath or Regex values - or even values from a previous HTTP request that was sent. 

Currently, two *actions* are provided: stop and continue. *Stop* will stop further action execution of the condition is a match. *Continue* will *only* continue further execution if the condition is a match, and otherwise stop.

The following "operators" are available:

* is equal to
* is not equal to
* starts with
* ends with
* contains
* does not contain
* is greater than
* is greater than or equal to
* is less than
* is less than or equal to

The "result" of the condition will be logged below the request details, so you can see what happened.