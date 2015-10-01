# Instagram Feed

Represents the instagram feed of a user.


## Instructions

### Register your app on Instagram

First you have to register your application on Instagram. 

Enter a name, description and (really important) the correct redirect url. You can add more than one. If you want to generate the code and access token to authenticate against instagram from the ProcessWire module setting page, you have to add a url similar as: ``http://page.dev/processwire/module/edit?name=InstagramFeed/``. Just install the module and copy the url inside the address bar.

![Register your app on Instagram](https://github.com/justonestep/processwire-instagramfeed/blob/master/screens/instagram-register.png)

When you've done you'll receive a Client ID and a Client Secret. 

![Receive Client ID and Secret](https://github.com/justonestep/processwire-instagramfeed/blob/master/screens/instagram-show.png)

### Install module

Next you have to install the module and enter at least Client ID and Client Secret in module settings.

Click submit.

### Generate access token

There are a few steps involved in the authentication process:

* You get directed to the Instagram website to allow access to your application
* You get a code back from Instagram
* This code is used to send a request to Instagram to get an access_token

If you entered the correct redirect url (instagram) and saved Client ID as well as Client Secret (ProcessWire module settings), you only have to click the link **get Access Token** in module settings.

If everything went well you'll see an access token in module settings. 

The access token does not specify an expiration time. Either the user revokes access, or Instagram expires the token after some period of time. 
Just click the link again to create a new access token.

![Generate Access Token](https://github.com/justonestep/processwire-instagramfeed/blob/master/screens/module-generate.png)

### More settings..

You can specify some more settings:

* **Username**: Instagram username to display content from (default: self)
* **Image Count**: Count of media to return

### ... and use it!

For example:

**PHP**

```php

<?php $feed = $modules->get('InstagramFeed')->getRecentMedia(); ?>

<div class="instagram>
  <?php foreach ($feed as $image): ?>
    <a href="<?=$image['link']; ?>" class="instagram-item">
      <picture>
        <source media="(min-width: 55rem)" srcset="<?=$image['images']['standard_resolution']['url']; ?>">
        <source media="(min-width: 45rem)" srcset="<?=$image['images']['low_resolution']['url']; ?>">
        <source srcset="<?=$image['images']['thumbnail']['url']; ?>">
        <img src="<?=$image['images']['thumbnail']['url']; ?>" alt="">
      </picture>
    </a>
  <?php endforeach; ?>
</div>

```

**Twig**

```html
{% set feed = modules.get('InstagramFeed').getRecentMedia() %}

<div class="instagram>
  {% for image in feed %}
    <a href="{{image.link}}">
      <picture>
        <source media="(min-width: 55rem)" srcset="{{image.images.standard_resolution.url}}">
        <source media="(min-width: 45rem)" srcset="{{image.images.low_resolution.url}}">
        <source srcset="{{image.images.thumbnail.url}}">
        <img src="{{image.images.thumbnail.url}}" alt="">
      </picture>
    </a>
  {% endfor %}
</div>
```

![Example](https://github.com/justonestep/processwire-instagramfeed/blob/master/screens/feed.png)

You can output any value explained [here](https://instagram.com/developer/endpoints/users/#get_users_media_recent).

If you want to output another feed, you can pass an username:

* ``$modules->get('InstagramFeed')->getRecentMedia('username');`` 

Priority:

1. argument passed by
2. username entered in module settings
3. default 'self', to get the most recent media published by the owner of the access token

**Links:**

* [oauth authentication](http://codular.com/oauth-authentication-with-instagram)
* [Authentication](https://instagram.com/developer/authentication/)
* [Endpoints](https://instagram.com/developer/endpoints/users/)
