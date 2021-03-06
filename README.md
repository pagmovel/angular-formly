## Formly
Formly for Angular is an AngularJS module which has directives to help customize and render JSON based forms. The directive originated from a need to allow our users to create surveys and distribute them easily. Currently we've can render the form data from JSON and assign a model to form so we can receive the submitted data.

	<formly-form result="formData" fields="formFields" options="formOptions" ng-submit="onSubmit()">
	</formly-form>

### Demo : http://Nimbly.github.io/angular-formly/

## Dependencies
- Required to use Formly:
 - Angular

- Dev dependencies to build Formly
 - npm


See `bower.json` and `index.html` in the `master` branch for a full list / more details

## Install in your project
- Install with Bower  
 `$ bower install angular-formly --save`

- Include the javascript file in your index.html, Formly comes in the following flavors:
 - Vanilla: no fancy styling, just plain html

 `<script src="bower_components/angular-formly/dist/formly.min.js"></script>`

 - Bootstrap: bootstrap compatible forms, form-groups, etc.

 `<script src="bower_components/angular-formly/dist/formly.bootstrap.min.js"></script>`

 - DIY: You can create your own templates with `formlyTemplateProvider`! Use any of the builds above and override all the templates or just the ones you need.

- Add 'formly' as a required module to your angular app, usually in `app.js`:  
 `var app = angular.module('app', ['ng', 'ui.router', 'formly']);`

## Documentation

You can add a formly-form in your HTML templates as shown below.
```html
	<formly-form result="formData" fields="formFields" options="formOptions" ng-submit="onSubmit()">
	</formly-form>
```  

Example data as it would be set in the controller
```javascript
	$scope.formData = {};
	$scope.formFields = [
		{
			//the key to be used in the result values {... "username": "johndoe" ... }
			key: 'username',

			//default value
			default: 'uberuser',
			type: 'text',
			label: 'Username',
			placeholder: 'johndoe',
			required: true,
			disabled: false //default: false
		},
		{
			key: 'password',
			type: 'password',
			label: 'Password',
			required: true,
			disabled: false, //default: false
			hideExpression: '!username' // hide when username is blank
		}

	];

	$scope.formOptions = {

		//Set the id of the form
		uniqueFormId: 'myFormId',

		//Hide the submit button that is added automaticaly
		//default: false
		hideSubmit: false,

		//Set the text on the default submit button
		//default: Submit
		submitCopy: 'Login'
	};

	$scope.onSubmit = function() {
		console.log('form submitted:', $scope.formData);
	};
```
### Creating Forms
Forms can be customized with the options below. Note, you can configure this on a form-by-form basis as shown in the example, or globally using the [`formlyOptionsProvider`](#global-config).

#### uniqueFormId (string, required)
>`uniqueFormId` is used to identify the form.

#### hideSubmit (boolean, optional)
>`hideSubmit` hides the submit button when set to `true`. Defaults to false.

#### submitCopy (string, optional)
>`submitCopy` customizes the submit button copy. Defaults to 'Submit'.

#### submitButtonTemplate (string, optional)
>`submitButtonTemplate` customizes the template used for the submit button. Compiled on the scope, so you have access to all other options (and any custom options) in your custom template.

### Creating Form Fields
When constructing fields use the options below to customize each field object. You must set at least a `type`, `template`, or `templateUrl`.

##### type (string)
>`type` is the type of field to be rendered. Either type, template, or templateUrl must be set.

###### Default
>`null`

###### Values
> [`text`](#text-form-field),
> [`textarea`](#textarea-form-field),
> [`radio`](#radio-form-field)
> [`select`](#select-form-field)
> [`number`](#number-form-field)
> [`checkbox`](#checkbox-form-field),
> [`password`](#password-form-field),
> [`hidden`](#hidden-form-field),
> [`email`](#email-form-field)

---
##### template (string)
>`template` can be set instead of `type` or `templateUrl` to use a custom html template form field. Should be used with one-liners mostly (like a directive). Useful for adding functionality to fields.

###### Default
>`undefined`

---
##### templateUrl (string)
>`templateUrl` can be set instead of `type` or `template` to use a custom html template form field. Set a path relative to the root of the application. ie `directives/custom-field.html`

###### Default
>`undefined`

---
##### key (string)
>By default form results are keyed by location in the form array, you can override this by specifying a `key`. 

###### Default
>`undefined`

---
##### label (string)
>`label` is used to add an html label to each field.

###### Default
>A default is set for each field based on its type. ie `Text`, `Checkbox`, `Password`

---
##### required (boolean)
>`required` is used to add the required attribute to a form field.

###### Default
>`undefined`

---
##### hideExpression (expression string)
>`hideExpression` is used to conditionally show the input. Evaluates on the `result` and uses the `hide` property on the field.

###### Default
>`undefined`

---
##### hide (boolean)
>`hide` is used to conditionally show the input. When true, the input is hidden (meant to be used with a watch).

###### Default
>`undefined`

---
##### disabled (boolean)
>`disabled` is used to add the disabled attribute to a form field.

###### Default
>`undefined`

---
##### placeholder (string)
>`placeholder` is used to add placeholder text to some inputs.

###### Default
>`undefined`

---
##### watch.expression (object)
>`watch` has two properties called `expression` and `listener`. The `watch.expression` is added to the formly directive's scope. If it's a function, it will be wrapped and called with the field as the first argument, followed by the normal arguments for a watcher. The `listener` will also be wrapped and called with the field as the first argument, followed by hte normal arguments for a watch listener.

For example:

```javascript
// normal watcher
$scope.$watch(function expression(theScope) {}, function listener(newValue, oldValue, theScope) {});

// field watcher
$scope.$watch(function expression(field, theScope) {}, function listener(field, newValue, oldValue, theScope) {});
```

###### Default
>`undefined`

### Form Fields
Below is a detailed description of each form fields and its custom properties.

#### Text form field
>The text field allows single line input with a input element set to `type='text'`. It doesn't have any custom properties.

##### default (string, optional)

_Example text field_
```json
	{
		"type": "text",
		"key": "firstName",
		"placeholder": "jane doe",
		"label": "First name"
	}
```

---
#### Textarea form field
>The textarea field creates multiline input with a textarea element.

##### default (string, optional)

##### lines (number, optional)
>`lines` sets the rows attribute for the textarea element. If unset, the default is 2 lines.

_Example textarea field_
```json
	{
		"type": "textarea",
		"key": "about",
		"placeholder": "I like puppies",
		"label": "Tell me about yourself",
		"lines": 4
	}
```

---
#### Checkbox form field
>The checkbox field allows checkbox input with a input element set to `type='checkbox'`. It doesn't have any custom properties.

##### default (boolean, optional)

_Example checkbox field_
```json
	{
		"type": "checkbox",
		"key": "checkThis",
		"label": "Check this box",
		"default": true
	}
```

---
#### Radio form field
>The radio field allows multiple choice input with a series of linked inputs, with `type='radio'`.

##### default (string, optional) 
>The default can be set to the `value` of one of the `options`.

##### options (array, required)
>`options` is an array of options for the radio form field to display. Each option should be an object with a `name`(string) and `value`(string or number).

_Example radio field_
```json
	{
		"key": "triedEmber",
		"type": "radio",
		"label": "Have you tried EmberJs yet?",
		"default": "no",
		"options": [
			{
				"name": "Yes, and I love it!",
				"value": "yesyes"
			},
			{
				"name": "Yes, but I'm not a fan...",
				"value": "yesno"
			},
			{
				"name": "Nope",
				"value": "no"
			}
		]
	}
```

---
#### Select form field
>The select field allows selection via dropdown using the select element.

##### default (number, optional)
>The default can be set to the index of one of the `options`.

##### options (array, required)
>`options` is an array of options for the select form field to display. Each option should be an object with a `name`(string). You may optionally add a `group` to some or all of your options.

_Example select field_
```json
	{
		"key": "transportation",
		"type": "select",
		"label": "How do you get around in the city",
		"options": [
			{
				"name": "Car"
			},
			{
				"name": "Helicopter"
			},
			{
				"name": "Sport Utility Vehicle"
			},
			{
				"name": "Bicycle",
				"group": "low emissions"
			},
			{
				"name": "Skateboard",
				"group": "low emissions"
			},
			{
				"name": "Walk",
				"group": "low emissions"
			},
			{
				"name": "Bus",
				"group": "low emissions"
			},
			{
				"name": "Scooter",
				"group": "low emissions"
			},
			{
				"name": "Train",
				"group": "low emissions"
			},
			{
				"name": "Hot Air Baloon",
				"group": "low emissions"
			}
		]
	}
```

---
#### Number form field
>The number field allows input that is restricted to numbers. Browsers also provide minimal ui to increase and decrease the current value.

##### default (number, optional)

##### min (number, optional)
>`min` sets minimum acceptable value for the input.

##### max (number, optional)
>`max` sets maximum acceptable value for the input.

##### minlength (number, optional)
>`minlength` sets minimum number of characters for the input. If a number less than this value it will not be submitted with the form. eg 1000 is 4 characters long and if `minlength` is set to 5, it would not be sent. Currently there is no error displayed to the user if they do not meet the requirement.

##### maxlength (number, optional)
>`maxlength` sets maximum number of characters for the input. If a number is greater than this value it will not be submitted with the form. eg 1000 is 4 characters long and if `maxlength` is set to 2, it would not be sent. Currently there is no error displayed to the user if they do not meet the requirement.

_Example number field_
```json
	{
		"key": "love",
		"type": "number",
		"label": "How much love?",
		"default": 2,
		"min": 0,
		"max": 100,
		"required": true
	}
```

---
#### Password form field
>The password field allows password input, it uses an input with `type='password'`.
##### default (string, optional)

_Example password field_
```json
	{
		"key": "password",
		"type": "password",
		"label": "Password"
	}
```

---
#### Hidden form field
>The hidden field allows hidden input, it uses an input with `type='hidden'`.

##### default (number or string, required)

_Example password field_
```json
	{
		"key": "hiddenCode",
		"type": "hidden",
		"default": "hidden_code"
	}
```

---
#### Email form field
>The email field allows email input, it uses an input with `type='email'`. Browsers will provide basic email address validation by default.

##### default (string, optional)

_Example password field_
```json
	{
		"key": "email",
		"type": "email",
		"placeholder": "janedoe@gmail.com"
	}
```

## Other Notes

### Global Config

#### formlyTemplateProvider

You can configure formly to use custom templates for specified types (your own "text" template) by injecting the `formlyTemplateProvider` in your app's `config` function. The `formlyTemplateProvider` has the following functions:

##### setTemplateUrl

Allows you to set a template

```javascript
formlyTemplateProvider.setTemplateUrl('radio', 'views/custom-formly-radio.html');
formlyTemplateProvider.setTemplateUrl('checkbox', 'views/custom-formly-checkbox.html');

// the same can be accomplished with

formlyTemplateProvider.setTemplate({
	radio: 'views/custom-formly-radio.html',
	checkbox: 'views/custom-formly-checkbox.html'
});
```

##### getTemplateUrl

Allows you to get the template

```javascript
formlyTemplateProvider.setTemplateUrl('radio', 'views/custom-formly-radio.html');
formlyTemplateProvider.getTemplateUrl('radio') === 'views/custom-formly-radio.html'; // true
```

#### formlyOptionsProvider

You can configure default options for all forms using the `formlyOptionsProvider` in your app's `config` function. This has the following api:

##### setOption

Allows you to set an option

```javascript
formlyOptionsProvider.setOption('uniqueFormId', 'probablyDontWantToDoThis');
formlyOptionsProvider.setOption('submitCopy', 'Save');

// the same can be accomplished with

formlyOptionsProvider.setOption({
	submitCopy: 'Save',
	uniqueFormId: 'probablyDontWantToDoThis'
});
```

##### getOptions

Returns a copy of the current options. This is used internally.

## Roadmap

## Release Notes

## Development

1. `git checkout master`
	1. run `npm install && bower install`
	2. test your code using `grunt dev` which hosts the app at `http://localhost:4000`
	3. commit your changes
3. update README, CHANGELOG, bower.json, and do any other final polishing to prepare for publishing
	1. git commit changes

## Grunt targets
* `grunt dev`: Creates a server for testing at `http://0.0.0.0:4000`
* `grunt publish`: Copies the src folder and bower_components to gh-pages
