---
layout: post
title: Laravel & Heroku Workflow
subtitle: Using Heroku as a staging environment for simple projects
---


Since some of our private work are simpler websites, with just some CRUD, a few mailers etc. it is probably neat to have a free staging environment to push your work to without the hassle, so that clients can see the progress.

In this guide you can learn how to deploy your Laravel apps on Heroku quickly.

You'll just need a Heroku account. You can create one [here](https://signup.heroku.com).

## Step 1

Create a new Laravel app: ``` composer create-project laravel/laravel heroku ```.
Now you should see this

![LaravelStarter](/img/Laravel.png)

Now you can go ahead and create a git repository in there.
Run `` git init `` to initialize a new repo.
Then, `` git add . && git commit -m "Initial Commit" ``

Now we're ready to push this up to our Github/Bitbucket repo if we want to, but not to Heroku just yet. So let's make that possible in step 2.

## Step 2

By default, Heroku launches an Apache server with PHP to serve apps from the root of the project. In our case the root is `/public` so we need to create a _Procfile_ that will configure that. If you want to use another web server you can look into that [here](https://devcenter.heroku.com/articles/custom-php-settings#specifying-which-web-server-to-use).

Use `echo "web: vendor/bin/heroku-php-apache2 public/" > Procfile` to create the Procfile.

Add and commit the files again.
`git add . && git commit -m "Created Procfile"`

Now we're ready to create a new Heroku app.

## Step 3

If you don't have Heroku Toolbelt installed, [install it now](https://toolbelt.heroku.com/).

Once the toolbelt is installed run `heroku create` to craft a new heroku app.
You should get a random app name similar to _calm-temple-91967_.

Since Laravel also includes a package.json file, Heroku will try to interpret the whole app as a Node.js app.

We need to create a buildpack to set the language to PHP. Proceed to step 4 to see how to do that.

## Step 4

To set the buildpack to PHP, run `heroku buildpacks:set heroku/php`
Now we need to tell heroku Laravel's `APP_KEY` environment variable value.

We can do that by generating a new one with `php artisan key:generate` inside our app. You should get an output like this

`Application key [XXzXcjl0HerRuxmIDN844wg3pN6YKNLk] set successfully.`

Now just set that same key in the Heroku config: `heroku config:set APP_KEY=XXzXcjl0HerRuxmIDN844wg3pN6YKNLk`.

You'll probably want to log errors to the Heroku log so just set
`'log' => 'errorlog'` in your app under config/app.php. 

Finally, run `git push heroku master` to push the app to heroku.
In the output you should get the app URL so you can visit it.

The output line that displays that looks something like this
`remote:        https://calm-temple-91967.herokuapp.com/ deployed to Heroku`

Now you are probably thinking, this is nice, but how do I create a database on Heroku? You can see that in my [Using MySQL on Heroku](/2016-02-07-using-mysql-on-heroku) guide.

