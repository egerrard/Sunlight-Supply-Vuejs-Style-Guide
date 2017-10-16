# Vue Docs

### Created properties: camel case

Good: 

`self.thing = null;`

Bad: 
	
`self.Thing = bad;`

### Model properties: keep model properties capital as they come over from C#

Good: 
	
`data.Part.Description = 'Good';`

Bad:
	
`data.Part.description = 'bad';`

### Method and computed names: camel case

Good:
	
`methodToDoStuff: function() {}`

Bad:
	
`MethodToDoStuff: function() {}`

### Use v-show over v-if where possible

Good:

`<div v-show="showIfTrue">`

Bad:

`<div v-if="showIfTrue">`

It is ok to use `v-if` for show/hide functionality if you do not want to load what is inside if false

Good:

`<dont-show v-if="showIfTrue">This won't be loaded in DOM.</dont-show>`

### Components: 
Do these.  Where ever it makes sense no matter how small any code that is used more than once we would like to have components for it.  

##### Javascript:
* Create a component JavaScript file inside a 'Component' folder inside the vue folder you are working in.
* Declare defaults for all props.

```javascript
Vue.component('example',
{
    props: {
        example: {
            type: String,
            default: function () { return 'default to this' }
        }
	}
}
```
    
* Use emits to contact parent components (do not use camel casing here.  It screws stuff up):

```javascript    
    contactParent: function () {
            this.$emit('contact-parent');
    }
```

##### Template:
* Create partial view for the `<template>` tag and include all of your html.  At the bottom of this page out side of your `<template>` tag reference the component's JavaScript file.

### Gotchas
* YOU MUST declare all properties you want in Vue before binding to a vue property.  Example:
    
Good
    
```javascript
    returnedFromServer.SomeValue = false;
    this.VueData = returnedFromServer;
    //now I can reference the value
    this.VueData.SomeValue = true;
```
    
BAD BAD
    
```javascript
   
    this.VueData = returnedFromServer;
    this.VueData.SomeValue = true;
    // you'll never see that ^  update.  You blew it.
```

* DO NOT USE camel case when using the emit functions on components. Camel case will break the emit function.  Instead use kebab case in JavaScript and html.

```javascript
	contactParent: function () {
            this.$emit('this-must-not-be-camel-case');
    }
```
    
```html
    <example v-on:this-must-not-be-camel-case="doStuff"></example>
```

* DO NOT USE camel case in html when passing a [prop to components](https://vuejs.org/v2/guide/components.html#camelCase-vs-kebab-case).  Use Camel case in JavaScript and kebab in the html.

Good

```html
    <example prop-wanted-in-this-component="iCanUseCamelCaseHere"></example>
```
```javascript
    props: {
            propWantedInThisComponent: {
                type: String,
                default: function() { return 'Good job not using kebab in here' }
            }
        },
```
   
Bad, will break in html

```html
    <example propWantedInThisComponent="iCanUseCamelCaseHere"></example>
```

Bad, will break in JavaScript

```javascript
    props: {
            prop-wanted-in-this-component: {
                type: String,
                default: function() { return 'Bad job, you blew it' }
            }
        },
```