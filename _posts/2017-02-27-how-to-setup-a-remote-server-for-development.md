---
layout: post
title: How To Setup A Remote Server for Development
date: 2017-02-27 00:35:00
categories:
  - web development
  - web design
  - how-to
featured_image:
summary_markdown: How to get a web server up and running (and why you would want to do it.)
comments: true
---


> Warning: This is a VERY geeky post, intended to be read by my web designer (and aspiring web designer) friends.  You’ve been warned.

In this post I’m going to explain the process of setting up a remote development server (A computer that runs all the time that hosts the code you are working on.)  When this is setup this way - you will no longer access your projects in a local folder and run programs on your folder using your own computer’s terminal.  You will instead work on your files right from the remote server and interact with them using SSH.

## But first, Why?

This isn’t **hard**. But it isn’t simple either.  So why would you want to do this?  Here are several reasons:

1. You’ll always have access to your code.

   If you do this, you will be able to access your current projects on every single computer that has access to the internet.  Want to work on a Chromebook or an iPad?  You can do it.  Want to jump on your wife’s or your kid’s computer, you can work on that too.  The locked down iMac at the oil change place or the chromebox at the library - you can work on those too.  You’ll always have access to your code.

2. You’ll only have to setup a dev environment once and it will work on all of your computers.

   A second advantage of this is that you will only have to setup CLI devtools (Gulp, Browsersync, Jekyll, Sass, etc.) on one computer.  So when you access your site on some random computer somewhere, you won’t have to go through the rigmarole of getting ruby setup.

3. You’ll be able to access the code you are working on in any browser, which will simplify stuff like browser testing.

   You’ll be able to setup a domain like dev.yourdomain.com where you can access your work on any computer.  You’ll be able to have your current project open on a second laptop, a phone, and a tablet and browsersync will allow you to code on one computer in full screen and see the changes take place across all of the devices at the same time.  You’ll be able to share a domain with clients who can see a work in progress.

There are other benefits too - chances are, for example, that your dev environment will be very similar to your production environment, that dev software will be easier to install, etc..

So, now that you know why…here are the steps to HOW:

## 1. Setup an Ubuntu box on Digital Ocean or Linode

Either one will work.  I have used both.  Currently, I use Linode.  You won’t need anything fancy - the base level box ($5 or $10 a month) will do just fine.  You’ll want to use the latest version of Ubuntu with LTS after it (currently it’s 16.04) and turn it on.

## 2. SSH into your box and create a user.

When you create one of these boxes, you’ll create a root password and be given a IP address for use with SSH.   (Something like 12.34.56.78) You’ll need to fire up your terminal and run `SSH root@THEIPADDRESS` . You are going to want to create a second user (you don’t want to work in root) and add that user to the group that is allowed to do `sudo`  commands.  Here is how you do that in Ubuntu 16.04:

```
adduser example_user
#This adds a new user called example_user, it will ask for a password.
adduser example_user sudo
#this adds example_user to the sudo group so that you can run the sudo command and have root access.
```

After you have done this, you’ll want to exit your SSH session by typing `exit` and then enter a new SSH session as your new user by typing `example_user@THEIPADDRESS`  The terminal should prompt you for your password and you are in.

**This is how you will access your machine in the future.**

## 3. (Optional) Add an A record to the domain that you want to use pointing to the server’s URL.

Everything works as is right now, but who wants to remember a random IP number (I don’t), so what you can do is login to the DNS settings of any domain that you own.  (We’ll use `YOURDOMAIN.COM` for this example.)  and add an A record (we’ll use `dev` for this example) that points to your new servers IP address.   After DNS propagates (it takes up to 24 hours) you’ll be able to type in `dev.YOURDOMAIN.com` in the browser or `ssh example_user@dev.YOURDOMAIN.com` in the terminal and access your stuff.

## 4. Install all of your command line tools.

You’ll want to SSH into your new server to start installing some tools.  Here are some you may want:

* Git - `sudo apt-get install git`
* Ruby - Follow this [tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-16-04)
* PHP - Follow this [tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04)
* Python - `sudo apt-get install python`
* Nodejs - Instructions [here](http://tecadmin.net/install-latest-nodejs-npm-on-ubuntu/#)
* Gulp - `npm install -g gulp`  requires Nodejs
* Jekyll - `gem install jekyll` requires Ruby
* Browser-sync - `npm install -g browser-sync`  require Nodejs
* Etc.

It’s outside the scope of this article to explain any of this in more detail. The beauty of the dev server is you only have to set these up once.  I can’t tell you how many times I’ve installed Ruby and run into a headache - having one place where I know it works is so helpful.

## 5. Make sure whatever build tool you use (Browser Sync, Jekyll, Etc.) outputs to 0.0.0.0

A regular part of the modern development workflow is to fire up a testing server (usually on port 3000 or 4000).  On a mac, you would get this at http://localhost:3000 or http://localhost:4000. But that won’t work with a dev server.

You can still run your favorite testing tools on your dev server, you will just need to be sure that it is posting to 0.0.0.0 rather than 127.0.0.1.  Then instead of hitting http://localhost:3000 you can hit http://yourIpAddress:3000.

This actually makes it super easy to use tablets, phones and other computers for testing regardless of your build tool.

## 6. Connect your IDE (or setup a cloud IDE)

You interact with the dev server’s terminal via SSH - but how do you edit files?  There are many ways to do this:

**On a Mac**

You can use Transmit (and I believe also Cyberduck and Forklift) to mount your SSH server over SFTP as a disk.  This then treats your server as a disk on your mac and you can edit in your coding tool of choice.

**On Windows**People still use windows? Just kidding.  You can do the same as above using a tool like [swish](http://www.swish-sftp.org/).

**On a Chromebook (Or any computer with an internet connection)**

You can use an online ide like Codetasty (my favorite), Cloud9, or Codeanywhere to mount your server over SSH.  These are all actually pretty good and I often use them to get coding work done on the go.

**On an iPad or iPhone**

Yes, you can.  You can use Coda by Panic to do both file editing and SSH.