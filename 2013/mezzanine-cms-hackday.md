## Goals
As we are a few [Django](https://www.djangoproject.com/) lovers in Lausanne we (Sylvain, Noé and I) spent a hackday exploring [Mezzanine](http://mezzanine.jupo.org/), a very light CMS layer on top of this nice Python framework.

The goals of the day were:

* list usual client requirements in terms of CMS
* compare them to what Mezzanine offers out of the box
* see how feasible is the rest
* have some fun!

### The dream

So we took our usual postits and came up with a list of CMS needs. We then voted and assigned a value to each of them to finally get the following: (1=most, 6=least important)

* 1 editable static page
* 1 media management
* 1 menu management
* 1 wysiwyg
* 2 custom content type
* 2 multilingual
* 3 contact form
* 3 easy linking from page to page
* 3 register user
* 3 roles/permissions
* 4 create custom blocks
* 4 photo gallery
* 5 publication workflow
* 5 user profile (custom fields)
* 6 easy theme switch
* 6 image resize
* 6 inline edition
* 6 seo
* 6 url management

Of course this list is completely subjective but at least it gave us a pretty good start to evaluate Mezzanine.

### The reality

We then had a look at Mezzanine doc and played a bit with the demo to see what it could offer out of the box. The documentation is quite good and the UI sufficiently intuitive that after a little while we could make a first estimation at what is possible from our list with very minimum coding. Here is the list updated:

* (/) editable static page
* (/) media management (UI not fantastic)
* (/) menu management (todo: check how to define other menus)
* (/) wysiwyg (UI not fantastic)
* (/) custom content type
* (x) multilingual (obviously possible as it's just django, but integration in the admin interface is not offered out of the box)
* (/) contact form
* (/) easy linking from page to page
* (/) register user
* (/) roles/permissions
* (x) create custom blocks
* (/) photo gallery (UI not fantastic)
* (/) publication workflow (basic, check how/if it can be extended)
* (/) user profile (custom fields)
* (?) easy theme switch (to be investigated)
* (/) image resize
* (/) inline edition (basic, not to be compared with createjs)
* (/) seo
* (/) url management

We were pretty happy with these initial results, even though having multiple languages is probably a must have for most of our clients.
At this point we had two choices, either investigate the multi-language issue or install and make a dummy project using Mezzanine which would verify our assumptions.
As we didn't have any experience with this CMS we chose the second one.

### MUL (Mezzanine Lausanne University or Mezzanine Unicorn Lovers)

The project would be called MUL and would at least fullfil the following requirements:

* static pages
* 1 language
* 3 roles (teacher/student/admin)
* custom content types:
 * activity (title, body, max nb of participants, teacher, sesssions)
 * session (date)
 * news (title, slideshow of images, body)
* categories (activity kinds)
* user profile
* user registration
* teacher can offer activities
* student can subscribe to activities
* teacher can see how many participants registered to their activities

#### Playing (aka let's code!)

Creating custom content types in Mezzanine was really easy and we had activities and their related sessions in no time. The only thing is that we first wrongly assume we should extend the "Page" model before understanding that we didn't want Activities to be mixed together (ie. hierarchically organized) with the others pages, so we finally extended the "Displayable" behaviour.  
As Mezzanine "is just Django" creating relations between content types (activity/sessions) was a breeze and we could, on a single admin page, edit our activity and add multiple sessions.

As a developer point of view it's a real pleasure to be able to define content types as simple Django model, especially compared to let's say Drupal content type (...) It makes it also way easier to collaborate on a project and share.
On top of that you get basic front-end editing: you just add a pre-defined template tag around your field and once logged in when browsing your site you see an "edit" button next to each rendered field! This is nowhere as powerful as what you can do with [createjs](http://www.createjs.com/#!/CreateJS) for ex. but is a nice feature nevertheless.

As the included category possibilities were not enough for what we wanted (no hierarchy) we installed [django-categories](https://github.com/callowayproject/django-categories) (remember "it's just Django!") and voila! we could organize our activities in a nice hierarchy.

Mezzanine, through a fork of [django-filebrowser](https://github.com/sehmaschine/django-filebrowser), provides a simple but effective image management solution. By default all content types images / wysiwyg images are uploaded in a shared directory (different sub-folders) which makes it easy to re-use images.

With the very limited time we still got we added a Teacher profile (one to one relationship to Django User) and role, so that we could easily add some permission checks. Again fairly straightforward.

I personally had so much fun that i added student registration / subscription to activities on my spare time!

Mezzanine seems like a very nice way to tackle the CMS issue: provide devs a very thin layer containing necessary behaviours (ie. Sluggable, Displayable, Ownable..) as well as image management, and leaving you handle the rest through usual Django. I only wish they would put some effort on i18n so that more is offered out of the box...
Thumbs up!