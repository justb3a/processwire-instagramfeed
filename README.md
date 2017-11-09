# Instagram Feed

Represents the instagram feed of any user or recently tagged media.

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

**Note:** Since 1. of June 2016 every new app created starts in Sandbox mode (Apps are restricted to 10 users, data is restricted to the 10 users and the 20 most recent media from each of those users). [read more](https://www.instagram.com/developer/sandbox/)

### More settings..

You can specify some more settings:

* **Username**: Instagram username to display content from (default: self)
* **Image Count**: Count of media to return

### ... and use it!

For example:

```php
<?php $feed = $modules->get('InstagramFeed')->getRecentMedia(); ?>

<div class="instagram">
  <?php foreach ($feed as $media): ?>
    <?php if ($media['type'] === 'image'): ?>
      <a href="<?=$media['link']; ?>" class="instagram-item">
        <picture>
          <source media="(min-width: 55rem)" srcset="<?=$media['images']['standard_resolution']['url']; ?>">
          <source media="(min-width: 45rem)" srcset="<?=$media['images']['low_resolution']['url']; ?>">
          <source srcset="<?=$media['images']['thumbnail']['url']; ?>">
          <img src="<?=$media['images']['thumbnail']['url']; ?>" alt="">
        </picture>
      </a>
    <?php else: ?>
      // output video  
    <?php endif; ?>
  <?php endforeach; ?>
</div>

```

![Example](https://github.com/justonestep/processwire-instagramfeed/blob/master/screens/feed.png)

You can output any value explained [here](https://instagram.com/developer/endpoints/users/#get_users_media_recent).

If you want to output another feed, you can pass a username:

* ``$modules->get('InstagramFeed')->getRecentMedia('username');`` 

Priority:

1. argument passed by
2. username entered in module settings
3. default 'self', to get the most recent media published by the owner of the access token

## Available endpoints

### Get the instagram feed of any user

- getRecentMedia($username = 'self')

```php
<?php $feed = $modules->get('InstagramFeed')->getRecentMedia(); ?>
```

### Get a list of recently tagged media

- getRecentMediaByTag($tagname)

```php
<?php $feed = $modules->get('InstagramFeed')->getRecentMediaByTag('processwire'); ?>
```

### Get a list of recent comments on a media object.

- getRecentComments($media)

```php
<?php $comments = $modules->get('InstagramFeed')->getRecentComments($media); ?>
```

For example:

```php
<?php 
$feed = $modules->get('InstagramFeed')->getRecentMedia(); ?>

<div class="instagram>
  <?php foreach ($feed as $media): ?>
    <?php if ($media['type'] === 'image'): ?>
      <a href="<?=$media['link']; ?>" class="instagram-item">
        <img src="<?=$media['images']['thumbnail']['url']; ?>" alt="">
      </a>

      // display comments
      $comments = $modules->get('InstagramFeed')->getRecentComments($media); ?>
      <?php if ($comments): ?>
        <ul>
          <?php foreach ($comments as $comment): ?>
            <li><?=$comment['text']></li>
          <?php endforeach; ?>
        </ul>
      <?php endif; ?>
    <?php endif; ?>

  <?php endforeach; ?>
</div>
```

### Override the imageCount value

- setImageCount($int)

This is a way of setting the `imageCount` value when initialising the module.
It might come in handy if you want to show different number of images on different pages.

```php
<?php $feed = $modules->get('InstagramFeed')->setImageCount(2)->getRecentMedia();  ?>
```

**Links:**

* [oauth authentication](http://codular.com/oauth-authentication-with-instagram)
* [Authentication](https://instagram.com/developer/authentication/)
* [Endpoints](https://instagram.com/developer/endpoints/users/)
* [Get Your Instagram Access Token](http://instagram.pixelunion.net/)
* [Sandbox Mode](https://www.instagram.com/developer/sandbox/)
