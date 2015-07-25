#Functions 
RWP provides a number of functions that can generate responsive images in your templates. 
##Element / Img
Based on an attachment ID, you can generate either a **picture** element or a **img** tag with ``srcset``/``sizes`` attributes.

````php
<?php echo rwp_img( $attachment_id ); ?>
````
 
````php
<?php echo rwp_picture( $attachment_id ); ?>
````

Let's say that you have the following markup for a very large header image:

````html
<header>
	<?php the_post_thumbnail( 'full' ); ?>
</header>
````

As you probably know, ``the_post_thumbnail()`` will just create a regular ``<img>`` tag for the full-size image in this case. 
But you does of course not want to send a big 1440px image to a mobile device. This can easily be solved like this:

````html
<header>
	<?php
	$thumbnail_id = get_post_thumbnail_id( $post->ID );
	echo rwp_picture( $thumbnail_id );
	?>
</header>
````

You can also specify which sizes of the image that should be used:

````php
<?php 
$thumbnail_id = get_post_thumbnail_id( $post->ID );
echo rwp_picture( $thumbnail_id, array(
	'sizes' => array( 'medium', 'large', 'full' )
) ); 
?>
````

##Style
There might be times when you have a ``div`` with a very large background image. It's very easy to replace the image with a smaller one using media queries in your stylesheet, but that requires you to hard code the filename.  
What if it's some kind of header image that can be changed later by the administrator of the site? In that case, you cannot hard code the filename inside your stylesheet.  
You might have done something like this in the past:

````html
<header id="hero" style="background-image: url(<?php echo $dynamic_header_image;?>)">
	<h1>Overlying headline</h1>
</header>
````

Instead, you could do this:

````php
<?php
echo rwp_style( $dynamic_header_image_ID, array(
	'selector' => '#hero'
) );
?>
<header id="hero">
	<h1>Overlying headline</h1>
</header>
````

This will generate a ``style`` tag containing the following:

````css
#hero {
	background-image: url("thumbnail.jpg");
}
@media screen and (min-width: 150px) {
	#hero {
		background-image: url("medium.jpg");
	}
} 
// And so on...
	
````

You can of course specify sizes and media queries here to.

````php
<?php
echo rwp_style( $dynamic_header_image_ID, array(
	'selector' => '#hero',
	'sizes' => array('medium', 'large', 'full'),
	'media_queries' => array(
		'large' => 'min-width: 500px',
		'full' => 'min-width 1024px'
	)
) );
?>
````
##Attributes
The ``rwp_attributes()`` function returns an array of attributes for the selected element. This might be 
useful if you for example want to create the elements later with Javascript.

````php
<?php
$img_attributes = rwp_attributes( $attachment_id, array(
	'element' => 'img'
) );

// $img_attributes equals this:
$img_attributes = array(
	'srcset' => 'thumbnail.jpg 150w, medium.jpg 300w, large.jpg 1024w',
	'sizes' => '(min-width: 300px) 1024px (min-width: 150px) 300px, 150px'
);
?>
````

Of course, this works with the picture element to:

````php
<?php
$picture_attributes = rwp_attributes( $attachment_id, array(
	'element' => 'picture'
) );

// $picture_attributes equals this:
$picture_attributes = array(
	'source' => array(
		array(
			'srcset' => 'large.jpg',
			'media' => '(min-width: 300px)'
		),
		array(
			'srcset' => 'medium.jpg',
			'media' => '(min-width: 150px)'
		),
	),
	'img' => array(
		'srcset' => 'thumbnail.jpg'
	)
);
?>
````

It is also possible to pass custom settings to the function.

````php
<?php
$picture_attributes = rwp_attributes( $attachment_id, array(
	'element' => 'picture',
	'sizes' => array('thumbnail', 'medium'),
	'media_queries' => array(
		'medium' => 'min-width: 1024px'
	)
) );

// $picture_attributes equals this:
$picture_attributes = array(
	'source' => array(
		array(
			'srcset' => 'medium.jpg',
			'media' => '(min-width: 1024px)'
		)
	),
	'img' => array(
		'srcset' => 'thumbnail.jpg'
	)
);
?>
````