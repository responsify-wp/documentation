#Settings
##Image sizes
You can **select which image sizes** that the plugin should use from the settings page.  
These settings can be overwritten from your templates. 

````php
<?php

// Using get_posts()
$posts = get_posts( array(
	'post_type' => 'portfolio',
	'rwp_settings' => array(
		'sizes' => array('large', 'full')
	)
) );
foreach( $posts as $post ) {
	// ...
}

// Using WP_Query()
$query = new WP_Query( array(
	'category_name' => 'wordpress',
	'rwp_settings' => array(
		'sizes' => array('large', 'full')
	)
) );
if ( $query->have_posts() ) {
	// ...
}
?>
````

##Sizes attribute
By default, ``<img>`` tags with ``sizes``/``srcset`` is the selected markup pattern. ``100vw`` is the default value of 
the ``sizes`` attribute, but it's possible to specify your own. 
````php
<?php
$posts = get_posts(array(
	'post_type' => 'portfolio',
	'rwp_settings' => array(
		'sizes' => array('medium', 'large'),
		'attributes' => array(
			'sizes' => '(min-width: 500px) 1024px, 300px'
		)
	)
));
?>
````

This will produce the following:
````html
<img srcset="medium.jpg 300w, large.jpg 1024w" sizes="(min-width: 500px) 1024px, 300px)" src="medium.jpg">
````

``large.jpg`` will be selected when the window width is at least 500px. On smaller screens, ``medium.jpg`` will be selected.  

##Media queries
If you've selected the ``picture`` element in the settings, you can specify your own media queries for the different image sizes.

````php
<?php
$posts = get_posts(array(
	'post_type' => 'portfolio',
	'rwp_settings' => array(
		'sizes' => array('thumbnail', 'medium', 'large'),
		'media_queries' => array(
			'medium' => 'min-width: 500px',
			'large' => 'min-width: 1024px'
		)
	)
));
?>
````

In the example above, ``thumbnail`` is the smallest image size and should therefore have no media query specified. 
``medium`` will be selected if the screen is 500 px or larger. The same goes for ``large`` if the screen is at least 1024 px.

````html
<picture>
    <source srcset="large.jpg" media="(min-width: 1024px)">
    <source srcset="medium.jpg" media="(min-width: 500px)">
    <img srcset="thumbnail.jpg" alt="Image description">
</picture>
````

##Retina
On the RWP settings page, you can choose if high resolution (retina) images should be used or not. This can be overwritten 
by setting ``retina`` to either ``true`` or ``false`` in the ``rwp_settings`` array.  
If set to ``true``, RWP will use all images that has the ``@[value]x`` suffix in the name, like ``thumbnail@2x`` or 
``medium@1.5x``.

````php
<?php
$posts = get_posts(array(
    'rwp_settings' => array(
        'retina' => true // or false
    )
));
?>
````

In this example, the three image sizes has a retina version which is twice the size.

````html
<picture>
    <source srcset="full.jpg" media="(min-width: 1024px)">
    <source srcset="large.jpg, large_retina.jpg 2x" media="(min-width: 300px)">
    <source srcset="medium.jpg, medium_retina.jpg 2x" media="(min-width: 150px)">
    <source srcset="thumbnail.jpg, thumbnail_retina.jpg 2x">
    <img srcset="thumbnail.jpg" alt="Image description">
</picture>
````

You can also add image sizes for other pixel densities, like ``1.5x`` or ``3x``. This is the same example that also has a 
retina version that's three times as large.

````html
<picture>
    <source srcset="full.jpg" media="(min-width: 1024px)">
    <source srcset="large.jpg, large_retina.jpg 2x, large_retina_xl.jpg 3x" media="(min-width: 300px)">
    <source srcset="medium.jpg, medium_retina.jpg 2x, medium_retina_xl.jpg 3x" media="(min-width: 150px)">
    <source srcset="thumbnail.jpg, thumbnail_retina.jpg 2x, thumbnail_retina_xl.jpg 3x">
    <img srcset="thumbnail.jpg" alt="Image description">
</picture>
````

You can select which versions that you wanna use by default on the RWP settings page. This can also be overwritten by passing 
an array of pixel densities to ``rwp_settings``.

````php
<?php
$posts = get_posts(array(
    'rwp_settings' => array(
        'retina' => array( '1.5x', '2x' )
    )
));
?>
````

For a single density, you can of course pass it as a string.

````php
<?php
'rwp_settings' => array(
	'retina' => '2x'
)
?>
````