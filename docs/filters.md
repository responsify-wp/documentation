#Filters   
##Edit attributes  
The ``rwp_edit_attributes`` filter allows you to edit the attributes of the generated element.  
````php
<?php
function edit_attributes( $attributes ) {
	// Do something with $attributes...
	return $attributes;
}
add_filter( 'rwp_edit_attributes', 'edit_attributes' );
?>
````
**Example**
````html
<!-- Original image -->
<img src="large.jpg" class="size-large" alt="My image">
````
````php
<?php
function edit_attributes( $attributes ) {
	// $attributes equals this:
	// $attributes = array(
	//   'sizes' => '(min-width: 300px) 1024px, (min-width: 150px) 300px, 150px',
	//   'class' => 'size-large',
	//   'alt' => 'My image'
	// );
	return $attributes;
}
add_filter( 'rwp_edit_attributes', 'edit_attributes' );
?>
````
This can be useful if you for example want to edit the ``sizes`` attribute in specific situations.  
The following example overrides the generated ``sizes`` attribute when the original image has the ``size-large`` class.
````php
<?php
function edit_attributes( $attributes ) {
	if ( is_integer( strpos( $attributes['class'], 'size-large') ) ) {
		$attributes['sizes'] = '(min-width: 500px) 1024px, (min-width: 300px) 300px, 150px';
	}
	return $attributes;
}
add_filter( 'rwp_edit_attributes', 'edit_attributes' );
?>
````

##Add filters  
The ``rwp_add_filters`` filter allows you to add additional filters (confusing, I know) that RWP 
should be applied on.  
RWP is by default applied to the ``post_thumbnail_html`` and ``the_content`` filters. Any images found inside 
these filters will be made responsive.  

````php
<?php
function add_filters( $filters ) {
	$filters[] = 'my_custom_filter';
	return $filters; // Equals array( 'the_content', 'post_thumbnail_html', 'my_custom_filter' )
}
add_filter( 'rwp_add_filters', 'add_filters' );	
?>
````