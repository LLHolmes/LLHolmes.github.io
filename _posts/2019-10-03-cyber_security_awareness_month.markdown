---
layout: post
title:      "Cyber Security Awareness Month"
date:       2019-10-03 21:51:36 +0000
permalink:  cyber_security_awareness_month
---


For the most part, we want to see the best in people.  We want to believe that our fellow human beings are good, and upright, and would choose to do the “right” thing.

Well, *you* may be trustworthy.  Your dog is OBVIOUSLY trustworthy.  You may even trust your mom.  But that still leaves over 7.5 billion unverified people out there…and don’t kid yourself, not everyone of them is honorable.

### Happy Cyber Security Awareness Month!!!

![](https://media.giphy.com/media/Q2W4hziDOyzu0/giphy.gif)

On one hand, as developers, I feel like we have a responsibility to protect our users, our apps, and our data.  One of the basic ways we can do this is to protect our sites from Cross Site Scripting (XSS).

> Cross-site scripting (XSS) is a type of computer security vulnerability typically found in web applications. XSS enables attackers to inject client-side scripts into web pages viewed by other users. A cross-site scripting vulnerability may be used by attackers to bypass access controls such as the same-origin policy.
> 
> ~ [Wikipedia](https://en.wikipedia.org/wiki/Cross-site_scripting)

There are tutorials out there for any language you're using.  Basically, an XSS vulnerability is when a user enters input that can be interpreted by the browser as HTML / XML.  It can lead to anything as simple as an annoying alert, an embarrassing image, or a nefarious link.  To try to protect our users, we "escape" or "sanitize" our user's input.  There are so many terms that touch this issue that get used interchangeably from time to time, so I wanted to visit what they actually do.

* **Escaping** is the process of converting a special character to it’s escape sequence.  For example, `<` is represented as `&lt;` and `>` is represented as `&gt;`.  This way, if a user inputs something like `“<script></script>"`, it won’t be interpreted as an HTML tag and the script won’t be run, it will instead be seen as content and will appear as a words on the page.  This is typically used on the front end when displaying content to the browser.

* **Filtering** is similar to escaping, except that the special characters are removed from the string, not converted.

* **[Sanitizing](https://en.wikipedia.org/wiki/HTML_sanitization)** typically assumes it's receiving HTML code and removes "unsafe" HTML tags like `<script>`, while allowing "safe" tags like `<b>`.  Depending on the language being used it can also be a bit more aggressive, running a whole gamut of conversions like removing extra white spaces, escaping special characters, filtering *all* tags, and possibly even adding validation.

* **[Serializing](https://en.wikipedia.org/wiki/Serialization)** is the process of converting the input to a form that can be transferred from the front end to the back end or vice versa.  How secure it is depends on the serializer being used.  For example, in JavaScript, `body = JSON.stringify(data);` takes the data and converts it to a string to be passed.  Whereas using [serialize-javascript](https://www.npmjs.com/package/serialize-javascript) in the form of `body = serialize(data, {isJSON: true});` will escape the output as well.

* **[Validating](https://www.whitehatsec.com/glossary/content/input-validation)** checks to make sure the data input matches a given set of requirements and is usually used for making sure the correct type of input is received (versus effecting security).  For example, making sure a telephone number has the required number of digits.

This is just the most basic security we as web developers can implement for our users.

On the other hand, we as users also need to protect ourselves.  Not to be overly dramatic, but Deep Throat had it right: 

![](https://i.kym-cdn.com/photos/images/original/001/183/656/f88)

I’ll add: verify everything.

It’s YOUR personal identity, YOUR credit score, YOUR financial well-being.  There are a lot of people out there trying to take advantage, and only one person protecting you…*YOU!*  If you don’t look out for yourself, no one else will.

We've all heard the generic solutions: 
* Don’t click suspicious links...
 
* Don’t share your personal information...
 
* Check for credit card skimmers at the gas pump...
 
* If someone wants to give you money for free, it's probably a scam...
 
* If your boss sends you an email asking you to send lots of money to an off-shore account, pick up the phone and get verbal confirmation before making that transaction…

* Back up your data to an external hard drive, especially if you're a business...

And do me one more favor.  That mom you think is so trustworthy…have a conversation  with her about phishing scams.  They’re still out there because people still fall for them.

Much love!  -  Make good choices!  -  Be safe!
