---
layout: post
title:  "Share Rails cookies between subdomains"
date:   2016-02-21
categories: rails domains
---

How can we share cookies between Rails sessions, or how can I stay logged in when accessing my website through www.mysite.com and mysite.com ?

Before we can answer that, here is what we need to know (alternatively skip to point 3 and have your answer):

1. How cookies work?
2. How can I have a www and naked subdomain locally?
3. How can Rails share cookies across subdomains.

## How cookies work
__Introduction__
REST presents us with a stateless client to server communication, where each URI represents a resource. 

How can we login a user, and know that a user is logged in, in a stateless communication? We have to pass the user, or something that lets us know who the user ( state ) is back and forth.

In order to keep track of the `current_user`, webservers generate a unique random token to identify the user and send it to the browser as a cookie.

In the image bellow, you can see the cookie value sent by the server.

{:.blog-image}
![rails cookies](/images/post_2/rails-session-cookie.png)

## How can I have a www and naked subdomain locally?

One way to setup is by forwarding your hosts file, and bind your Rails server to localhost. First run rails server with:

<pre>
  <code class="bash">
    rails s -b 0.0.0.0
  </code>
</pre>

In order to bind the server to localhost.

In order to redirect localhost to a domain and sub domain, go to the terminal and type:

<pre>
  <code class="bash">
  sudo vi /etc/hosts
  </code>
</pre>

add the following lines to your hosts file:

<pre>
  <code class="bash">
  127.0.0.1 yourwebsitename.com
  127.0.0.1 www.yourwebsitename.com
  </code>
</pre>


Now on your browser, if you go to `www.yourwebsite.com:3000` or `yourwebsite.com:3000` you will be forwarded to your development web server.

Awesome right? Well if login on `www.yourwebsite.com:3000` and then go to `yourwebsite.com:3000` you will see that you don't maintain your session, and will need to re login.

__What is going on here?__ Well, the thing is, browser cookies are stored TODO: explain how cookies are stored..

You can see that your browser is storing two different cookies for the browser and naked domain by going to the firefox `remove individual cookies option` and see that there are two different cookies. One set for the subdomain with www and another set to the naked domain:

TODO: add two-different-cookies.gif

## How can Rails share cookies across subdomains.
This is easy, the only thing you have to do is go to `config/initializers/session_store.rb` and add `domain: :all`, like:

<pre>
  <code class="bash">
Rails.application.config.session_store :cookie_store, key: '_yourapp_session', domain: :all
  </code>
</pre>

What happens now is TODO: explain what domain: :all does.
TODO: add image after-domain-all.png
