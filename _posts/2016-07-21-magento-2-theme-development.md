---
layout: post
title:  "Magento 2 Theme Development"
date:   2016-07-21 15:02:44 -0400
categories: general
permalink: /blog/posts/magento-2-theme-development
tagline: Understanding & establishing best practices
author: Brad Leslie
---
I've been working on my first full Magento 2 build (custom functionality & theme development) and have uncovered some
really cool features. Although these features make our lives easier as developers, you have to know about them in order
to benefit. This will be an ever-evolving post about the things I've found.

<h2>view.xml</h2>

Magento's (improved) documentation definitely makes getting started with building a custom theme in M2 much more
streamlined than M1. One of my favorite features of M2 theme dev is briefly mentioned on the first page of the
<a href="http://devdocs.magento.com/guides/v2.1/frontend-dev-guide/themes/theme-create.html" target="_blank">theme documentation</a>.

Your theme's etc/view.xml file can be used to set the image sizes throughout the site, as mentioned in the documentation linked above,
but it's much more powerful than that. I've found that you can actually manipulate a lot of the front end via this file alone.
For example, you can set custom breakpoints, manually exclude JavaScript files and even customize the product page media gallery
as well as the image magnifier.

If you look at Magento_Catalog's etc/view.xml file and search for "gallery", you'll find that you have a lot of control
over the look and feel of the gallery -- simply by updating the values in your own theme's view.xml:

    <!-- Gallery and magnifier theme settings. Start -->
    <var name="gallery">
        <var name="nav">thumbs</var> <!-- Gallery navigation style (false/thumbs/dots) -->
        <var name="loop">true</var> <!-- Gallery navigation loop (true/false) -->
        <var name="keyboard">true</var> <!-- Turn on/off keyboard arrows navigation (true/false) -->
        <var name="arrows">true</var> <!-- Turn on/off arrows on the sides preview (true/false) -->
        <var name="caption">false</var> <!-- Display alt text as image title (true/false) -->
        <var name="allowfullscreen">true</var> <!-- Turn on/off fullscreen (true/false) -->
        <var name="navdir">vertical</var> <!-- Sliding direction of thumbnails (horizontal/vertical) -->
        <var name="navarrows">true</var> <!-- Turn on/off on the thumbs navigation sides arrows(true/false) -->
        <var name="navtype">slides</var> <!-- Sliding type of thumbnails (slides/thumbs) -->
        <var name="transition">
            <var name="effect">slide</var> <!-- Sets transition effect for slides changing (slide/crossfade/dissolve) -->
            <var name="duration">500</var> <!-- Sets transition duration in ms -->
        </var>
        <var name="fullscreen">
            <var name="nav">thumbs</var> <!-- Fullscreen navigation style (false/thumbs/dots) -->
            <var name="loop">true</var> <!-- Fullscreen navigation loop (true/false/null) -->
            <var name="arrows">true</var> <!-- Turn on/off arrows on the sides preview in fullscreen (true/false/null) -->
            <var name="caption">false</var> <!-- Display alt text as image title in fullscreen(true/false) -->
            <var name="navdir">horizontal</var> <!--Sliding direction of thumbnails in fullscreen(horizontal/vertical)  -->
            <var name="navarrows">false</var> <!-- Turn on/off on the thumbs navigation sides arrows(true/false) -->
            <var name="navtype">slides</var> <!-- Sliding type of thumbnails (slides/thumbs) -->
            <var name="transition">
                <var name="effect">slide</var> <!-- Sets transition effect for slides changing (slide/crossfade/dissolve) -->
                <var name="duration">500</var> <!-- Sets transition duration in ms -->
            </var>
        </var>
    </var>

    <var name="magnifier">
        <var name="fullscreenzoom">20</var>  <!-- Zoom for fullscreen (integer)-->
        <var name="top"></var> <!-- Top position of magnifier -->
        <var name="left"></var> <!-- Left position of magnifier -->
        <var name="width"></var> <!-- Width of magnifier block -->
        <var name="height"></var> <!-- Height of magnifier block -->
        <var name="eventType">hover</var> <!-- Action that atcivates zoom (hover/click) -->
        <var name="enabled">false</var> <!-- Turn on/off magnifier (true/false) -->
    </var>

I'm sure that there are a number of other benefits from manipulating view.xml values from other components, but I have
used it within my theme to customize the Catalog values for the most part.

<h2>display: flex;</h2>

The first thing I do when building a new theme is construct the header. Since it's used on every page, it offers a nice
jump-start to what can sometimes be a daunting challenge. When I started my first M2 theme and began moving header elements around,
I hit my first huge wall of M2 theme dev.

My goal was to move the 'top links' (account, welcome message, etc) into the main nav. My initial thought was to use the new
"move" XML syntax so that the top links and nav elements were displayed one-after-the-other, then float them both to give
the appearance of a single, unified navigation: category and other catalog-related links left aligned and top links right aligned.
Despite moving the top links successfully, all efforts to float either the top links or the nav failed.

After banging my head against the wall for more hours than I'd like to admit, I discovered something new:

    display: flex;

For the uninitiated, "flex" refers to <a href="https://css-tricks.com/snippets/css/a-guide-to-flexbox/" target="_blank">
Flexbox</a>. That guide by CSS-Tricks is a great way to bring yourself up to speed with Flexbox, what it can do and how
you can manipulate your M2 theme with it.

In my own words, it feels like an easier Bootstrap. If you wanted to build a
Magento theme using Bootstrap, you'd have to override dozens of template files in order to add custom classes to designate
the width of each element for each screen size (using their column system). Flexbox allows us to almost do the opposite,
meaning that you don't have to override a single template file -- it's all done via CSS / less. The guide above does a
great job of explaining Flexbox in detail, so I'll suffice it to say that your life (and your theme) will be easier if you
leverage Flexbox rather than trying to build a custom Bootstrap theme.

Getting back to the original point, child elements of a Flexbox cannot be floated. In order to achieve the desired result,
I was able to move the top links into the nav element itself. While I have not utilized Flexbox to its full potential in
my first theme, I will be looking for ways to do so in the future.

<h2>JavaScript Initialization</h2>

More kudos to Magento for <a href="http://devdocs.magento.com/guides/v2.1/javascript-dev-guide/javascript/js_init.html" target="_blank">
their documentation</a>, which does a good job of explaining JS initialization and how you can utilize it in your theme.
It's essentially a means to add custom JS without adding a new JS file. In addition to keeping performance at the forefront,
development time is decreased once you understand what you're able to accomplish.

Widgets such as accordion, tabs, navigation, etc. are able to be initialized with relative ease using the techniques outlined
in the documentation above. A good way to see how M2 utilizes JS initialization is to search your file system for "data-mage-init".
In addition to adding widgets, JS initialization is the preferred way to add custom JS to a single element. I'm looking forward
to diving into this aspect as I continue to customize my first theme.
