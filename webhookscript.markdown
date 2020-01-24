---
title: WebhookScript
navigation_weight: 400
---

WebhookScript is an extremely easy to use scripting language for executing actions on incoming requests. While the other actions like Extract Regex and Send Email allows you to create flows in a WYSIWYG (what you see is what you get) manner, WebhookScript makes it quicker to create more advanced logic in less time.

## Examples

### Validate request

In this example, we use a common method of verifying webhooks by taking a hash of its contents concatenated to a secret. It demonstrates the way WebhookScript can get various information about the request by using the `get_variable()` function, as well as string concatenation, hashing, if statements and returning responses with content, status codes and headers using `respond()`.

```javascript
verification_secret = "JHRlc3RTY3JpcHRTZWNyZXQ";
verification_challenge = get_variable("request.header.x-request-verification");
verification_result = hash_sha256(get_variable('request.content') + verification_secret);

if (!verification_challenge or verification_challenge != verification_result) {
    respond("Invalid request", 500);
}

headers = ["X-Success: Yes", "X-Verification: "+verification_challenge];
respond("Successful request", 200, headers);
```

### Transform and resend

In the following, an incoming request is JSON decoded to an array, transformed and sent to "Web Service 1". Then the output is saved and passed on to "Web Service 2" in XML format. Basic error handling and validation is demonstrated.

```javascript
// Configuration
ws1_api_key = 'xxxxx';
ws2_user_token = 'yyyyy';

// Parse original request
orig_req = json_decode(get_variable('request.content'));
first_name = orig_req['firstName'];
last_name = orig_req['lastName'];
list_id = orig_req['listId'];
group_id = orig_req['groupingId'];


// Send request to Web Service 1
ws1_url = 'https://ws1.example.com/3.0/lists/'+ list_id +'/interest-categories/'+ group_id +'/interests';
ws1_content = '{"first_name": "'+ first_name +'", "last_name": "'+ last_name +'"}';
ws1_response = request(
  ws1_url,
  ws1_content,
  'POST',
  ['Authorization: Basic '+ws1_api_key]
)

// Don't go further if the Web Service 1 step didn't succeed
if (ws1_response['status'] != 200) {
  echo(ws1_response['content']); // Log content to debug log
  respond('Error', 500);
}

// Get a value from the Web Service 1 request
ws1_group_id = ws1_content['id'];

// Pass response on to Web Service 2 in XML format
ws2_content = '<qdbapi>'+
    '<usertoken>'+ ws2_user_token +'</usertoken>'+
    '<listid>'+to_string(list_id)+'</listid>'+
    '<field fid="7">'+ws1_group_id+'</field>'+
  '</qdbapi>';
ws2_response = request(
  'https://ws2.example.com/db/zzzzzz',
  ws2_content,
  'POST', 
  [
    "Action: API_EditRecord", 
    "Content-Type: application/xml"
  ]
);

if (ws2_response['status'] != 200) {
  echo(ws2_response['content']); // Log content to debug log
  respond('Error', 500);
}

// Output the WS2 response content to debug output
echo(ws2_response['content']);
respond('OK', 200);
```

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

## Global Functions

These are the functions that can be used in your script, and includes various utility functions and functions to interact with your Webhook.site URL.

### General

#### `echo(*string* string)`

Adds `string` to script debug output

#### `dd(*any* value)`

Halts Custom Action execution and adds `value` to debug output

#### `dump(*any* value)`

Adds `value` to script debug output

#### `get_variable(*string* variable_name) : mixed`

gets a specific Webhook.site Variable. Variables should be without the `$` format used in other actions. Examples: `request.header.x-request-verification`, `request.content`

#### `request(*string* url, *string* content = '', *string* method = 'GET', *array* headers = []) : array`

Sends a HTTP request and returns an array with the following keys: `content`, `status`, `headers`, `url`.

The headers should be an array of strings in the form of `Header-Key: Value`.

#### `respond(*string* content, *int* status, *array* headers)`

Halts Custom Action execution and return a response

#### `set_content(*string* content)`

Sets the response content of the URL

#### `set_header(*string* header_name, *string* header_value)`

Sets or overwrites a response header of the URL. Headers should be an array of strings e.g. `["X-Example: Value"]`

#### `set_response(*string* content, *int* status, *array* headers)`

Set response content, status and headers in single function

#### `set_status(*number* status)`

Sets the http status of the URL

#### `set_variable(*string* variable_name, *string* variable_value)`

Sets a variable for usage in a later (Script) action

#### `stop()`

Stop executing current script.

#### `variables : array`

Global variable containing an associative array with all available Webhook.site variables

### String

#### `hash_md5(*string/number* value) : string`

Returns md5 hash of value

#### `hash_sha256(*string/number* value) : string`

Returns sha256 hash of value

#### `json_decode(*string* json) : array`

Decodes `json` and returns an array

#### `json_encode(*array* array) : string`

Takes an array and encodes it as a JSON string

#### `json_path(*string* json, *string* jsonpath, *bool* return_first = true) : string`

Returns parses the `json` string using the JSONPath functionality.

Per default, if there's just one match (e.g. if matching on a property value that's a string), this value is returned. To always return an array, set `return_first` to false. 

```javascript
dump(json_path('{"v": []}', 'v[*]', false))
dump(json_path('{"v": []}', 'v[*]', true))
// []
// ""
 
dump(json_path('{"v": ["item1"]}', 'v[*]', false))
dump(json_path('{"v": ["item1"]}', 'v[*]', true))
// [0: "item1"]
// "item1"
 
dump(json_path('{"v": ["item1", "item2"]}', 'v[*]', false))
dump(json_path('{"v": ["item1", "item2"]}', 'v[*]', true))
// [0: "item1", 1: "item2"]
// [0: "item1", 1: "item2"]
```
#### `regex_match(*regex* regex, *string* subject) : string`

Returns the first matching string, otherwise false

#### `regex_to_string(*regex* regex) : string`

Returns the regex converted to a string

#### `string_contains(*string* subject, *number/string/regex* value) : bool`

Returns boolean if *subject* contains *value*

#### `string_find_first(*string* subject, *number/string* value) : number`

Returns position of *value* in *subject*, or false if not found

#### `string_find_last(*string* subject, *number/string* value) : number`

Returns position of *value* in *subject*, or false if not found

#### `string_format(*string* formatString, ...*any* items) : string`

Sprintf-like formatting of formatString with *items*, see PHP sprintf docs.

#### `string_join(*string* subject, *array* items (number/string/bool/array)) : string`

Joins items with string *subject*

#### `string_length(*string* string) : number`

Returns length of string (multibyte-aware)

#### `string_number_of(*string* string) : number`

Returns number value of *string*

#### `string_replace(*string* subject, *string* search, *string* replace) : string`

Replaces string *search* with *replace* found in *subject*.

#### `string_reverse(*string* subject) : string`

Reverses string *subject* 

#### `string_shuffle(*string* string) : string`

Returns shuffled string

#### `string_split(*string* subject, *string/regex* delimiter) : string`

Returns array of split string *subject* with *delimiter* 

#### `to_regex(*string* regex) : regex`

Converts a regex string to a regex type

#### `to_string(*string/number/bool* value) : string`

Returns *value* as string 

### Math and Numbers

#### `abs(*number* number) : number`

#### `atan(*number* number) : number`

#### `ceil(*number* number) : number`

#### `cos(*number* number) : number`

#### `floor(*number* number) : number`

#### `number_length(*number* number) : number`

#### `pow(*number* number, *number* power) : number`

#### `round(*number* number, *number* precision) : number`

#### `sin(*number* number) : number`

#### `sqrt(*number* number) : number`

#### `tan(*number* number) : number`

#### `to_number(*any* value) : number`

### Arrays

#### `array_contains(*array* array, *string/number* needle) : bool`

Returns true or false depending on whether *array* contains *needle* 

#### `array_copy(*array* array) : array`

Returns a copy of *array*

#### `array_diff(*array* array1, *array* array2) : array`

Returns the items of array1 that are not present in array2 while keeping the array indices.

#### `array_get(*array* array, *string/number* index, *any* default) : any` 

#### `array_has(*array* array, *string/number* key) : bool`

#### `array_length(*array* array) : number` 

#### `array_map(*array* array, *func* function)`

Runs function with each array value, and returns array with key as result

#### `array_number_of(*array*, *string/number* value) : number`

Returns amount of *value*

#### `array_pop(*array* array) : any`

Pop element off end of array

#### `array_push(*array* array, *any* value) : any`

Adds *value* to end of *array* and returns *value*

#### `array_random(*array* array) : any`

Returns random value of *array* 

#### `array_reverse(*array* array) : array` 

Returns *array* in reverse order

#### `array_shuffle(*array* array) : array`

Returns shuffled version of *array*

#### `to_array(*array* array) : array`

Returns *array*.

### Booleans

#### `bool_and(*bool* value1, *bool* value2): *bool*`

Returns `value1 && value2`

#### `bool_not(*bool* value) : *bool*`

Returns `!value`

#### `to_bool(*string/number/array/bool* value) : *bool*`

Casts `value` to a bool.

### Types

#### `type(*any* value) : *string*`

Returns the type name of value, e.g. `string`.

