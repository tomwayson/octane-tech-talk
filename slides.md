# Ember Octane & Hub

3/14/20



## Octane?

🤷‍



<img src="https://pbs.twimg.com/profile_images/861010112852262912/nbPZKMyR_400x400.jpg">



### Features
- [Native Classes](https://guides.emberjs.com/release/upgrading/current-edition/native-classes/)
- [Glimmer components](https://guides.emberjs.com/release/upgrading/current-edition/glimmer-components)
- [Angle Bracket Syntax](https://guides.emberjs.com/release/upgrading/current-edition/templates/#toc_benefits-of-angle-brackets)
- [Autotracking](https://guides.emberjs.com/release/in-depth-topics/autotracking-in-depth/)
- [Modifiers](https://guides.emberjs.com/release/components/template-lifecycle-dom-and-modifiers/#toc_event-handlers)
- [Decorators](https://guides.emberjs.com/release/getting-started/working-with-html-css-and-javascript/#toc_decorators)



#### What < Why ~~How?~~ <!-- .element: class="fragment" -->




> Octane is Ember's new component model and reactivity system, which is more modern, easier to use, and just more fun



<img src="https://emberjs.com/images/tomsters/octane.png">



## Developer ergonomics

<small>(read: productivity)</small> <!-- .element: class="fragment" -->



## Octane does NOT
- fix what ain't broke (router) 👍 <!-- .element: class="fragment" -->
- modernize the dated build system 😦 <!-- .element: class="fragment" -->
- fix JSAPI incompatibility w/ the loader 😞 <!-- .element: class="fragment" -->



🔥 "Octane makes Ember _feel_ modern" - Tom, 2019



<!-- .slide: data-background="#fff" -->

<img src="https://inspirationalperspective.com/wp-content/uploads/2014/03/BeingCalledaHater.png" width="400" style="box-shadow: none">



## Innovative Ideas
- Autotracking reactivity model
- Element modifiers



## Philosophy: HTML First
- "it's just HTML and CSS" <!-- .element: class="fragment" -->
- except when you need variables, conditionals, loops, etc 😉 <!-- .element: class="fragment" -->
- start w/ template; only add backing JS as needed <!-- .element: class="fragment" -->



### WHen you do need JS

"it's just JS"



### Native Classes

```js
// Before
import Controller from '@ember/controller';

export default Controller.extend({
  title: 'hello-world.app', // property
});
```
```js
// After
import Controller from '@ember/controller';

export default class ApplicationController extends Controller {
  title = 'hello-world.app'; // class field
}
```



No `init()` for Objects & Arrays!!!

```js
// Before
import Controller from '@ember/controller';

export default Controller.extend({
  init () {
    this.set('featureFlags', { test: true });
  }
});
```
```js
// After
import Controller from '@ember/controller';

export default class ApplicationController extends Controller {
  featureFlags = { test: true }; // this is OK now!!!
}
```



## Autotracking

```js
import Service from '@ember/service';
import { tracked } from '@glimmer/tracking';

export default class Session extends Service {
  @tracked username = '';

  get isAuthenticated () {
    // CPs are now just getters
    return !!this.username;
  }

  logIn (username) {
    // no more this.set()!!!!
    this.username = username;
  }
};
```



#### No. More. Checking. `isDestroyed`!!!



### Autotracking vs React State vs RxJS

https://youtu.be/HDBSU2HCLbU

TLDW: delivers best dev ergonomics & perf



## More explicit code

Less of this confusing ✨magic✨:

```hbs
{{something}} {{!-- variable? helper? --}}
{{!-- component? where are these values defined --}}
{{what-am-i fromMe=possibly fromParent=maybe}}
```



#### Variables

Use `@namedArguments` and `this.properties`

```hbs
<p>From me: {{this.someProperty}}</p>
<p>From parent: {{@someArgument}}</p>
```



#### Components

Use angle bracket invocation

```hbs
<MyComponent
  @anArgument="can be a literal, or..."
  @fromMe={{this.someProperty}}
  @fromParent=@forSure
  class="blue {{this.computedClasses}}"
  aria-role="main"
  data-test-id="mosDef"
/>
```



#### Clarity in Component JS too

```js
export default class MyComponent extends Component {
  get derivedProperty() {
    // it's clear what belongs to the component or the parent
    return this.someProperty + this.args.someArgument;
  }
}
```



## ...attributes

Let the HTML flow

```hbs
<MyComponent class="foo-component" data-foo="foo" data-bar="foo" />

<!-- my-component template -->
<p class="some-class" data-foo="bar" ...attributes data-bar="baz"></p>

<!-- output -->
<p class="some-class foo-component" data-foo="foo" data-bar="baz"></p>
```



## Glimmer Components

- Tagless
- Less API surface
- Use `@action` instead of `actions`



### Less API = Less JS

- no `tagName`, `classNames`, `attributeBindings`
- don't use lifecycle methods

All that moves to the template




## Modifiers

Add behavior to _any_ element



### Modifiers replace

- Ember events (`click={{action 'someAction}}`)
- action helper `{{action 'someAction}}`
- lifecycle hooks (`didInsertElement`)
- more



### `{{on}}` Helper

Given an `@action add(num = 1)`

```hbs
<button {{on "click" this.add}}> 
    Add 1
</button>

<button {{on "click" (fn this.add 5)}}>
  Add 5
</button>
```


### Custom Elements too!

```hbs
<calcite-slider {{on "sliderUpdated" this.onSliderUpdate}} />
```



### did-insert

```hbs
<div did-insert={{this.resize}}>
  ...
</div>
```



### Custom Modifiers

```hbs
<div {{draggable}}>Drag me!</div>
<button {{tooltip "About me"}}>Click me!</button>
<div {{webmap @itemId this.center this.zoom}}>
```



### More Granular Composition

```hbs
<div {{draggable}} {{tooltip "About me"}}>Drag me!</div>
```



## Octane in the Hub

When to go Octane?



## Anything net-new
- Native classes & `@tracked` for all new services, routes, controllers
- All new components should be Glimmer components



## Update existing stuff?

don't spend time on this... yet



### HTML First, an Opportunity
- Designers could write markdown and styles in storybook
- easy for developers to add interactivity (JS) later

