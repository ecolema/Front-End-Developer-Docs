# JavaScript Plugins #
- [JavaScript Plugins](#javascript-plugins)
  * [productGallery](#productgallery)
    + [Intro](#intro)
    + [Options](#options)
  * [imageContainer](#imagecontainer)
    + [Intro](#intro-1)
    + [CSS](#css)
    + [Multiple sizes (aspect ratios)](#multiple-sizes--aspect-ratios-)
    + [Options](#options-1)
  * [productCollection](#productcollection)
    + [Intro](#intro-2)
    + [Examples](#examples)
      - [collection.twig](#collectiontwig)
      - [JS:](#js-)
    + [Options](#options-2)
  * [productAddToBasketButton](#productaddtobasketbutton)
    + [Intro](#intro-3)
    + [Examples](#examples-1)
      - [JS](#js)
    + [Options](#options-3)
	
----

## productGallery ##
Toggle product view main image.

### Intro ###
Swap main product image when thumbnail preview is clicked

*Examples*

TWIG:
```twig
{# main image #}
<img src="{{ product.photo_url }}" alt="{{ product.photo_description }}" class="product-gallery-photo">

{# thumbnails #}
{% if product.photos|length > 1 %}
	<div class="product-thumbnail-slider product-gallery">
		{% for photo in product.photos %}
			<a href="{{ photo.url }}" class="product-gallery-thumbnail p{{ loop.index0 }}”>
				<img src="{{ photo.url }}" alt="{{ photo.description }}">
			</a>
		{% endfor %}
	</div>
{% endif %}
```
JS:
```javascript
$('.product-gallery').productGallery();
```

### Options ###
Name	| Default| Description
--- | --- | ---
`thumbnailSelector` | `.product-gallery-thumbnail`	| thumbnails container CSS selector
`photoSelector` | `.product-gallery-photo`	| Main image CSS selector


## imageContainer ##
Uniformed presentation of images with varied sizes and/or aspect ratios.

### Intro ###
Images should be placed inside a containing element. The container class is then bound to the plugin.

JS
```javascript
$(‘.image-container’).imageContainer();
```

Example HTML
```html
<div class=“image-container” data-fit=“1”>
	<img src=“foo.jpg” alt=“bar”>
</div>
```

### CSS ###
The plugin depends on the following CSS:
```css
  .image-container {
    position: relative;
    display: block;
    overflow: hidden;
    height: 0;
    padding-top: 100%;
  }
  
  .image-container img {
    position: absolute;
    left: 0;
    top: 0;
    width: auto;
    height: auto;
  }
```
> NOTE: the container’s padding (padding-top: 100%), combined with `height: 0` ensures containers are rendered as exact squares. In some circumstances, where a different size ratio is desired, the padding can be adjusted in the CSS. 

### Multiple sizes (aspect ratios) ###
We recommend sub classing the container element should a variety of container sizes be used.

Example:
```css
.image-container.letterbox {
  padding-top: 50%;
}
```

### Options ###
Options can be set through data attributes (on the container) to control framing of the inner image.

Data Attribute	| Default Type | Description
--- | --- | ---
`data-fit` | `1`| boolean fits image inside container if larger
`data-scale` | `0` | boolean scales image to fit inside container irrespective of size (data-fit must also be set to 1)
`data-padding` | `0` | integer (`px`) creates padding between container and image



## productCollection ##
Filter and paginate item collections

### Intro ###
Collections views include category and search result product listings. The productCollection plugin controls pagination and product filters (if active) via AJAX.

### Examples ###
Collection views normally inherit layout from the collection template `/views/templates/collection.twig`. This template contains the outer HTML, required for the plugin to work.

The containing element, by default has a className `collection-container`, this element also holds a data attribute `data-pages` which outputs the `{{ total_pages }}` twig variable.

All product filters, page links and products should be inside this container:

#### collection.twig ####
```twig
<div class="collection-container" data-pages="{{ total_pages|default(1) }}">
	{% include 'partials/product_filters.twig' with { 'sorting': true } %}
	{% block items %}{% endblock %}
</div>
```

The pagination markup may be found in a partial file; the name the file may differ between themes. 

The twig markup for the filter system should be added when installing the product filters extension.

#### JS: ####
```javascript
$('.collection-container').productCollection({
	// options
});
```
### Options ###
Name | Default | Type | Description
--- | --- | --- | ---
`pageLinkSelector` | `.page-link` |	string (CSS selector) | individual page links selector
`pageNumberSelector` | `.page-number` | string (CSS selector) | optional page number container selector
`paginationContainerSelector` | `.pagination-container` | 	string (CSS selector)	pagination links container
`sortOptionSelector` | `.sort-option` | string (CSS selector) | Sort form container selector
`filterSelector` | `.filter` | string (CSS selector) | individual filter checkbox selector
`filterGroupSelector` | `.filter-group` | string (CSS selector) | filter group selector
`filterContainerSelector` | `.filter-container` | string (CSS selector) | container selector for each individual filter checkbox selector
`filterCountSelector` | `.filter-count` | string (CSS selector) | optional - selector for displaying number of items next to each filter
`filterHiddenClass` | `''`  | string | className used to hide filter groups
`priceRangeSelector` | `.price-range` | 	string (CSS selector) | 	element containing filter price ranges
`containerSelector`	 | `.items` | string (CSS selector) | 	products list container selector
`pageLinkCurrentClass`	 | `current` | 	string	 | className used to denote current page
`sortOptionCurrentClass` | `current` | string | className used to denote selected sort option
`filterCurrentClass` | `current` | string | className used to denote selected filters
`priceRangeCurrentClass` | `current` | string | className used to denote selected price rage
`loadingClass` | `loading` | string | className temporally applied to items container after AJAX request is sent and before a response is received
`sortDropDownSelector` | `.sort-drop-down` | string (CSS selector) | 	select element used for changing item sort order
`resetButtonSelector`	 | `.reset-button`	 | string (CSS selector)	 | optional reset button for unchecking filters in the context of a filter group
`resetAllButtonSelector` | 	`.reset-all-button`	 | string (CSS selector) | 	optional reset button for resetting ALL filters
`itemsPerPageDropDownSelector`	 | `.items-per-page-drop-down` | 	string (CSS selector) | 	optional - className for select element for user to choose number of items per page
`ajaxValue` | 	`partials/products` | string | view containing twig markup for the collection items.
`adjacentPageLinks` | `4` | integer | truncate X number of page links
`infinite` | `false` | boolean | set `true` for next items to load below current list (instead of switching to the next page)
`afterLoad` | `false` | boolean | run a function before AJAX request
`beforeLoad` | `false` | boolean | run a function after AJAX response
`itemsPerRequest` | `10` | integer | Number of items loaded per request (when infinite == true)
`itemsToRefresh` | `10` | integer | number of items to refresh (when infinite == true)
`initializeImageContainers` | `true` | boolean | renders elements that require the imageContainer() plugin.


## productAddToBasketButton ##
The soft add to basket feature must be enabled for the plugin to work.

### Intro ###
The button prevents shoppers from being redirected to the checkout page immediately after adding a product to their basket. Instead, a modal box will appear confirming the basket contents (including the item just added).

### Examples ###

`product.twig`
When the feature is enabled a className should be added to the `add to basket` button.
```twig
	<button class="btn button-add-to-basket full-sm add-button {{ global.features.ajax_basket ? 'product-soft-add-button' : '' }}" name="cart_button”>…</button>
```


#### JS ####
```javascript
	$('.product-soft-add-button').productAddToBasketButton({
		// options
	});
```

### Options ###
Name | Default | Description
--- | --- | ---
`view` | `partials/basket_items` | view containing twig markup that displays the items list inside the modal box
`totalSelector` | `.shopwired-basket-total-value` | optional - CSS selector for the value of the shopping basket. 
`modalSelector` | `.shopwired-basket-modal` | selector for hidden modal box
`error` | `function` | By default error dialogue is displayed as a simple javascript alert. To change this behaviour, create your own function. The argument for message text is `message`.

## All plugins 

Plugin | Function | Description
--- | --- | ---
`grid` | Grid | Arranges items into a grid layout
`productVideoButton` | Watch video button | Displays video when clicked.
`productSortDropDown` | Sort drop down | Used to sort product listings
`quantityBox` | Quantity box | Plus / minus toggle for input containing integer
`addressForm` | Address form | Used to show/hide checkout address section and copy field values
`listWithDropDown` | List with drop down | Expandable menu
`customCheckBox` | Custom check box | For styling of checkbox inputs
`imageZoomer` | Image zoomer | Roll over image zoom
`productGallery` | Gallery thumbnails | click image thimbnails to show as main image
`productFilter` | Product filter | Filters product collections
`imageContainer` | Image container | Uniforms the appearance of product images
`ajaxForm` | AJAX form | Submit form without reloading page
`linkWithConfirmation` | Link with confirmation | Helper for js confirm() when link is clicked
`checkoutAddressForm` | Checkout address form | For checkout form
`formWithValidation` | Form with validation | Inloine form validation
`infoMessage` | Info message | displays popup banner notifications
`basketDeliveryForm` | Basket delivery form | Basket page
`productForm` | Product form | For product page form
`productCollection` | Product collection | supports filters and pagination
`productAddToBasketButton` | Product add to basket button | Soft add to basket
`toggleButton` | Toggle button | toggleButton
`stateDropDown` | State drop down | outpus select options for US states when country select is changed
`productFinder` | Product finder | queries '/search/product-finder-values' to retrive products when option selected
