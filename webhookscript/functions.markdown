---
title: Functions
nav_order: 500
parent: WebhookScript
---

# Global Functions
{: .no_toc }

These are the functions that can be used in your script, and includes various utility functions and functions to interact with your Webhook.site URL.

1. TOC
{:toc}

## Debugging and output

### echo(***string*** string)

Adds `string` to script debug output.

### dd(***any*** value)

Halts Custom Action execution and adds `value` to debug output.

### dump(***any*** value)

Adds `value` as a decoded string to script debug output.

## Custom Action Variables

These functions lets you interface with other Custom Actions by getting and setting variables from them. These functions are also how you retrieve Global Variables defined in the Control Panel.

### var(***string*** variable_name) : mixed

Gets a specific Webhook.site Variable, including Global Variables (defined in the Control Panel). Variables should be without the `$` format used in other actions. 

Example: `var('request.header.x-request-verification')`. 

`get_variable()` is an alias to this function.

### set(***string*** variable_name, ***string*** variable_value)

Sets a variable for usage in a later WebhookScript Action or other Custom Action. 

`set_variable()` is an alias to this function.

### variables : array

Global variable (not a function) containing an associative array with all available Webhook.site variables.

## HTTP

### request(***string*** url, ***string*** content = '', ***string*** method = 'GET', ***array*** headers = []) : array

Sends a HTTP request and returns an array with the following keys: `content`, `status`, `headers`, `url`.

The headers should be an array of strings in the form of `Header-Key: Value`.

To get a JSON document, validate if valid JSON, and get a property:

```javascript
response = request('https://example.com')

decoded = json_decode(response['content'])
if (decoded) {
  value = decoded['value']
}
```

## Flow Control and Responses

### respond(***string*** content, ***int*** status, ***array*** headers)

Halts Custom Action execution and return a response.

### stop()

Stop executing current script.

### set_content(***string*** content)

Sets the response content of the URL

### set_header(***string*** header_name, ***string*** header_value)

Sets or overwrites a response header of the URL. Headers should be an array of strings e.g. `["X-Example: Value"]

### set_response(***string*** content, ***int*** status, ***array*** headers)

Set response content, status and headers in single function.

### set_status(***number*** status)

Sets the HTTP response status of the current URL.

## String

### hash_md5(***string/number*** value) : string

Returns md5 hash of value

### hash_sha256(***string/number*** value) : string

Returns sha256 hash of value

### json_decode(***string*** json) : array

Decodes `json` and returns an array

### json_encode(***array*** array) : string

Takes an array and encodes it as a JSON string

### json_path(***string*** json, ***string*** jsonpath, ***bool*** return_first = true) : string

Returns parses the `json` string using the JSONPath functionality.

Per default, if there's just one match (e.g. if matching on a property value that's a string), this value is returned. To always return an array, set `return_first` to false. 

```javascript
dump(json_path('{"v": []}', 'v[*]', false))
dump(json_path('{"v": []}', 'v[*]'))
// []
// ""
 
dump(json_path('{"v": ["item1"]}', 'v[*]', false))
dump(json_path('{"v": ["item1"]}', 'v[*]'))
// [0: "item1"]
// "item1"
 
dump(json_path('{"v": ["item1", "item2"]}', 'v[*]', false))
dump(json_path('{"v": ["item1", "item2"]}', 'v[*]'))
// [0: "item1", 1: "item2"]
// [0: "item1", 1: "item2"]
```

### regex_match(***regex*** regex, ***string*** subject) : string

Returns the first matching string, otherwise false.

### regex_to_string(***regex*** regex) : string

Returns the regex converted to a string

### string_contains(***string*** subject, ***number/string/regex*** value) : bool

Returns boolean if ***subject*** contains ***value***

### string_find_first(***string*** subject, ***number/string*** value) : number

Returns position of ***value*** in ***subject***, or false if not found

### string_find_last(***string*** subject, ***number/string*** value) : number

Returns position of ***value*** in ***subject***, or false if not found

### string_format(***string*** formatString, ...***any*** items) : string

Sprintf-like formatting of formatString with ***items***, see PHP sprintf docs.

### string_join(***string*** subject, ***array*** items (number/string/bool/array)) : string

Joins items with string ***subject***

### string_length(***string*** string) : number

Returns length of string (multibyte-aware)

### string_number_of(***string*** string) : number

Returns number value of ***string***

### string_replace(***string*** subject, ***string*** search, ***string*** replace) : string

Replaces string ***search*** with ***replace*** found in ***subject***.

### string_reverse(***string*** subject) : string

Reverses string ***subject*** 

### string_shuffle(***string*** string) : string

Returns shuffled string

### string_split(***string*** subject, ***string/regex*** delimiter) : string

Returns array of split string ***subject*** with ***delimiter*** 

### to_regex(***string*** regex) : regex

Converts a regex string to a regex type

### to_string(***string/number/bool*** value) : string

Returns ***value*** as string 

## Math and Numbers

### abs(***number*** number) : number

### atan(***number*** number) : number

### ceil(***number*** number) : number

### cos(***number*** number) : number

### floor(***number*** number) : number

### number_length(***number*** number) : number

### pow(***number*** number, ***number*** power) : number

### round(***number*** number, ***number*** precision) : number

### sin(***number*** number) : number

### sqrt(***number*** number) : number

### tan(***number*** number) : number

### to_number(***any*** value) : number

## Arrays

### array_contains(***array*** array, ***string/number*** needle) : bool

Returns true or false depending on whether ***array*** contains ***needle*** 

### array_copy(***array*** array) : array

Returns a copy of ***array***

### array_diff(***array*** array1, ***array*** array2) : array

Returns the items of array1 that are not present in array2 while keeping the array indices.

### array_get(***array*** array, ***string/number*** index, ***any*** default) : any` 

### array_has(***array*** array, ***string/number*** key) : bool

### array_length(***array*** array) : number` 

### array_map(***array*** array, ***func*** function)

Runs function with each array value, and returns array with key as result

### array_number_of(***array***, ***string/number*** value) : number

Returns amount of ***value***

### array_pop(***array*** array) : any

Pop element off end of array

### array_push(***array*** array, ***any*** value) : any

Adds ***value*** to end of ***array*** and returns ***value***

### array_random(***array*** array) : any

Returns random value of ***array*** 

### array_reverse(***array*** array) : array` 

Returns ***array*** in reverse order

### array_shuffle(***array*** array) : array

Returns shuffled version of ***array***

### to_array(***array*** array) : array

Returns ***array***.

## Booleans

### bool_and(***bool*** value1, ***bool*** value2): ***bool***

Returns `value1 && value2

### bool_not(***bool*** value) : ***bool***

Returns `!value

### to_bool(***string/number/array/bool*** value) : ***bool***

Casts `value` to a bool.

## Types

### type(***any*** value) : ***string***

Returns the type name of a value, e.g. `"string"`.
