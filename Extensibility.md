## Targets

### How it works

(Using) 
	interceptor pattern 
(to) 
	modify core behavior 
(by) 
	modifying core code output during build time

()
	The point 
(that)
	you can intercept the normal logic 
(is)
	Targets

()
	targets
(are)
	variants of a JavaScript pattern 
(called)
	Tappable hook

()
	targets 
(share)
	 some functionality 
(with)
	NodeJS's EventEmitter class

## TargetProviders

()
	The TargetProvider
(is)
	an object that 
(provides)
	public API to
(create)
	targets 
(and intercepting)
	targets from other extensions

(For example)
	you can 
(use)
	routes Targets
(to)
	add a new route
((In this case))
	you create an intercept.js file
(in)
	use "routes" target to add a new route to your storefront

code

```js

// intercept.js
module.exports = targets => {
	const veniaTargets = targets.of('@magento/venia-ui');
	const routes = veniaTargets.routes

	routes.tap(
		routesArray => {
			routesArray.push({
				name: 'New route',
				pattern: '/new-route',
				path: '@organization/module'
			});
			return routesArray;
		}
	)
}

// package.json
{
	"pwa-studio":{
		"targets": {
			"intercept": "intercept.js"
		}
	}
}

```

## Targetables

Another method 
= to customize PWA Studio 
is
= using targetable
which 
= are object which represent the source files in your projects

Thanks to
= targetables your can
for example
= get a specific component
and
= insert JSX wherever you want

Example

``` js

const { Targetables } = require('@magento/pwa-buildpack')

module.exports = targets => {

	const targetables = Targetables.using(targets);

	const MainComponent = targetables.reactComponent(
		`@magento/venia-ui/lib/components/Header/header.js`
	);

	MainComponent.insertAfterJSX('<MegaMenu/>', '<div>My new section!!</div>')

}

```

On the other hand
= if you create your own PWA Studio extension you
can
= use Targetables 
to 
= add specific targets
for 
= other modules

## How to customize PWA Studio

Typically if
= you deal with PWA Studio you
create
= a new Storefront 
or 
= a new extension

### Creating a new storefront

The first option
= to extend PWA Studio with new features 
is by
= creating a new storefront
!
Venia concept
= can be your starting point, but it's not obligatory.
Typically
= you will start from the Venia concept 
because
= it has many excellent features already implemented
! !
In this case
= you start by scaffolding your project
using
= the `yarn create @magento/pwa` command
! ! !
Then
= you have the opportunity to add customizations

First
= scaffold the PWA Studio project

Second
= add this code to the `local-intercept.js` file

``` js


const { Targetables } = require('@magento/pwa-buildpack');

module.exports = targets => {
	const targetables = Targetables.using(targets);

	const ProductFullDetailComponent = targetables,reactComponent(
		'@magento/venia-ui/lib/components/ProductFullDetail/productFullDetail.js'
	);

	ProductFullDetailComponent.insertBeforeJSX('<Button type="submit" />', '<span>Hello World!</span>');
}

```

This code
= inserts a span with Hello World! 
Before
= submit button of the add to cart form

### Targetables public API

In the example above
= i used `insertBeforeJSX` method
of
= `@magento/pwa-buildpack/lib/WebpackTools/targetables/TargetableReactComponent.js`

There are
= a few more public methods available 
that 
= help you with modifying the PWA Studio storefront

- insertAfterSX 
	- Inserting a JSX element _after_ `element`
- addJSXClassName 
	- Adding a CSS classname to a [[JSX]] `element`
- addReactLazyImport
	- Add a new named dynamic import of another React component
- appendJSX
	- Appending a JSX element to the children of `element`
- prependJSX
	- Prepending a JSX element to the children of `element`
- removeJSX
	- Removing the JSX node matched by `element`
- removeJSXProps
	- Removing JSX props from the element if they match one of the lists of names
- replaceJSX
	- Replacing a JSX element with a different code
- setJSXProps 
	- Setting JSX props on a JSX element
- surroundJSX
	- wrapping a JSX element in an outer element

## Creating a new extension

Another way 
= to customize PWA Studio
is by
= creating a new extension
!
For example
= if you had wanted 
to 
= add integration with any headless CMS systems
you would
= have to create a new extension

Everyone who
= wants to 
use
= your extension
can
= install it and use in their project
!
This also means that
= any extension can be
extended
= in storefronts where it's used

As
= an extension creator
you can
= use Targetables in your intercept file
to add
= specific Targets that are
available to
= other extensions

## Styling PWA Studio

The common thing that
= frontend developers 
do
= is styling
!
In terms if
= you want 
to
= customize styles of the Storefront
you need to
= add your custom styles somewhere

Thanks to Targetables
= styling is much more simplified than before

[https://dev.to/chrisbrabender/simplifying-styling-in-pwa-studio-1ki1](https://dev.to/chrisbrabender/simplifying-styling-in-pwa-studio-1ki1)

