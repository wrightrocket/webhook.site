---
title: WebhookScript
navigation_weight: 400
---

WebhookScript is an extremely easy to use scripting language for executing actions on incoming requests. While the other actions like Extract Regex and Send Email allows you to create flows in a WYSIWYG (what you see is what you get) manner, WebhookScript makes it quicker to create more advanced logic in less time.

## Syntax

WebhookScript is based on [Primi](https://github.com/smuuf/primi) with a few extra additions and functions that are relevant to the flow of requests at Webhook.site. 

See also the [full language specification](https://github.com/smuuf/primi/blob/master/docs/language_reference.md).

The syntax is very similar to PHP and JavaScript.

## Examples

## Functions

### General

`debug(*any* value)` - sets a value to be returned in the debug console

`get_variable(*string* variable_name)` - gets a specific Webhook.site variable

`set_content(*string* content)` - sets the response content of the URL

`set_header(*string* header_name, *string* header_value)` - sets or overwrites a response header of the URL

`set_status(*number* status)` - sets the http status of the URL

`set_variable(*string* variable_name, *string* variable_value)` - sets a variable for usage in a later (Script) action

`variables()` - returns an array with all available Webhook.site variables

### String

`hash_md5(*string/number* value)` - returns md5 hash of value

`hash_sha256(*string/number* value)` - returns sha256 hash of value

`jsonpath(*string* json, *string* jsonpath)` - returns parses the *json* string using the JSONPath; multiple values are comma-separated

`regex_match(*regex* regex, *string* subject)` - returns the first matching string, otherwise false

`regex_to_string(*regex* regex)` - returns the regex converted to a string

`string_contains(*string* subject, *number/string/regex* value)` - returns boolean if *subject* contains *value*

`string_find_first(*string* subject, *number/string* value)` - returns position of *value* in *subject*, or false if not found

`string_find_last(*string* subject, *number/string* value)` - returns position of *value* in *subject*, or false if not found

`string_format(*string* formatString, ...*any* items)` - sprintf-like formatting of formatString with *items*, see PHP sprintf docs.

`string_join(*string* subject, *array* items (number/string/bool/array))` - joins items with string *subject*

`string_length(*string* string)` - returns length of string (multibyte-aware)

`string_number_of(*string* string)` - returns number value of *string*

`string_replace(*string* subject, *string* search, *string* replace)` - replaces string *search* with *replace* found in *subject*.

`string_reverse(*string* subject)` - reverses string *subject* 

`string_shuffle(*string* string)` - returns shuffled string

`string_split(*string* subject, *string/regex* delimiter)` - returns array of split string *subject* with *delimiter* 

`to_regex(*string* regex)` - converts a regex string to a regex type

`to_string(*string/number/bool* value)` - returns *value* as string 

### Math and Numbers

`abs(*number* number)`

`atan(*number* number)`

`ceil(*number* number)`

`cos(*number* number)`

`floor(*number* number)`

`number_length(*number* number)`

`pow(*number* number, *number* power)`

`round(*number* number, *number* precision)`

`sin(*number* number)`

`sqrt(*number* number)`

`tan(*number* number)`

`to_number(*any* value)`


### Arrays

`array_contains(*array* array, *string/number* needle) : bool` - returns true or false depending on whether *array* contains *needle* 

`array_copy(*array* array) : array` - returns a copy of *array*

`array_get(*array* array, *string/number* index, *any* default) : any` 

`array_has(*array* array, *string/number* key) : bool`

`array_length(*array* array) : number` 

`array_map(*array* array, *func* function)` - runs function with each array value, and returns array with key as result

`array_number_of(*array*, *string/number* value) : number` - returns amount of *value*

`array_pop(*array* array) : any` - pop element off end of array

`array_push(*array* array, *any* value) : any` - adds *value* to end of *array* and returns *value*

`array_random(*array* array) : any` - returns random value of *array* 

`array_reverse(*array* array) : array` 

`array_shuffle(*array* array) : array` - returns shuffled version of *array*

`to_array(*array* array) : array` - 

### Booleans

`bool_and()`

`bool_not()`

`to_bool()`

### Types

`type()`

