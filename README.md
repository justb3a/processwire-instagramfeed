# Instagram Feed

Represents the instagram feed of any user.

* [oauth authentication](http://codular.com/oauth-authentication-with-instagram)
* [Authentication](https://instagram.com/developer/authentication/)
* [Endpoints](https://instagram.com/developer/endpoints/users/)

## Instructions

### Register you app on Instagram

First you have to register your application on Instagram. 

Enter a name, description and (really important) the correct redirect url. You can add more than one. If you want to generate the code and access token to authenticate against instagram from the ProcessWire module setting page, you have to add a url similar as: ``http://page.dev/processwire/module/edit?name=InstagramFeed/``. Just install the module and copy the url inside the address bar.

When you've done you'll receive a Client ID and a Client Secret. 

### Install module

Next you have to install the module and enter at least Client ID and Client Secret in module settings.

Click submit.

### Generate access token

There are a few steps involved in the authentication process:

* You get directed to the Instagram website to allow access to your application
* You get given a code back from Instagram
* This code is used to send a request to Instagram to get an access_token

If you entered the correct redirect url (instagram) and saved Client ID as well as Client Secret (ProcessWire module settings), you only have to click the link **get Access Token** in module settings as described in **Instructions 3.**

If everything went well you'll see an access token saved in module settings. Anytime you want create a new access token, just click the link again.

### more settings..

You can specify some more settings:

* **Username**: Instagram username to display content from (default: self)
* **Image Count**: Count of media to return

### ... and use it!

For example:

```html
{% set feed = modules.get('InstagramFeed').init() %}

{% for image in feed %}
  <a href="{{image.link}}">
    <picture>
      <source media="(min-width: 45rem)" srcset="{{image.images.low_resolution.url}}">
      <source srcset="{{image.images.thumbnail.url}}">
      <img src="{{image.images.thumbnail.url}}" alt="">
    </picture>
  </a>
{% endfor %}
```

You can output any value explained [here](https://instagram.com/developer/endpoints/users/#get_users_media_recentI.

