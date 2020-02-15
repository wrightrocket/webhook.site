---
title: Examples
nav_order: 500
parent: WebhookScript
---

# WebhookScript Examples
{: .no_toc }

Do you have a nice example to share with other users? Please contact [support@webhook.site](mailto:support@webhook.site) 😊

1. TOC
{:toc}

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

## Send a x-www-form-urlencoded request

```javascript
content = query([
    'country': 'Curaçao', 
    'population': 158665
])
headers = ['Content-Type: application/x-www-form-urlencoded'];
response = request('https://example.com', content, 'POST', headers);
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

## Telegram bot

The Messaging service Telegram allows bots using their API. The general principle is this:

1. [Create a Bot using the /newbot command](https://core.telegram.org/bots#6-botfather) sent to the *BotFather* Telegram User
2. Using the bot token sent from *BotFather*, use the Telegram API to [create a Webhook subscription](https://core.telegram.org/bots/api#setwebhook) (using your Webhook.site URL)
3. Add some logic using WebhookScript!

Note: Everywhere you see `TELEGRAM_TOKEN`, replace it with the token you got from BotFather!

### Subscribe to Webhook

To create the Webhook subscription, change the token and the Webhook.site URL to your own and go to the following URL in your browser:

https://api.telegram.org/bot**TELEGRAM_TOKEN**/setWebhook?url=**https://webhook.site/a1351781**

You should get a response similar to this:

```json
{
  "ok": true,
  "result": true,
  "description": "Webhook was set"
}
```

### First incoming Webhook

When you add your bot to your Telegram contacts list, Telegram automatically sends a `/start` command to the bot, which triggers a Webhook similar to this:

```json
{
  "update_id": 176446573,
  "message": {
    "message_id": 1,
    "from": {
      "id": 2346545645,
      "is_bot": false,
      "first_name": "Simon",
      "language_code": "en"
    },
    "chat": {
      "id": 34534673234,
      "first_name": "Simon",
      "type": "private"
    },
    "date": 1581706369,
    "text": "/start",
    "entities": [
      {
        "offset": 0,
        "length": 6,
        "type": "bot_command"
      }
    ]
  }
}
```

You should be able to see this in the Webhook.site requests list.

From this, we have all the parts needed to build a script that answers to commands:

```javascript
// Telegram API token
token = 'TELEGRAM_TOKEN';

content = json_decode(var('$request.content$'));
msg = content['message']['text'];

// Default message
response = "Couldn't come up with anything witty.";

if (msg == "How's it going?") {
    response = 'Pretty good.'
}
if (msg == "How's the weather?") {
    response = 'Raining.'
}
if (msg == "/start") {
    response = "Hi! I'm WebhookBot."
}

// Send a JSON request to the Telegram API using the chat ID and the response message
json = [
    'chat_id': content['message']['chat']['id'],
    'text': response
];
request('https://api.telegram.org/bot{}/sendMessage'.format(token), json_encode(json), 'POST');
```

Simply copy this script into a WebhookScript Custom Action (remember to change the token!), and click Save Action.

!["WebhookScript" Telegram Bot screenshot](/webhookscript/example-telegram-bot-script.png)

Then, you can interact with the bot using the Telegram app:

!["WebhookScript" Telegram Bot screenshot](/webhookscript/example-telegram-bot.png)

And that's it! Congratulations on your bot. It's not very smart, but from here, the possibilities are endless!