---
layout: post
title:  "Bootstrap Theming Part 2"
date:   2022-09-07 08:30:00 -0500
categories: Web Design, Web Development
---

## Sudden Urgency

I need my portfolio to modernize and get all repurposed and impressive looking for a potential training/ID employer/client and to accommodate all the new Web and App development projects I'm creating.

## Moving Ahead with Caution and Patience

I've done this sort of thing too many times and taking a two-hour task into a two-week nightmare.

Something to consider is that Bootstrap is, as of this moment, up to version 5. If memory serves, some of my favorite features required an older version of bootstrap *years ago*.

## Making Sense of all these tutes

I needed to

- compare all the tutes
- See what they had in common
- See what order those common steps were in

## Tutorial Elimination

Some I eliminated because I didn't want to make a theme from scratch -- I want to make an inomplete theme work. I need to learn what the pieces are and where they fit and just as importantly, how to make the blog page dynamically make pages.

I'm not looking to create a theme as much as customize one, I think.

**My hope** is that it will be simple copying and pasting for the most part.

Having said that and other stuff before it, I am adding the main, official site to the resources below because it's just overflowing with useful information.

## Creating the File Structure

Perhaps because I'm stupid, I'm not sketching anything out the wireframes on [How to Create a Custom Boostrap Theme from Scratch](https://designmodo.com/create-bootstrap-theme/) and some other site because the theme I want to customize already has layouts I love -- that's why I'm doing all this hard work. I suppose I could just look for the components I want but I want to learn how to hack around and put one of these together so, I'm moving straight on to creating a skeleton file structure. DesignModo's begins with this ...

root/
|—css/
|——custom.css
|—webfonts/
|—images/
|—js/
|——script.js
|—index.html

I love that because it's simple, but the tute is also over two years old. Design modo continues with 

"Download the Bootstrap source files and add them to our JavaScript and CSS folder. To do this, go to the Bootstrap download page and download the source files of any theme. Inside the download source files, we need to copy the bootstrap.min.css to our CSS folder. We also need to copy the bootstrap.min.js and bootstrap.bundle.min.js on our JavaScript folder."

That sounds super easy.

The official [Bootstrap Theming Documenation](https://getbootstrap.com/docs/4.1/getting-started/theming/) says:

"Whenever possible, avoid modifying Bootstrap’s core files. For Sass, that means creating your own stylesheet that imports Bootstrap so you can modify and extend it. Assuming you’re using a package manager like npm, you’ll have a file structure that looks like this:"

your-project/
├── scss
│   └── custom.scss
└── node_modules/
    └── bootstrap
        ├── js
        └── scss

It seems like I'm definitely using npm and gulp so ... first to make the file structure something like a combo of those two.

At this point, the official documentation became a bit useless to me so I closed it and stuck with DesignModo.

You'll notice, however, that DM's tute has only vanilla CSS but Slate comes with SCSS files ... so ...

## Gathering Resources

To get the rest of what I need, I did what DM told me and downloaded any old theme from the official Bootstrap page.

It seems Bootstrap now uses **SASS**, and, so far, all of them want me to use **Gulp**.

Wow there are *tons* of plugins and stuff this will need.

All the plugins seem to be in the themelight folder. 

I copied everything from themelight except the bootstrap.css which I took from Slate. No, cancel that. Right now we'll play it safe and just copy over all themelight stuff.

## Tutorial and Documentation

- [Bootstrap Official Theming Documenation](https://getbootstrap.com/docs/4.1/getting-started/theming/)
- [Get Bootstrap](https://getbootstrap.com/)
- [How to Create a Custom Boostrap Theme from Scratch](https://designmodo.com/create-bootstrap-theme/)
- [Slate from Bootswatch - the color theme I want](https://bootswatch.com/slate/)

### Nope

These were not chosen.

- [Bootstrap Builder](https://bootstrap.build/)
- [Boostrap Magic 4.0: Create Your Bootstrap 4.0 Themes Easily](https://pikock.github.io/bootstrap-magic/index.html)
- [Customize Bootstrap with the theme kit: How to build your own Bootstrap themes](https://hackerthemes.com/kit/)
- [How to Create Boostrap Theme from Scratch](https://www.webnots.com/beginners-guide-to-create-bootstrap-theme-from-scratch/)
- [How to Create Your Custom Bootstrap Theme](https://levelup.gitconnected.com/create-your-bootstrap-theme-4228aca9117a)
- [Official Bootstrap Guide to Customizing Themes](https://themes.getbootstrap.com/guide/)