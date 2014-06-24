# easy-wordpress

Easy (and sensibly separated) composer-ised install of wordpress for deployment to Heroku

I've included the plugin ```tantan-s3-cloudfront``` to allow uploading assets outside of Heroku's read-only filesystem. This will require an Amazon S3 bucket and settings on the Admin panel. That's beyond the scope of this document, but I might be persuaded to write a wiki page :-)

## Acknowlegments
I used ideas from 
* [roots.io](http://roots.io/using-composer-with-wordpress/) (I really like their attempt to make Wordpress behave like a [12-factor](http://12factor.net) app)
* [mhoofman/wordpress-heroku](https://github.com/mhoofman/wordpress-heroku)

I very much liked the approach taken with [roots/bedrock](https://github.com/roots/bedrock), but it was a little more complex than I was looking for

## Wordpress on Heroku in seven steps

```
git clone https://github.com/andypalmer/easy-wordpress.git my-new-wordpress
cd my-new-wordpress
heroku apps:create my-new-wordpress
heroku addons:add cleardb:ignite
heroku config:add DATABASE_URL=`heroku config:get CLEARDB_DATABASE_URL`
git push heroku master
heroku open
```

#### Optional tweak
Go through the installation process. When complete, log into the site, click on Settings and remove the trailing ```/wp``` from ```Site Address```

## Warnings
The free plan for ClearDB is limited to 5MB, which is quite small. After you've tested the install, you'll probably want to upgrade your plan or use a different MySQL provider

We haven't set any salt values, so Wordpress will generate some for us. These might not be as secure as the [salts from the API](https://api.wordpress.org/secret-key/1.1/salt/)

## Additional settings
I've modified the wp-config.php to pick up the majority of the settings from the environment. Have a look at [```wp-config.php```](https://github.com/andypalmer/easy-wordpress/blob/master/wp-config.php)

Some interesting ones to play with are the salt values (see warning above) and ```TABLE_PREFIX``` which lets you have multiple installations in the same database
