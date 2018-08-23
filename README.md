# Spoooky Basics
Spoooky is a Instant HTML Personalisation Service. It allows to create content very similar to what `Mail Merge` does with email, but with Presentations, Websites, Landing Pages, Videos & Ads. It then shares with you a summary of all the viewers that have accessed your content for marketing and follow up purposes.

It essentially replaces `{{parameters}}` placed in the HTML content, with personalised variables you feed it. 

## URL Variables
The easiest and most dynamic way to feeding Spoooky with dynamic content is the use of URL Variables. IF you are familiar with Google Analytics Tracking links `?utm_campaign=something&utm_medium=somethingElse`, this will be very natural to you. You can ammend any custom variable to the urls you send to your correspondents `?shoesize=44&height=194cm&favorite_color=yellow` and use them to customise the content they see. Spoooky variables and other variables play along nicely.

## Unknown Viewers
In the use case that you would like to personalise content for an new audience, you can request the required variables with a popup before they access the content. This is helpful when you not reach out to existing customer by the means of an email newsletter, but use Advertising instead.

## Social Variables
To make the optin more user-friendly, we have added the ability for viewers to use Social oAuth. This will give them access to your personalised content with one-click, and will share with you their; first & last name, profile picture  & email address (on Facebook) plus their jobtitle, company name, company website & logo url (on Linkedin). 

## Shared Variables
Shared variables is the information we deem you are interested in to know about your leads, and they are shared with you to a webhook of your choice to follow up with them, just like your would after they opt-in on an email newsletter. They currently include, as available: `fname`
`lname`
`proflepic`
`email`
`phone`
`jobtitle`
`company`
`website`
`logo`

All other variables you add by the means of URL Variables will be used in the Personalisation of the content, but will not we incldued in the shared info.

NOTE: all Flags and Variables should be url encoded.

# Sample Markup
To identify variables we enclose them with `{{ }}` so the for the url: 

https://yourwebsite.com/?fname=John&lname=Doe&jobtitle=CEO&company=Spoooky&website=spooo.ky

We would place the variables in the content like:
`{{fname}}`
`{{lname}}`
`{{jobtitle}}`
`{{company}}`
`{{website}}`

We can also use fallbacks, if variables are not available, to avoid blanks.:
`{{website || company || "some text"}}`

> this will use the value for `website`, unless its emtpy, then it will use `company`, unless its empty, then it will use `some text`.

NOTE: if you want to use text instead of a variable, you have to put it in between `"quotes"`

To keep a proper read and flow of context, you might want to pre/post fixes to the variables.
`{{"prefix" + website }}`

To combine fallbacks with pre/post fixes, you need to use IF statements:

`{{fname ? ("Dear" + fname) : "Hey There"}}`

This works like:

`{{ if this ? then this : else this }}`

> these statements can be nested, and you can make them as complex as you like.

You can also use the `mobile` parameter, to show different content on mobiles or desktops.

`{{mobile ? "swipe up" : "press space"}}`

This works like:

`{{if mobile ? then this : else this}}`

## Simple Title Animations
Rotator allows you to change a line of text. A nice usecase is the page title (remember to use the `og:title` to get readable rich previews. the final number is the time between text changes. Add a `,` between each sentence.

`{{rotate (fname + " Lets Talk", "Personalised Marketing", "For " + ( company || website || "Your Company"), 2)}}`

You can also add animated icons:

`{{marquee ()}}`

And change marquee symbol and max amount:

`{{marquee ("*", 3)}}`

`{{marquee ("@", 5)}}`

## Dates
To give your content an up-to-date touch, you can use the current data including offsets.

for the current date:   `{{date ()}}`

for tomorrow            `{{date (1)}}`

for next week           `{{date (7)}}`

You can also exclude business days from the count, like:

`{{date (2, 'Saturday, Sunday')}}`

> on a Friday, this would print Tuesdays date.

If you would like to make dates persistent, from the first time someone saw the date, use:

`{{persistentDate (10)}}`

`{{persistantDate (7, 'Saturday, Sunday')}}`

> visiting the content days later will not update the date.

## Countdowns
To add a sense of urgency to your content, you can use countdowns in the same way.

`{{countdown (1, 'hour')}}`

`{{countdown (1, 'day') || 'Time is Up'}}`

> you can define the fallback text to show when the countdown reaches 0.

Countdown skipping non-business days <br>

`{{countdown (7, 'day', 'Saturday, Sunday') || 'time is up'}}<br>`

To make a countdown continue from the first visit, you use the persistent variant:

`{{persistentCountdown (1, 'day') || 'Offer Expired'}}`

## Advanced Variables
When you create some more complex nested statements, you can create your own combined variables for repetitive use.

`{{client = website || (company ? company + "'s website" : "your website")}}`

The nest time you can then just use `{{client}}` to get the same result. 

## Advanced Usages
You are not limited to using the variables in content only, you can also use them to customise redirect, iframes, and alike.

For example, you can include a custom iframe of the recipients website:

`<iframe src="https://{{website}}" height=480 width=640></iframe>`

You can include a custom map of the recipients city:

`<iframe src="https://maps.google.com/maps?q={{city||"Amsterdam"}}&z=12&output=embed" width="600" height="450" frameborder="0" style="border:0" allowfullscreen></iframe>`

Or you can link your CTA button to a page depending on whether the countdown has run out already:

`https://yourwebsite.com/{{countdown (1, "day") ? "ealy-bird-offer" : "signup"}}`

For testing, check out [this playground](https://jsfiddle.net/hopla_tools/0zLu7rya/)

# System Flags, Defaults & Hierarchy.
To make Spoooky function we have chosen to use defaults so you get started straight away without having to configure everything. The following are some system flags we use to control Spoooky behaviour.

## Pop Up Behaviour
In compliance with GDPR we need to get the viewers consent to share with you their personal information. Hence we use a popup where they can submit, update & confirm their data. We have several options to invoke this popup, and alter its options.

auth_when=

never = the popup will show never (mainly for testing your content personalisation)

always = the popup will show always

nodata = the popup will show when there is no data (anonymous users only)

once = the popup will show only the first time (like a cookie consent notice)

nosocial = (default) the popup will show when there is no previous social login.

auth_login=

F = Facebook

L = Linkedin

E = Email

> any combination can be used: `LF`, `EL`, `LFE` (default), `L`, `F`, `FE`, `FL`.
 
auth_skip=

yes = Shows a close button, and allows to continue to the content without sharing details. (default)

no = Requires sharing details to continue to the presentation.
 
## Popup Styling
To make the popup match your brand, you can customise it using the following variables:

auth_logo= change the logo shown at the top (url)

auth_header= change the popup title (text)

auth_icon= change the button icon (url)

auth_cookie= change the link to your privacy policy (url)

auth_privacy= change the link to your privacy policy (url)

auth_color= change the default red color to one matching your brand (hex)

## (Hardcoded) Flags, Vars and the Hierarchy
Besides adding Flags and Vars via the URL, you can also add them as script attributes.

`<script src=“https://api.spooo.ky/bit.js?api_key=XXX” data-set-auth_skip="no" data-set-auth_login="FL"></script>`

> This way you can prevent URLs becoming very, very, very long. :)

So: `data-set-[any_var_name]="any_value"` attribute works as default var setter

When both URL variables and script attributes are used, keep it mind:
`URL Variables` overwrite `Script Attributes` overwrite `Spoooky Defaults`

## Clean URL Variables
We have a system in place to clean the urls, for a couple of reasons:

1) They can become very long, and very messy. We prefer them tidy.
2) We do not want to spoil the surprise of the personalisation
3) If your viewers share the presentation, we don't want them to share it with their details.
4) We don't want to pass Personal Identifiable Information in any other way

We cannot, however, clear all URL Variables, as many systems depend on them to operate (Like Google Analytics and their `utm_*` paramters). We therefore only clean our `Shared Variables`. This are the Varaibles in our case that include PII. 

If you want to clean your own variables from the URL too, you can do this with a script attribute:

`<script data-clean="shoesize,height,favorite_color" src="https://api.spooo.ky/bit.js?api_key=XXX"></script>`

So: `data-clean="[comma_separated_variables]"` attribute works as var cleaner.

# How to Install Spoooky
Just add the script, including your api_key, in the `<head>` of your HMTL content. 
 
`<script src="https://api.spooo.ky/bit.js?XXX”</script>`

# Spoooky Cookies, Sessions and Retargeting.
We use cookies to identify repetitive viewers, to make the personalised experience as seamless as possible. Cookies can be linked and merged based on `Email`, `Linkedin` and `Facebook` data. This allows us to provide cross-browser and cross-device tracking, for an optimal Dynamic Ad Retargeting experience.

To stop Spoooky Retargeting, a user has to add `?doNotTrack=true` to *any* URL using the Spoooky Service, and hit _ENTER_. This will clear all the cookies, pixels and sessions we use, and all linked data.

For more info check out our [cookie](https://spooo.ky/cookie-policy) and [privacy](https://spooo.ky/privacy-policy) policies

# Done-For-You Personalised Marketing and Advertising Solution
Is the implementation of this system over your head. Please contact [BrandPixel.io](https://brandpixel.io) for a full-fledged professional solution of creating and/or personalising your marketing creatives.

