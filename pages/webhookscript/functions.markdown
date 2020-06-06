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

Furthermore, functions that begin in a type can be referenced without it, for example, when calling the `format` function with the first argument being a string, the language infers that actually the `string_format` function should be used.

```javascript
echo(string_format('hello %s', 'world')) // hello world

'hello %s'.format('world').echo() // hello world
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

Stops Custom Action execution and adds `value` to script debug output.

### dump(***any*** value)

Adds `value` as a decoded string to script debug output.

## Custom Action Variables

These functions lets you interface with other Custom Actions by getting and setting variables from them. These functions are also how you retrieve Global Variables defined in the Control Panel.

### var(***string*** variable_name) : mixed

Retrieves the value of a Variable or Global Variable (defined in the Control Panel). The surrounding dollar signs are not mandatory.

Returns `null` if the variable does not exist.

```javascript
var('request.header.x-request-verification') // returns value of the `x-request-verification` header
```

### set(***string*** variable_name, ***string*** variable_value)

Sets a Variable for usage in current action execution. The Variable is available to downstream actions, but not stored permanently.

### store(***string*** global_variable_name, ***any*** value): ***any***

Permanently creates or updates a Global Variable (as defined in Control Panel.)

The value can be retrieved with the `var()` function in subsequent action executions.

### variables : array

A variable (not a function) containing an associative array with all available Webhook.site variables.

```javascript
user_agent = variables['request.header.user-agent']
```

## HTTP

### query(***array*** form_values) : string

Converts an associative array into a form-style string, used for e.g. `application/x-www-form-urlencoded` requests or HTTP query strings.

```javascript
query(['country': 'Curaçao', 'population': 158665]) // country=Cura%C3%A7ao&population=158665
```

### request(***string*** url, ***string*** content = '', ***string*** method = 'GET', ***array*** headers = []) : array

Sends a HTTP request and returns an array with the following keys containing response data:

* `content`
* `status`
* `headers`
* `url`

The headers should be an array of strings in the form of `['Header-Key-1: Foo', 'Header-Key-2: Bar']`.

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

```javascript
url_encode('here\'s a value') // here%27s+a+value
```

## Flow Control and Responses

### delay(***int*** seconds, ***string*** code)

Executes code in the future. Any output will be stored on the request and will show with a "Was delayed" label. 

The code will not inherit the execution scope.

In this example, the `format` function is used to prepare the code string with a URL, causing `{}` to be replaced with with `https://example.com`.

```javascript
code = '
  request(
      "{}",
      \'{"message": "Hello World!"}\',
      "POST"
  )
'

url = 'https://example.com'

delay(5, code.format(url));
```

The maximum amount of seconds allowed is 604800 (7 days).

### exec(***string*** code) : ***any***

Executes code in `code` and returns the result. The code will inherit the execution scope.

### import(***string*** url) : ***any***

Downloads code located at `url` and returns the result. The code will inherit the execution scope. 

As an example, this can be used if you want to re-use code. Just upload it to a server or e.g. Github and use it in different WebhookScript actions.

```javascript
result = import('https://raw.githubusercontent.com/fredsted/webhookscripts/ec22946a83ea85f607fcc6bff83f9d81ed2fe4ed/hello_world.ws')
echo(result) // value
```

### respond(***string*** content, ***int*** status, ***array*** headers)

Stops Custom Action execution and returns a response.

### stop()

Stops Custom Action execution.

### set_content(***string*** content)

Sets or overwrites the response content of the URL. Script execution continues.

### set_header(***string*** header_name, ***string*** header_value)

Sets or overwrites a response header of the URL. Script execution continues.

### set_response(***string*** content, ***int*** status, ***array*** headers)

Sets or overwrites response content, status and headers in single function.  Script execution continues.

`headers` should be an array of strings e.g. `["X-Example: Value", "X-Foo: Bar"]`.

### set_status(***number*** status)

Sets or overwrites the HTTP response status of the current URL.  Script execution continues.

## String

### base64_decode(***string*** string) : string

Returns a base64-encoded string.

### base64_encode(***string*** string) : string

Returns decoded base64 string.

### hash(***string/number*** value, ***string*** algo) : string

Returns a hashed version of `value` using the `algo` algorithm.

```javscript
hash('md5', 'hello world')  // 5eb63bbbe01eeed093cb22bb8f5acdc3
```



The following built-in algorithms are available: `md2`, `md4`, `md5`, `sha1`, `sha224`, `sha256`, `sha384`, `sha512/224`, `sha512/256`, `sha512`, `sha3-224`, `sha3-256`, `sha3-384`, `sha3-512`, `ripemd128`, `ripemd160`, `ripemd256`, `ripemd320`, `whirlpool`, `tiger128,3`, `tiger160,3`, `tiger192,3`, `tiger128,4`, `tiger160,4`, `tiger192,4`, `snefru`, `snefru256`, `gost`, `gost-crypto`, `adler32`, `crc32`, `crc32b`, `fnv132`, `fnv1a32`, `fnv164`, `fnv1a64`, `joaat`, `haval128,3`, `haval160,3`, `haval192,3`, `haval224,3`, `haval256,3`, `haval128,4`, `haval160,4`, `haval192,4`, `haval224,4`, `haval256,4`, `haval128,5`, `haval160,5`, `haval192,5`, `haval224,5`, `haval256,5`.

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

## Date Manipulation

In WebhookScript, dates are not a specific type, and ar expressed in strings instead, which WebhookScript will attempt to parse. If possible, we recommend using a specific date format like ISO-8601 which leaves little doubt for misunderstanding.

### Date format characters

WebhookScript uses the ISO format for converting and formatting dates, and the format is compatible with the [Moment.js format method](https://momentjs.com/docs/#/parsing/string-format/).

The following examples are based on the date `2017-01-05 17:04:05.084512`.

| Code      | Example       | Description                                                                                                                                       |
|-----------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| OD        | 5             | Day number with alternative numbers such as 三 for 3 if locale is ja_JP                                                                           |
| OM        | 1             | Month number with alternative numbers such as ၀၂ for 2 if locale is my_MM                                                                         |
| OY        | 2017          | Year number with alternative numbers such as ۱۹۹۸ for 1998 if locale is fa                                                                        |
| OH        | 17            | 24-hours number with alternative numbers such as ႑႓ for 13 if locale is shn_MM                                                                    |
| Oh        | 5             | 12-hours number with alternative numbers such as 十一 for 11 if locale is lzh_TW                                                                  |
| Om        | 4             | Minute number with alternative numbers such as ୫୭ for 57 if locale is or                                                                          |
| Os        | 5             | Second number with alternative numbers such as 十五 for 15 if locale is ja_JP                                                                     |
| D         | 5             | Day of month number (from 1 to 31)                                                                                                                |
| DD        | 05            | Day of month number with trailing zero (from 01 to 31)                                                                                            |
| Do        | 5th           | Day of month with ordinal suffix (from 1st to 31th), translatable                                                                                 |
| d         | 4             | Day of week number (from 0 (Sunday) to 6 (Saturday))                                                                                              |
| dd        | Th            | Minified day name (from Su to Sa), transatable                                                                                                    |
| ddd       | Thu           | Short day name (from Sun to Sat), transatable                                                                                                     |
| dddd      | Thursday      | Day name (from Sunday to Saturday), transatable                                                                                                   |
| DDD       | 5             | Day of year number (from 1 to 366)                                                                                                                |
| DDDD      | 005           | Day of year number with trailing zeros (3 digits, from 001 to 366)                                                                                |
| DDDo      | 5th           | Day of year number with ordinal suffix (from 1st to 366th), translatable                                                                          |
| e         | 4             | Day of week number (from 0 (Sunday) to 6 (Saturday)), similar to "d" but this one is translatable (takes first day of week of the current locale) |
| E         | 4             | Day of week number (from 1 (Monday) to 7 (Sunday))                                                                                                |
| H         | 17            | Hour from 0 to 23                                                                                                                                 |
| HH        | 17            | Hour with trailing zero from 00 to 23                                                                                                             |
| h         | 5             | Hour from 0 to 12                                                                                                                                 |
| hh        | 05            | Hour with trailing zero from 00 to 12                                                                                                             |
| k         | 17            | Hour from 1 to 24                                                                                                                                 |
| kk        | 17            | Hour with trailing zero from 01 to 24                                                                                                             |
| m         | 4             | Minute from 0 to 59                                                                                                                               |
| mm        | 04            | Minute with trailing zero from 00 to 59                                                                                                           |
| a         | pm            | Meridiem am/pm                                                                                                                                    |
| A         | PM            | Meridiem AM/PM                                                                                                                                    |
| s         | 5             | Second from 0 to 59                                                                                                                               |
| ss        | 05            | Second with trailing zero from 00 to 59                                                                                                           |
| S         | 0             | Second tenth                                                                                                                                      |
| SS        | 08            | Second hundredth (on 2 digits with trailing zero)                                                                                                 |
| SSS       | 084           | Millisecond (on 3 digits with trailing zeros)                                                                                                     |
| SSSS      | 0845          | Second ten thousandth (on 4 digits with trailing zeros)                                                                                           |
| SSSSS     | 08451         | Second hundred thousandth (on 5 digits with trailing zeros)                                                                                       |
| SSSSSS    | 084512        | Microsecond (on 6 digits with trailing zeros)                                                                                                     |
| SSSSSSS   | 0845120       | Second ten millionth (on 7 digits with trailing zeros)                                                                                            |
| SSSSSSSS  | 08451200      | Second hundred millionth (on 8 digits with trailing zeros)                                                                                        |
| SSSSSSSSS | 084512000     | Nanosecond (on 9 digits with trailing zeros)                                                                                                      |
| M         | 1             | Month from 1 to 12                                                                                                                                |
| MM        | 01            | Month with trailing zero from 01 to 12                                                                                                            |
| MMM       | Jan           | Short month name, translatable                                                                                                                    |
| MMMM      | January       | Month name, translatable                                                                                                                          |
| Mo        | 1st           | Month with ordinal suffix from 1st to 12th, translatable                                                                                          |
| Q         | 1             | Quarter from 1 to 4                                                                                                                               |
| Qo        | 1st           | Quarter with ordinal suffix from 1st to 4th, translatable                                                                                         |
| G         | 2017          | ISO week year (see ISO week date)                                                                                                                 |
| GG        | 2017          | ISO week year (on 2 digits with trailing zero)                                                                                                    |
| GGG       | 2017          | ISO week year (on 3 digits with trailing zeros)                                                                                                   |
| GGGG      | 2017          | ISO week year (on 4 digits with trailing zeros)                                                                                                   |
| GGGGG     | 02017         | ISO week year (on 5 digits with trailing zeros)                                                                                                   |
| g         | 2017          | Week year according to locale settings, translatable                                                                                              |
| gg        | 2017          | Week year according to locale settings (on 2 digits with trailing zero), translatable                                                             |
| ggg       | 2017          | Week year according to locale settings (on 3 digits with trailing zeros), translatable                                                            |
| gggg      | 2017          | Week year according to locale settings (on 4 digits with trailing zeros), translatable                                                            |
| ggggg     | 02017         | Week year according to locale settings (on 5 digits with trailing zeros), translatable                                                            |
| W         | 1             | ISO week number in the year (see ISO week date)                                                                                                   |
| WW        | 01            | ISO week number in the year (on 2 digits with trailing zero)                                                                                      |
| Wo        | 1st           | ISO week number in the year with ordinal suffix, translatable                                                                                     |
| w         | 1             | Week number in the year according to locale settings, translatable                                                                                |
| ww        | 01            | Week number in the year according to locale settings (on 2 digits with trailing zero)                                                             |
| wo        | 1st           | Week number in the year according to locale settings with ordinal suffix, translatable                                                            |
| x         | 1483635845085 | Millisecond-precision timestamp (same as date.getTime() in JavaScript)                                                                            |
| X         | 1483635845    | Timestamp (number of seconds since 1970-01-01)                                                                                                    |
| Y         | 2017          | Full year from -9999 to 9999                                                                                                                      |
| YY        | 17            | Year on 2 digits from 00 to 99                                                                                                                    |
| YYYY      | 2017          | Year on 4 digits from 0000 to 9999                                                                                                                |
| YYYYY     | 02017         | Year on 5 digits from 00000 to 09999                                                                                                              |
| YYYYYY    | +002017       | Year on 5 digits with sign from -09999 to +09999                                                                                                  |
| z         | UTC           | Abbreviated time zone name                                                                                                                        |
| zz        | UTC           | Time zone name                                                                                                                                    |
| Z         | +00:00        | Time zone offset HH:mm                                                                                                                            |
| ZZ        | +0000         | Time zone offset HHmm                                                                                                                             |

Source: [Carbon Docs](https://carbon.nesbot.com/docs/#api-localization)

### Date locales available

[Click here for a list of possible locales/translations available for date display functions.](/webhookscript/date-locales.html)

### to_date(***string*** date, ***?string*** format): ***string***

Will attempt to parse a string to a ISO-8601 formatted date string. 

If specified, `format` is used to parse the date without having to guess the format (see the [Date Format Characters](#date-format-characters) above.)

Many formats are supported, like relative dates and various specified date formats.

If the date is invalid or could not be guessed, `null` is returned.

```javascript
// Relative formats
'last wednesday 4 am'.to_date()        // 2020-05-27T04:00:00.000000Z
'first monday august 2019'.to_date()   // 2019-08-05T00:00:00.000000Z

// Automatic format guessing
'2020-01-01 23:02:01'.to_date()        // 2020-01-01T23:02:01.000000Z

// Unix timestamp
'@1215282385'.to_date()                // 2008-07-05T18:26:25.000000Z

// Custom date format
'2/4/12 06:03'.to_date('M/D/YY HH:mm') // 2012-02-04T06:03:00.000000Z

// To escape characters in the format string, backslashes can be used
'2020-01-05 12h30m15s'.to_date('YYYY-MM-DD HH\\hmm\\mss\\s') // 2020-01-05T12:30:15.000000Z
```

### date_format(***string*** date, ***?string*** format, ***?string*** locale): ***string***

Returns a date parsed to the format specified in `format`.

```javascript
date_format('2008-07-05T18:26:25.000000Z', 'YYYY-MM-DD') // 2008-07-05

date_format('2008-07-05T18:26:25.000000Z', 'LLLL', 'da') // lørdag d. 5. juli 2008 kl. 18:26

// If no format is specified, a default human readable readable string is returned
date_format('2008-07-05T18:26:25.000000Z') // Sat Jul 05 2008 18:26:25 GMT+0000
```

### date_to_array(***string***): ***array***

Returns an array containing all the components of a given date.

```javascript
dump(date_to_array('2008-07-05T18:26:25.324542Z'))

// [
//   "year": 2008,
//   "month": 7,
//   "day": 5,
//   "dayOfWeek": 6,
//   "dayOfYear": 187,
//   "hour": 18,
//   "minute": 26,
//   "second": 25,
//   "micro": 324542,
//   "timestamp": 1215282385,
//   "formatted": "2008-07-05 18:26:25",
//   "timezone": "Z"
// ]
```

### date_interval(***string*** date1, ***string*** date2, ***?string*** format): ***string/int***

Calculates the interval between `date1` and `date2`.

If no format string is specified, the interval is returned as the number of seconds between the dates, with the number being negative if `date2` is before `date1`.

Note that for the format string, this function does not use the ISO format, but rather the uses the [PHP `DateInterval` format specification](https://www.php.net/manual/en/dateinterval.format.php).

```javascript
date_interval('2008-07-16T23:13:26.234212Z', '2008-07-05T18:26:25.324542Z') // -967620

date_interval(
    '2008-07-16T23:13:26.234212Z',
    '2008-07-05T18:26:25.324542Z',
    '%d days, %h hours, %i minutes'
)
// 11 days, 4 hours, 47 minutes
```

### date_interval_human(***string*** date1, ***string*** date2, ***?string*** locale): ***string/int***

Formats the difference between 2 dates in a way that's easy to read for humans.

If no locale is specified, English is used.

```javascript
date_interval_human(
    '2008-07-16T23:13:26.234212Z',
    '2008-07-05T18:26:25.324542Z'
)
// 1 week after

date_interval_human(
    '2008-07-16T23:13:26.234212Z',
    '2008-07-05T18:26:25.324542Z',
    'es'
)
// 1 semana después
```


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

### array_get(***array*** array, ***string/number*** index, ***any*** default) : any

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

### array_reverse(***array*** array) : array

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
