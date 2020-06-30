---
title: News & Changelog
nav_order: 50
---

# News & Changelog

Subscribe below to receive updates about improvements and new features on Webhook.site. Expect a newsletter every month at the most.

<link href="//cdn-images.mailchimp.com/embedcode/horizontal-slim-10_7.css" rel="stylesheet" type="text/css">
<style type="text/css">
	#mc_embed_signup form {text-align: left;padding: 0;}
</style>
<div id="mc_embed_signup">
<form action="https://site.us19.list-manage.com/subscribe/post?u=a4698f7ac47cff759ecdcca24&amp;id=6c5386b81d" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank" novalidate>
    <div id="mc_embed_signup_scroll">
  	  <input type="email" value="" name="EMAIL" class="email" id="mce-EMAIL" placeholder="email address" required>
      <!-- real people should not fill this in and expect good things - do not remove this or risk form bot signups-->
      <div style="position: absolute; left: -5000px;" aria-hidden="true"><input type="text" name="b_a4698f7ac47cff759ecdcca24_6c5386b81d" tabindex="-1" value=""></div>
      <div class="clear"><input type="submit" value="Subscribe" name="subscribe" id="mc-embedded-subscribe" class="button"></div>
    </div>
</form>
</div>

## 23 June 2020

* Added new Dropbox Custom Actions: Create Folder, Download, Upload, Delete, Get Link. 

## 14 June 2020

* WebhookScript editor syntax highlighting improved with regards to multiline strings, escape characters and more
* New WebhookScript editor keyboard shortcut for saving (Mac: Cmd-S, Windows: Ctrl-S.)
* Fix WebhookScript fullscreen mode not being disengaged when saving
* URLs can now accept file uploads via multipart. File contents are available via variables: `request.file.<formname>.content`, `request.file.<formname>.size`, `request.file.<formname>.filename`
* New WebhookScript function: `csv_to_array()`, which converts a CSV file from a string to an array that can easily be parsed.
* Added an [example script](/webhookscript/examples#uploading-and-parsing-csv-file) demoing file uploads and CSV parsing

## 7 June 2020

* Added new Slack Send Message Custom Action, which lets you send Slack messages via a Slack Webhook URL.

## 5 June 2020

* Added "Formula Mode" checkbox for Add and Update Row Google Sheets actions, which parses values like entered in a cell, and allows inserting formulas.

## 4 June 2020

* Added new endpoints to fetch the content and information about the latest request on a URL. See [here](/api#get-single-request) and [here](/api#get-raw-request-content).

## 2 June 2020

* Added Amazon Web Services CloudFront Cache Invalidation Custom Action.
* Added ability to toggle Auto Cleanup, which automatically cleans up requests/emails.

## 30 May 2020

* Added Discord Custom Action for sending messages. [Read more in the docs](/custom-actions#discord)

## 29 May 2020

* New set of date functions added to WebhookScript, namely: `to_date()`, `date_format()`, `date_to_array()`, `date_interval()`, `date_interval_human()`. [Read more in the docs](webhookscript/functions#date-manipulation)

## 25 May 2020

* New set of Amazon Web Services S3 actions: Create Bucket, Put Object, Delete Object and Get Object (which retrieves object contents to a Variable.)

## 22 May 2020

* New Schedule intervals: every 1 minute and every 10 minutes, in addition to the already [existing ones](/schedules).

## 17 May 2020

* New WebhookScript function: `delay()`, which lets you delay and execute WebhookScript code a set amount of seconds in the future.
* New WebhookScript function: `exec()`, which lets you dynamically execute code in a string.
* New WebhookScript function: `import()`, which downloads and executes code from a URL - great if you want to reuse your code, just put it on Github and import it with the URL!

## 16 May 2020

* It's now possible to set a Token (URL or email address) to expire automatically, even for Premium users. This is useful for creating tokens for automated testing.

## 3 May 2020

* New Google Sheets Custom Actions (Beta). 3 initial actions are available: Append Row, Update Row and Get Values, which allow you to manipulate or retrieve the values of a Google Sheet without writing any code.

## 24 April 2020

* New fullscreen mode in WebhookScript editor
* Ability to edit your user profile
* New WebhookScript functions: `base64_encode()`, `base64_decode()`.

## 21 April 2020

* New WebhookScript function: store() - creates or updates an existing Global Variable

## 20 April 2020

* New Global Variables section in Control Panel.

## 19 April 2020

* New WebhookScript math functions: max(), min(), mod(), pi(), rand() - for more, see the [Reference](/webhookscript/reference)

## 28 March 2020

* New feature: Webhook.site Schedules lets you request any URL – including Webhook.site URLs – automatically on an interval, so you can for example run Custom Actions every 5 minutes, or make a health check for your Web site.
* Send Request action request timeout has been raised from 5 to 10 seconds. Applies to both the Custom Action and the WebhookScript request() function.

## 24 March 2020

* New feature: API Keys can now be created so you can use the API with URLs that have the "Require Authentication" or "Password" options set.
* Fix: It's now possible to test Send Email actions before creating them. Prior to this, the Test button would not do anything for the Send Email action.

## 6 March 2020

* New feature: Extract XPath Custom Action with accompanying xpath() WebhookScript function. [Read more.](/custom-actions#extract-xpath)

## 29 Feburary 2020

* Values are no longer required for Condition actions, so it's possible to compare an empty string or missing value.
* Fixed an issue where a tooltip in the Custom Actions modal would not disappear.
* Removed Beta label from WebhookScript.

## 27 Feburary 2020

* Fixed a bug where slashes at the end of Webhook.site Single Page App URLs didn't work.
* Fixed a bug where Copy To CURL sometimes wouldn't return a valid CURL command.

## 26 Feburary 2020

* It's no longer a requirement to specify a variable for the Send Request action.

## 25 Feburary 2020

* New feature: It's now possible to receive emails, which are treated like Webhooks – so you can automate emails with Custom Actions on Webhook.site. You can also test email deliverability using DKIM, DMARC and SPF validation.
* JSON formatting is now always enabled per default.
* The requests view is cleared after deleting the last request, and the tutorial text is shown.

## 16 Feburary 2020

* WebhookScript: Added `regex_extract` and `regex_extract_first` functions.

## 15 Feburary 2020

* WebhookScript: Added `hash` function.

## 14 Feburary 2020

* New feature: Export to CSV lets you export all requests on a given URL to a CSV file.
* Fixed a bug where duplicate in-app notifications would appear on new requests.

## 12 Feburary 2020

* WebhookScript: Added `url_encode`, `url_decode`, and `query` functions.

## 8 Febuary 2020

* New Custom Action type: Condition, which lets you conditionally stop a set of actions based on comparisons.

## 28 January 2020

* WebhookScript now supports newline literals (`\\n`) in strings, escaped by 2 backslashes.

## 25 January 2020

* Global Variables are now available in the Control Panel, which lets you keep configuration like API keys in a separate place from your Custom Actions and scripts while managing them at a central place.

* Changed the font of the WebhookScript editor, which resulted in uneven selection of text.

## 23 January 2020

* You can now specify a source variable for JSONPath and Regex actions, so you can extract text from not only the request content.

## 12 January 2020

* Switched to using Ace as editor for WebhookScript.