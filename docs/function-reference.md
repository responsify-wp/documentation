##Reference
RWP is providing the following functions:

- ``rwp_img( $attachment_id, $settings )``
- ``rwp_picture( $attachment_id, $settings )``
- ``rwp_span( $attachment_id, $settings )``
- ``rwp_attributes( $attachment_id, $settings )``
- ``rwp_style( $attachment_id, $settings )``
  
###Settings
These are the settings that is currently avaliable:

* **sizes** (array): The image sizes that you want to use.
* **media_queries** (array): An associative array of names of the image sizes and a custom media query.
* **attributes** (array): An associative array of attribute names and values that you want to add on the element (see example).
* **retina** (bool|string|array): True/false. String or array of pixel densities (x descriptor).
* **ignored_image_formats** (array): An array of image formats that you want RWP to ignore.

####Example - sizes

````php
<?php
$settings = array(
	'sizes' => array('medium', 'large')
);

echo rwp_img( $attachment_id, $settings );
echo rwp_picture( $attachment_id, $settings );
?>
````

````html
<img sizes="(min-width: 300px) 1024px, 300px"
    srcset="large.jpg 1024w,
  medium.jpg 300w"
    alt="Image description">
    
<picture>
    <source srcset="large.jpg" media="(min-width: 300px)">
    <img srcset="medium.jpg" alt="Image description">
</picture>
````

####Example - custom media queries

````php
<?php
$settings = array(
	'sizes' => array('thumbnail', 'medium', 'large'),
	'media_queries' => array(
		'medium' => 'min-width: 500px',
		'large' => 'min-width: 1024px'
	)
);
echo rwp_picture( $attachment_id, $settings );
?>
````

Notice that you **should not** specify a media query for the smallest image size. You **must** specify a media query for 
all the selected image sizes.

````html
<picture>
    <source srcset="large.jpg" media="(min-width: 1024px)">
    <source srcset="medium.jpg" media="(min-width: 500px)">
    <img srcset="thumbnail.jpg" alt="Image description">
</picture>
````

####Example - attributes

#####img

````php
<?php
$settings = array(
	'attributes' => array(
		'id' => 'responsive-image',
		'sizes' => '(min-width: 500px) 1024px, 300px'
	)
);
echo rwp_img( $attachment_id, $settings );
?>
````

````html
<img id="responsive-image" sizes="(min-width: 500px) 1024px, 300px" 
	    srcset="thumbnail.jpg 150w,
	    medium.jpg 300w,
	    large.jpg 1024w,
	    full-size.jpg 1440w"
	    alt="Image description">
````

#####picture

````php
<?php
$settings = array(
	'attributes' => array(
		'picture' => array(
			'id' => 'picture-element'
		),
		'source' => array(
			'data-foo' => 'bar'
		),
		'img' => array(
		    'id' => 'responsive-image'
		)
	)
);
echo rwp_picture( $attachment_id, $settings );
?>
````

````html
<picture id="picture-element">
    <source data-foo="bar" srcset="full-size.jpg" media="(min-width: 1024px)">
    <source data-foo="bar" srcset="large.jpg" media="(min-width: 300px)">
    <source data-foo="bar" srcset="medium.jpg" media="(min-width: 150px)">
    <img id="responsive-image" srcset="thumbnail.jpg" alt="Image description">
</picture>
````

#####span

````php
<?php
$settings = array(
	'attributes' => array(
		'picture_span' => array(
			'id' => 'picture-element',
			'data-alt' => 'Overrides the alternative text of the image'
		),
		'src_span' => array(
			'class' => 'responsive-image'
		)
	)
);
echo rwp_picture( $attachment_id, $settings );
?>
````

````html
<span data-picture id="picture-element" data-alt="Overrides the alternative text of the image">
	<span class="responsive-image" data-src="thumbnail.jpg"></span>
	<span class="responsive-image" data-src="medium.jpg" data-media="(min-width: 150px)"></span>
	<span class="responsive-image" data-src="large.jpg" data-media="(min-width: 300px)"></span>
	<span class="responsive-image" data-src="full-size.jpg" data-media="(min-width: 1024px)"></span>
	<noscript>
		<img src="thumbnail.jpg" alt="Image description">
	</noscript>
</span>
````