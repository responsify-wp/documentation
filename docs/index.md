#Responsify WP
[Responsify WP](https://wordpress.org/plugins/responsify-wp/) is the WordPress plugin that cares about responsive images.

* Use ``img`` with srcset/sizes attributes.
* ...or the ``picture`` element.
* Works with or without [Picturefill](http://scottjehl.github.io/picturefill/).
* Supports high resolution images (retina).
* Custom media queries.
* Handpick which image sizes to use.
* Responsive background images.

**Basic demonstration**  
[https://www.youtube.com/watch?v=3ThYWO6vHKI](https://www.youtube.com/watch?v=3ThYWO6vHKI&spfreload=10)  
**Demo site**  
[http://responsifywp.com/demo/](http://responsifywp.com/demo/)

##<a name="description"></a>Description
In short, it will replace all ``<img>`` tags within ``the_content`` (or other filters that you can [add yourself](#filters)) with responsive images.
For example, you might have a template that looks like this:  

````html
<article>
	<h1><?php the_title();?></h1>
	<?php the_content();?>
</article>
````

This might output something like this:

````html
<article>
	<h1>Hello world</h1>
	<p>Lorem ipsum dolor sit amet...</p>
	<img src="full-size.jpg" alt="Image description">
</article>
````

But once you have activated the plugin, it will look like this instead:

````html
<article>
	<h1>Hello world</h1>
	<p>Lorem ipsum dolor sit amet...</p>
	<img sizes="(min-width 1024px) 1440px, (min-width: 300px) 1024px, (min-width: 150px) 300px, 150px" 
	    srcset="thumbnail.jpg 150w,
	    medium.jpg 300w,
	    large.jpg 1024w,
	    full-size.jpg 1440w"
	    alt="Image description">
</article>
````

On the RWP settings page, you can select between using an ``<img>`` tag with ``sizes``/``srcset`` attributes and the ``<picture>`` element.

````html
<picture>
    <source srcset="full-size.jpg" media="(min-width: 1024px)">
    <source srcset="large.jpg" media="(min-width: 300px)">
    <source srcset="medium.jpg" media="(min-width: 150px)">
    <img srcset="thumbnail.jpg" alt="Image description">
</picture>
````

Congratulations! You're now serving images with an appropriate size to the users.    
The different versions of the image in the examples above is in the standard ``thumbnail``, ``medium``, ``large`` and ``full`` sizes. 
Any **custom sizes** of the image will also be found and used.  
The **media queries** that the ``picture`` element are using is based on the width of the "previous" image.  

RWP also has support for the old markup pattern that uses ``<span>`` tags. However, this solution has some 
limitations and is not the recommended.

````html
<span data-picture data-alt="Image description">
    <span data-src="thumbnail.jpg"></span>
    <span data-src="medium.jpg" data-media="(min-width: 150px)"></span>
    <span data-src="large.jpg" data-media="(min-width: 300px)"></span>
    <span data-src="full-size.jpg" data-media="(min-width: 1024px)"></span>
    <noscript>
        <img src="thumbnail.jpg" alt="Image description">
    </noscript>
</span>
````
