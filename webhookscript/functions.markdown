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

## Chaining

Functions can be chained directly to a primitive (strings, numbers, arrays).

These two statements are equivalent:

```javascript
'Hello World'.echo()
```

```javascript
echo('Hello World')
```

They can even be chained, for example:

```javascript
'Hello World'.hash('md5').echo() 
```

Read more about functions in the [reference](/webhookscript/reference.html#functions).

## Custom functions

Define your own functions like this:

```javascript
function sub(a, b) {
    return a - b;
}
```

Read more about functions in the [reference](/webhookscript/reference.html#functions).

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

### store(***string*** global_variable_name, ***any*** value): ***any***

Permanently creates or updates a Global Variable named `global_variable_name` with value. The value can be retrieved with the `var()` function.

### variables : array

Global variable (not a function) containing an associative array with all available Webhook.site variables.

## HTTP

### query(***array*** form_values) : string

Converts an associative array into a form-style string, like would be used for `application/x-www-form-urlencoded` requests or HTTP query strings.

Example: `query(['country': 'Curaçao', 'population': 158665])` returns `country=Cura%C3%A7ao&population=158665`

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

### url_decode(***string*** value) : string

Returns an URL-decoded version of *value*.

### url_encode(***string*** value) : string

Returns an URL-encoded version of *value*.

Example: `url_encode('here\'s a value')` returns `here%27s+a+value`.

## Flow Control and Responses

### delay(***int*** seconds, ***string*** code)

Executes code in the future. Any output will be stored on the request and will show with a "Was delayed" label. 

The code will not inherit the execution scope, as you can see in this example where the `format` function is used to prepare the code string with a URL, causing `{}` to be replaced with with `https://example.com`.

```javascript
url = 'https://example.com'

code = '
  request(
      "{}",
      \'{"message": "Hello World!"}\',
      "POST"
  )
'.format(url)

delay(5, code);
```

The maximum amount of seconds allowed is 604800.

### exec(***string*** code) : ***any***

Executes code in `code` and returns the result. The code will inherit the execution scope.

### import(***string*** url) : ***any***

Downloads code located at `url` and returns the result. The code will inherit the execution scope. As an example, this can be used if you want to re-use code. Just upload it to your server or e.g. Github and use it in different WebhookScript actions.

```javascript
result = import('https://raw.githubusercontent.com/fredsted/webhookscripts/ec22946a83ea85f607fcc6bff83f9d81ed2fe4ed/hello_world.ws')
echo(result) // -> "value"
```

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

### base64_decode(***string*** string) : string

Returns a base64-encoded string.

### base64_encode(***string*** string) : string

Returns decoded base64 string.

### hash(***string/number*** value, ***string*** algo) : string

Returns a hashed version of `value` using the `algo` algorithm.

`hash('md5', 'hello world')` returns `5eb63bbbe01eeed093cb22bb8f5acdc3`.

The following algorithms are available: `md2`, `md4`, `md5`, `sha1`, `sha224`, `sha256`, `sha384`, `sha512/224`, `sha512/256`, `sha512`, `sha3-224`, `sha3-256`, `sha3-384`, `sha3-512`, `ripemd128`, `ripemd160`, `ripemd256`, `ripemd320`, `whirlpool`, `tiger128,3`, `tiger160,3`, `tiger192,3`, `tiger128,4`, `tiger160,4`, `tiger192,4`, `snefru`, `snefru256`, `gost`, `gost-crypto`, `adler32`, `crc32`, `crc32b`, `fnv132`, `fnv1a32`, `fnv164`, `fnv1a64`, `joaat`, `haval128,3`, `haval160,3`, `haval192,3`, `haval224,3`, `haval256,3`, `haval128,4`, `haval160,4`, `haval192,4`, `haval224,4`, `haval256,4`, `haval128,5`, `haval160,5`, `haval192,5`, `haval224,5`, `haval256,5`.

### json_decode(***string*** json) : array

Decodes `json` and returns an array

### json_encode(***array*** array) : string

Takes an array and encodes it as a JSON string

### json_path(***string*** json, ***string*** jsonpath, ***bool*** return_first = true) : string

Returns the result of a `json` string parsed using the [JSONPath](/custom-actions.html#extract-jsonpath) functionality.

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

### regex_extract(***regex*** regex, ***string*** subject) : array/false

Returns the matching string and all match groups as an array, and `false` on failure.

```javascript
input = "You're a good bot"

output = regex_extract(r"You're (\w) (.*)", input)

dump(output) // [0: "You're a good bot", 1: "a", 2: "good bot"]
```

### regex_extract_first(***regex*** regex, ***string*** subject) : string/false

Returns the first match group of a regex, and `false` on failure.

```javascript
input = "You're a good bot"

output = regex_extract(r"You're (.*)", input)

dump(output) // "a good bot"
```

### regex_match(***regex*** regex, ***string*** subject) : string/false

Returns the first matching string, otherwise false.

```javascript
input = "You're a good bot"

output = regex_match(r"You're .*", input)

dump(output) // "You're a good bot"
```

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

### xpath(***string*** xpath, ***string*** input): string/null

Returns the first result of an XPath query on XML document `input`.

Given a request with the following content:

```
<?xml version="1.0"?>
<organization name="ExampleCo">
  <employees>
    <employee id="1">Jack</employee>
    <employee id="2">Ann</employee>
  </employees>
</organization>
```

* `xpath(var('$request.content$'), '//employee[1]') // returns "Jack"`
* `var('$request.content$').xpath('//employee[1]') // returns "Jack"`

[More information and examples regarding XPath](/custom-actions.html#extract-xpath).

### xpath_all(***string*** xpath, ***string*** input): string/null

Returns the results of an XPath query on XML document `input` as an array.

Given a request with the following content:

```
<?xml version="1.0"?>
<organization name="ExampleCo">
  <employees>
    <employee id="1">Jack</employee>
    <employee id="2">Ann</employee>
  </employees>
</organization>
```

* `xpath_all(var('$request.content$'), '//employee]') // returns [0: "Jack", 1: "Ann"]`
* `var('$request.content$').xpath('//employee') // returns [0: "Jack", 1: "Ann"]`

[More information and examples regarding XPath](/custom-actions.html#extract-xpath).

## Math and Numbers

### abs(***number*** number) : number

### atan(***number*** number) : number

### ceil(***number*** number) : number

### cos(***number*** number) : number

### floor(***number*** number) : number

### max(***array*** numbers) : number

### min(***array*** numbers) : number

### mod(***number*** number, ***number*** divisor) : number

Returns the remainder after number is divided by `divisor`.

### number_length(***number*** number) : number

### pi() : number

Returns the value of Pi.

### pow(***number*** number, ***number*** power) : number

### rand(***number*** min, ***number*** max) : number

Returns a random number between `min` and `max`.

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
