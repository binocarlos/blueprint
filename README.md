blueprint
=========

STATUS: VAPOUR - I'm still copying code over

Angular directives to display forms based on an array of field descriptors (blueprints)

Useful for different pages that edit the same data using different forms - composition of blueprints over inheritance!

## example

Here is an angular directive that will render a form for a model using am ad-hoc blueprint.

The controller (where we define our blueprint and grab the model):

```js
// blueprint fields for a 'product'
$scope.product_blueprint = [{
	name:'name'
}, {
	name:'price',
	type:'number'
}, {
	name:'desc',
	title:'Product Notes',
	type:'textarea'
}]

// the object we will edit using the blueprint form
$scope.product_model = {
	name:'Table',
	price:12,
	desc:'Oak table'
}

// our custom form handler logic - blueprint does not get involved here
$scope.saveform = function(){
	// $scope.product_model is the edited model
}
```

The template (where we wrap the blueprint directive around the context for it):

```html
<h3>Edit table details</h3>
<!--
	render the product form in bootstrap markup
 -->
<blueprint markup="bootstrap" fields="product_blueprint" model="product_model" />
<button ng-click="saveform()">Save</button>
```

## installation

blueprint is a [component](https://github.com/component/component)

```
$ component install binocarlos/blueprint
```

You can also download the [standalone version](https://raw.github.com/binocarlos/blueprint/master/dist/bundle.js) to include in your page.

## usage

### blueprint directive
The main api is via the blueprint directive.  This accepts the following properties:

 * model [Object] - the object that provides values to the form to be edited
 * fields [Array] - a javascript array of field descriptors
 * readonly [Boolean] - if we are just displaying the model
 * blueprint [String] - get the fields from a pre-registered blueprint
 * markup [String] - describes which markup templates to use - currently plain | bootstrap

### markup
Blueprint keeps its views in markup collections which can be switched.

The markup decides what HTML is used to wrap each field.

There are 2 markup flavours currently:

 * plain (default) - this is just standard HTML divs
 * bootstrap - uses bootstrap 3 markup - requires bootstrap.css to be on the page

### model
This is the object that will be edited via the blueprint form.

New values will be created for dot notation paths.

For example the name 'address.street' would create:

```js
var model = {
	address:{
		street:''
	}
}
```

Being angular - as the user changes values they will change in the model.

### fields
The fields property is an array of objects each describing a field in the form.

Each field object has the following options:

 * name [String] - the field name that is edited in the model ( is ok = {address:{street:''}})
 * title [String] - the text to display for the field (created from the name as default)
 * type [String] - the type of the field (more below)
 * required [Boolean] - if the user must provide a value for the field
 * list [Boolean] - if the field is a list of values
 * options [Array] - an array of options for radio and select fields
 * template_name [String] - the name of a registered template (more below)
 * template [String] - the HTML to use for the field

### type
The 'type' property dictates what is displayed to the user.  These are core types that blueprint provides:

 * text
 * textarea
 * radio
 * select
 * checkbox
 * url
 * email
 * number
 * template - see templates below

### templates
The page can register templates with blueprint.

### registering
Blueprints can be registered for later use
### templates
Templates can be registered with blueprint so that 