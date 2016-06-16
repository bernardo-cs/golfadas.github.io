---
layout: post
title:  "Loading Rails fixtures into Heroku 😱"
date:    2016-06-16
categories: fixtures rails heroku
---

When loading rails fixtures into an Heroku instance you might get the following warning `WARNING: Rails was not able to disable referential integrity.`

And afterwards a bunch of errors follow:


{% highlight bash %}
cause: PG::InsufficientPrivilege: ERROR:  permission denied: "RI_ConstraintTrigger_a_1339057" is a system trigger
{% endhighlight %}


Fear not! This is the way rails has to tell you that it doesn't have permissions to overwrite referential permissions. Referential permissions are triggered by SQL table references.

An example of an SQL table reference is user_id on a posts table that could look like the following:


{% highlight bash %}
id    user_id   body
-------------------------------
1     1         When loading...

{% endhighlight %}

Referential integrity is used to make sure that whenever you create a post it always belongs to an existing user. This is good, but when loading fixtures, rails might first try to load all posts without having any users on the database, which will trigger referential integrity errors.

The problem is that on Heroku, rails doesn't have permissions to disable referential integrity. Referential integrity is enforced by postgres constrains, so we can just drop them, and then add them back.

If you want an automatic solution, look into [this](http://pvcarrera.github.io/general/2016/02/15/rails-fixtures-and-referencial-integrity.html) blog post by @pvcarrera. Otherwise, this is how you can do it by yourself:

First boot up your database console with:

{% highlight bash %}
heroku run rails db:console
{% endhighlight %}

See the constrain name by describing the table at hand:

{% highlight bash %}
\d posts
{% endhighlight %}

Rails foreign key constrains will be under `Foreign-key constraints:` look something like `fk_rails_ac850f983f`.
Delete the constrain by:

{% highlight sql %}
ALTER TABLE posts DROP CONSTRAINT fk_rails_ac850f983f;
{% endhighlight %}


You can now load your migrations 👏

Don't forget thought! After loading your migrations, you should recreate your constrains by:

{% highlight sql %}
ALTER TABLE posts ADD CONSTRAINT fk_rails_ac850f983f
FOREIGN KEY (user_id) REFERENCES users(id);
{% endhighlight %}

