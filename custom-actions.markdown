---
title: Custom Actions
order: 10
---

With Custom Actions, it is possible to string together a set of actions that are executed whenever a Webhook.site URL receives a request. The actions can extract data from the request and use it to send another request as well as modify the response. With this, you can connect different APIs together that aren't compatible.

![Custom actions screenshot](/custom-actions.png)

## Available actions

### Extract JSONPath

This action runs a JSONPath query on the contents of a request. With it, you can extract any data from a JSON document and store it in a variable, which can then be used in a downstream action.

JSONPath is very similar to the `jq` commandline utility. 

For more details on what's possible with JSONPath, [take a look at the docs](https://github.com/FlowCommunications/JSONPath#jsonpath-examples).

As you start entering a JSONPath, the results are validated and shown next to the input field.

### Extract Regex

This action runs a Regex (regular expression) query on the contents of a request. With it, you can extract any data from a text document and store it in a variable, which can then be used in a downstream action.

As you start entering a Regex, the results are validated and shown next to the input field.

### Send Request

This will send a request with variable contents from the Webhook.site cloud. Per default, the request contents will be identical to what was sent to the URL originally.

Variables extracted previously can be used.

### Send Email

This will send a email with variable contents from the Webhook.site cloud. Variables extracted previously can be used.

### Modify Response

This action can be used to modify the response of the Webhook.site URL based on the input.