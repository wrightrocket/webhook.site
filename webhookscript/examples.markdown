---
title: Examples
nav_order: 500
parent: WebhookScript
---

# WebhookScript Examples

Do you have a nice example to share with other users? Please contact [support@webhook.site](mailto:support@webhook.site) 😊

## Validate request

In this example, we use a common method of verifying webhooks by taking a hash of its contents concatenated to a secret. It demonstrates the way WebhookScript can get various information about the request by using the `get_variable()` function, as well as string concatenation, hashing, if statements and returning responses with content, status codes and headers using `respond()`, which halts execution.

```javascript
verification_secret = "JHRlc3RTY3JpcHRTZWNyZXQ";
verification_challenge = var("request.header.x-request-verification");
verification_result = hash_sha256(var('request.content') + verification_secret);

if (!verification_challenge or verification_challenge != verification_result) {
    respond("Invalid request", 500);
}

headers = ["X-Success: Yes", "X-Verification: "+verification_challenge];
respond("Successful request", 200, headers);
```

## Transform and resend

In the following, an incoming request is JSON decoded to an array, transformed and sent to "Web Service 1". Then the output is saved and passed on to "Web Service 2" in XML format. Basic error handling and validation is demonstrated.

```javascript
// Configuration, fetched from the users' Global Variables
ws1_api_key = var('WS1_KEY')
ws2_user_token = var('WS1_USER_TOKEN')

// Function for error handling
function error (message) {
    echo('Error: {}'.format(message))
    respond(json_encode(['error': message]), 500)
}

// Parse original request
orig_req = json_decode(var('request.content'))

// If the JSON was invalid
if (!orig_req) {
  error('Invalid request')
}

// Send request to Web Service 1, using format() for string placeholders
ws1_url = 'https://ws1.example.com/3.0/lists/{}/interest-categories/{}/interests'.format(
  list_id,
  group_id
)

ws1_content = [
  'first_name': orig_req['firstName'],
  'last_name': orig_req['lastName']
]
ws1_response = request(
    ws1_url,
    json_encode(ws1_content),
    'POST',
    ['Authorization: Basic ' + ws1_api_key]
)

// Don't go further if the Web Service 1 step didn't succeed
if (ws1_response['status'] != 200) {
  echo(ws1_response['content']); // Log content to output
  error('Invalid response from WS1')
}

// Get a value from the Web Service 1 request
ws1_response_id = json_decode(ws1_response)['id']

// Pass response on to Web Service 2 in XML format, using a multi-line string and format()
ws2_content = '
  <qdbapi>
    <usertoken>{}</usertoken>
    <listid>{}</listid>
    <field fid="7">{}</field>
  </qdbapi>'.format(ws2_user_token, orig_req['listId'], ws1_group_id)
  
ws2_response = request(
  'https://ws2.example.com/db/zzzzzz',
  ws2_content,
  'POST', 
  [
    "Action: API_EditRecord", 
    "Content-Type: application/xml"
  ]
)

if (ws2_response['status'] != 200) {
  echo(ws2_response['content']); // Log content to debug log
  error('Invalid response from WS2')
}

// Output the WS2 response content to debug output
echo(ws2_response['content'])
respond('OK', 200)
```
