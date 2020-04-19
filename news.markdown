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

## 19 April 2019

* New WebhookScript math functions: max(), min(), mod(), pi(), rand() - for more, see the [Reference](/webhookscript/reference.html)

## 28 March 2019

* New feature: Webhook.site Schedules lets you request any URL – including Webhook.site URLs – automatically on an interval, so you can for example run Custom Actions every 5 minutes, or make a health check for your Web site.
* Send Request action request timeout has been raised from 5 to 10 seconds. Applies to both the Custom Action and the WebhookScript request() function.

## 24 March 2019

* New feature: API Keys can now be created so you can use the API with URLs that have the "Require Authentication" or "Password" options set.
* Fix: It's now possible to test Send Email actions before creating them. Prior to this, the Test button would not do anything for the Send Email action.

## 6 March 2019

* New feature: Extract XPath Custom Action with accompanying xpath() WebhookScript function. [Read more.](/custom-actions.html#extract-xpath)

## 29 Feburary 2019

* Values are no longer required for Condition actions, so it's possible to compare an empty string or missing value.
* Fixed an issue where a tooltip in the Custom Actions modal would not disappear.
* Removed Beta label from WebhookScript.

## 27 Feburary 2019

* Fixed a bug where slashes at the end of Webhook.site Single Page App URLs didn't work.
* Fixed a bug where Copy To CURL sometimes wouldn't return a valid CURL command.

## 26 Feburary 2019

* It's no longer a requirement to specify a variable for the Send Request action.

## 25 Feburary 2019

* New feature: It's now possible to receive emails, which are treated like Webhooks – so you can automate emails with Custom Actions on Webhook.site. You can also test email deliverability using DKIM, DMARC and SPF validation.
* JSON formatting is now always enabled per default.
* The requests view is cleared after deleting the last request, and the tutorial text is shown.

## 16 Feburary 2019

* WebhookScript: Added `regex_extract` and `regex_extract_first` functions.

## 15 Feburary 2019

* WebhookScript: Added `hash` function.

## 14 Feburary 2019

* New feature: Export to CSV lets you export all requests on a given URL to a CSV file.
* Fixed a bug where duplicate in-app notifications would appear on new requests.

## 12 Feburary 2019

* WebhookScript: Added `url_encode`, `url_decode`, and `query` functions.

## 8 Febuary 2019

* New Custom Action type: Condition, which lets you conditionally stop a set of actions based on comparisons.

## 28 January 2019

* WebhookScript now supports newline literals (`\\n`) in strings, escaped by 2 backslashes.

## 25 January 2019

* Global Variables are now available in the Control Panel, which lets you keep configuration like API keys in a separate place from your Custom Actions and scripts while managing them at a central place.

* Changed the font of the WebhookScript editor, which resulted in uneven selection of text.

## 23 January 2019

* You can now specify a source variable for JSONPath and Regex actions, so you can extract text from not only the request content.

## 12 January 2019

* Switched to using Ace as editor for WebhookScript.