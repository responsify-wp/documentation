#Retina
A new feature in Responsify WP 1.7 is the support for high resolution (retina) images. This means that devices with 
high pixel density screens will receive larger images which will look crisp and sharp.  
However, larger images means heavier images, which is something RWP is meant to prevent. With that said, this is how 
the generated markup looks like:  

````html
<!-- Picture element -->
<picture>
    <source srcset="large.jpg, large_retina.jpg 2x" media="(min-width: 300px)">
    <source srcset="medium.jpg, medium_retina.jpg 2x" media="(min-width: 150px)">
    <img srcset="thumbnail.jpg" alt="Image description">
</picture>

<!-- img with srcset/sizes -->
<img srcset="
	thumbnail.jpg 150w,
	medium.jpg 300w,
	medium_retina.jpg 600w,
	large.jpg 1024w,
	large_retina.jpg 2048w"
	sizes="(min-width: 300px) 1024px, (min-width: 150px) 300px, 150px">
````

Notice that this is the first version of the retina feature, so if you comes across any bug or unexpected behavior, 
please notify me at the [support forum](https://wordpress.org/support/plugin/responsify-wp) or here at Github.

##Setup  
If you want to use retina images, they has to be generated first. This requires you to manually add custom image sizes 
using the [`add_image_size`](http://codex.wordpress.org/Function_Reference/add_image_size) function.  
Let's use the three default image sizes that ships with WordPress and their default settings as an example:

- `thumbnail` 150x150px
- `medium` 300x300px
- `large` 1024x1024px

If you want to create retina versions of these image sizes, you should add something like this to your `functions.php`:

````php
<?php
add_image_size( 'medium@2x', 600, 600 );
add_image_size( 'large@2x', 2048, 2048 );
?>
````

As you can see, the `@2x` suffix indicates that `medium@2x` is twice as large as `medium` for example. 
There's no `thumbnail@2x` since that would be the same as `medium`.  
Images that are uploaded to WordPress from now on will be generated in these sizes. If you want to create retina versions 
of existing images, use the [Regenerate thumbnails](https://wordpress.org/plugins/regenerate-thumbnails/) plugin.  
RWP works with other pixel densities beside of `2x` as well. `1.5x` or `3x` will also work just fine. The important thing 
is that you name your custom image sizes like this: 

````
[original]@[value]x`  
````

##Examples  
Add `1.5x` and `2x` versions of the default image sizes: 
````php
<?php
add_image_size( 'thumbnail@1.5x', 225, 225 );
add_image_size( 'medium@1.5x', 450, 450 );
add_image_size( 'medium@2x', 600, 600 );
add_image_size( 'large@2x', 2048, 2048 );
?>
````

It of course works with other custom sizes as well!

````php
<?php
add_image_size( 'tablet-landscape', 800, 600 );
add_image_size( 'tablet-landscape@2x', 1600, 1200 );
add_image_size( 'tablet-landscape@3x', 2400, 1800 );
?>
````

`thumbnail`, `medium` and `large` will work as usual:

````html
<!-- Picture element -->
<picture>
    <source srcset="large.jpg" media="(min-width: 800px)">
    <source srcset="tablet_landscape.jpg, tablet_landscape@2x.jpg 2x, tablet_landscape@3x.jpg 3x" media="(min-width: 150px)">
    <source srcset="medium.jpg" media="(min-width: 150px)">
    <img srcset="thumbnail.jpg" alt="Image description">
</picture>

<!-- img with srcset/sizes -->
<img srcset="
	thumbnail.jpg 150w,
	medium.jpg 300w,
	tablet_landscape.jpg 800w,
	tablet_landscape@2x.jpg 1600w,
	tablet_landscape@3x.jpg 2400w,
	large.jpg 1024w,"
	sizes="(min-width: 800px) 1024px, (min-width: 300px) 800px, (min-width: 150px) 300px, 150px">
````