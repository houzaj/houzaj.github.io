---
layout: post
title: 'Vue.js Guide Notebook - Essential'
date: 2019-03-07
author: HouZAJ
cover: 'https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190310%20VueJSNote_Essential/20190310-01.png'
tags: VueJS
---

> Vue.js Guide Notebook - Essential  

<br>

<iframe type="text/html" src="http://music.163.com/outchain/player?type=2&id=401722084&auto=0&height=66" frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86"></iframe>      

<br>

* CATAlOGY  
{:toc}  

<br>

### BB
HI EVERYONE!  
In order to avoid ambiguity, I try my best to write this article in English. \( Nevertheless, I'm poor in English QAQ  
This article is concerning the essential part of `Vue.js`, and most of the content is from official guide(documentation). Of course, I may modify the samples in some part.   
Additionally, the note of this series may **NOT** be updated, because I just want to use some nice UI library based on Vue.js.  
<br>

### Introduction
#### Text Interpolation
```html
<body>
    <div id="app">
        {{ message }}
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                message: 'Hello Vue!'
            }
        })
    </script>
</body>
```
<br>

#### Bind Element Attributes
e.g. Keep this element’s `title` attribute up-to-date with the `message` property on the Vue instance.
```html
<body>
    <div id="app">
        <span v-bind:title="message">
            Hover it!
        </span>
    </div>
    <script type="text/html">
        var app = new Vue({
            el: '#app',
            data: {
                message: 'Hello Vue!'
            }
        })
    </script>
</body>
```
<br>

#### Conditionals
```html
<body>
    <div id="app">
        <span v-if="ok">
            Surprise!
        </span>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                ok: true
            }
        })
    </script>
</body>
```
<br>

#### Loops
```html
<body>
    <div id="app">
        <p v-for="ele in todos">
            {{ ele.text }}
        </p>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                todos: [
                    { text: 'A' },
                    { text: 'B' },
                    { text: 'C' }
                ]
            }
        })
    </script>
</body>
```
<br>

#### Event Listeners
`v-on`: add an event listener  
```html
<body>
    <div id="app">
        <p> {{ message }} </p>
        <button v-on:click="funReverse"> Click Here! </button>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                message: 'This is a message!'
            },
            methods: {
                funReverse: function() {
                    this.message = this.message.split('').reverse().join('')
                }
            }
        })
    </script>
</body>
```
<br>

#### Two-way Binding
`v-model`: two-way bindings  
```html
<body>
    <div id="app">
        <p> {{ message }} </p>
        <input v-model="message">
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                message: 'This is a message!'
            }
        })
    </script>
</body>
```
<br>

#### Component
```html
<body>
    <div id="app">
        <ol>
            <template-todo
                v-for="item in ls"
                v-bind:todo="item"
                v-bind:key="item.id"
            ></template-todo>
        </ol>
    </div>
    <script type="text/javascript">
        Vue.component('template-todo', {
            props: ['todo'],
            template: '<li>{{ todo.text }}</li>'
        })

        var app = new Vue({
            el: '#app',
            data: {
                ls: [
                    { id: 0, text: 'A' },
                    { id: 1, text: 'B' },
                    { id: 2, text: 'C' }
                ]
            }
        })
    </script>
</body>
```
<br>

### Template Syntax
#### Once-time Text Interpolation
`v-once`: perform one-time interpolations  
```html
<body>
    <div id="app">
        <p v-once>{{ msg }}</p>
    </div>
    <script type="text/javascript">

        var app = new Vue({
            el: '#app',
            data: {
                msg: 'Hello Vue!'
            }
        })
    </script>
</body>
```
<br>

#### Raw HTML
```html
<body>
    <div id="app">
        <p>{{ rawhtml }}</p>
        <p><span v-html="rawhtml"></span></p>
    </div>
    <script type="text/javascript">

        var app = new Vue({
            el: '#app',
            data: {
                rawhtml: "<span style='color:red'>RED</span>"
            }
        })
    </script>
</body>
```
<br>

#### HTML Attributes
`v-bind`: binding html attributes  
```html
<body>
    <div id="app">
        <button v-bind:disabled="ok">Button</button>
    </div>
    <script type="text/javascript">

        var app = new Vue({
            el: '#app',
            data: {
                ok: true
            }
        })
    </script>
</body>
```
<br>

#### Directives
**Directives**: Special attributes with the `v-` prefix. e.g. `v-if`  
**Argument**: Some directives can take an “argument”, denoted by a colon after the directive name. e.g. `v-bind:title="msg"`  
**Dynamic Arguments**: New in 2.6.0+! e.g. `<a v-on:[eventName]="doSomething"> ... </a>`, in which `eventName` is dynamic.  
**Modifiers**: Special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. e.g. `<form v-on:submit.prevent="onSubmit"> ... </form>`  
<br>

#### Shorthands
```html
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>


<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>
```
<br>

### Computed Properties and Watchers
#### Computed Properties
Computed properties are cached based on their dependencies.  
```html
<body>
    <div id="app">
        <p>Orgin: {{ msg }}</p>
        <p>Reversed: {{ revMsg }}</p>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                msg: "Hello World!"
            },
			// Computed Properties (getter)
            computed: {
                revMsg: function () {
                    return this.msg.split('').reverse().join('')
                }
            }
        })
    </script>
</body>
```
<br>
```html
<body>
    <div id="app">
        <p>Name: {{ fullName }}</p>
        <p>FirstName: {{ firstName }}</p>
        <p>LastName: {{ lastName }}</p>
        <button @click="tfun">Click here to Change Full Name</button>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                firstName: "first",
                lastName: "last"
            },
            computed: {
                fullName: {
					// getter
                    get: function () {
                        return this.firstName + ' ' + this.lastName
                    },
					// setter
                    set: function (nv) {
                        var names = nv.split(' ')
                        this.firstName = names[0]
                        this.lastName = names[names.length - 1]
                    }
                }
            },
            methods: {
                tfun: function () {
                    this.fullName = "Hello World!"
                }
            }
        })
    </script>
</body>
```
<br>

#### Watchers
```html
<body>
    <div id="app">
        <p>Please input: <input type="text" v-model="question"></p>
        <p>{{ response }}</p>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                question: "",
                response: "Waiting"
            },
            watch: {
                question: function (nq, oq) {
                    this.response = "Typing..."
                }
            },
        })
    </script>
</body>
```
<br>

### Binding HTML Classes
`v-bind:class`: dynamically toggle class  
e.g. if `isActive` is true, the class will be shown  
```html
<body>
    <div id="app" class="static" :class="{ active: isActive }">
        <button @click="toggleActive"> Click! </button>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                isActive: true
            },
            methods: {
                toggleActive: function () {
                    if(this.isActive === true) {
                        this.isActive = false
                    } else {
                        this.isActive = true
                    }
                }
            }

        })
    </script>
</body>
```
<br>
Bind to a computed property that returns a object  
```html
<body>
    <div id="app" class="static" :class="classObject">
        <button @click="toggleActive"> Click! </button>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                isActive: true
            },
            computed: {
                classObject: function () {
                    return {
                        isActive: this.isActive
                    }
                }
            },
            methods: {
                toggleActive: function () {
                    if(this.isActive === true) {
                        this.isActive = false
                    } else {
                        this.isActive = true
                    }
                }
            }
        })
    </script>
</body>
```
<br>

Pass an array to `v-bind:class` to apply a list of classes  
```html
<body>
    <div id="app" :class="[activeClass, errorClass]"></div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                activeClass: 'active',
                errorClass: 'text-danger'
            }
        })
    </script>
</body>
```
<br>

```html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```
<br>

### Binding Inline Styles
`v-bind:style`: bind to css  
```html
<body>
    <div id="app" v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">
        Here is the text!
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                activeColor: "red",
                fontSize: 30
            }
        })
    </script>
</body>
```
<br>
```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```
<br>

### Conditional Rendering
#### v-if, v-else, v-else-if
```html
<body>
    <div id="app">
        <div v-if="Math.random() > 0.5">
            Now you see me
        </div>
        <div v-else>
            Now you dont
        </div>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
            }
        })
    </script>
</body>
```
<br>
```html
<body>
    <div id="app">
        <div v-if="Math.random() < 0.3">
            Number is in 0 - 0.3
        </div>
        <div v-else-if="Math.random() < 0.6">
            Number is in 0.3 - 0.6
        </div>
        <div v-else>
            Other
        </div>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
            }
        })
    </script>
</body>
```
<br>

#### template on v-if
```html
<body>
    <div id="app">
        <template v-if="ok">
            <h1>Title</h1>
            <p>Paragraph 1</p>
            <p>Paragraph 2</p>
        </template>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                ok: true
            }
        })
    </script>
</body>
```
<br>

#### Controlling Reusable Elements with \[key\]
Add a `key` attribute to make elements separate  
```html
<body>
    <div id="app">
        <template v-if="loginType === 'username'">
            <label>Username</label>
            <input placeholder="Enter your username" key="username-input">
        </template>
        <template v-else>
            <label>Email</label>
            <input placeholder="Enter your email address" key="email-input">
        </template>
        <br>
        <button @click="toggleLoginType"> Submit </button>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                loginType: 'username'
            },
            methods: {
                toggleLoginType: function () {
                    if(this.loginType === 'username') {
                        this.loginType = 'email'
                    } else {
                        this.loginType = 'username'
                    }
                }
            }
        })
    </script>
</body>
```
<br>

#### v-show
`v-show` will always be rendered and remain in DOM.  
```html
<h1 v-show="ok">Hello!</h1>
```
<br>

### List Rendering
#### Mapping an array to list with v-for
```html
<body>
    <div id="app">
        <ol>
            <li v-for="ele in arr">
                {{ ele.msg }}
            </li>
        </ol>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                arr: [
                    { msg: 'Hello' },
                    { msg: 'World' }
                ]
            },
        })
    </script>
</body>
```
<br>

An optional second argument is for the index of item  
```html
<body>
    <div id="app">
        <ul>
            <li v-for="(ele, idx) in arr">
                {{ ele.msg }} ranks {{ idx + 1 }}.
            </li>
        </ul>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                arr: [
                    { msg: 'Hello' },
                    { msg: 'World' }
                ]
            },
        })
    </script>
</body>
```
<br>

#### An object with v-for
```html
<body>
    <div id="app">
        <ul>
            <li v-for="ele in obj">
                {{ ele }}
            </li>
        </ul>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                obj: {
                    firstName: 'houz',
                    lastName: 'aj',
                    score: 0,
                    ranking: 999
                }
            },
        })
    </script>
</body>
```
<br>

An optional second argument is for the key of item
```html
<body>
    <div id="app">
        <ul>
            <li v-for="(ele, key) in obj">
                {{ key }}: {{ ele }}
            </li>
        </ul>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                obj: {
                    firstName: 'houz',
                    lastName: 'aj',
                    score: 0,
                    ranking: 999
                }
            },
        })
    </script>
</body>
```
<br>

#### About array
Because of limitations in javascript, Vue cannot detect the following changes to an array:  
```html
vm.items[indexOfItem] = newValue
vm.items.length = newLength
```
Therefore, use the following code to replace them  
```html
Vue.set(vm.items, indexOfItem, newValue) //vm.$set <==> Vue.set
vm.items.splice(indexOfItem, 1, newValue)
vm.items.splice(newLength)
```
<br>

#### Object Change Detection
Vue cannot detect property addition or deletion. the method `Vue.set(object, key, value)` can be used to make Vue detect it.  
```html
var vm = new Vue({
  data: {
    userProfile: {
      name: 'Anika'
    }
  }
})

Vue.set(vm.userProfile, 'age', 27)
```
the method `Vue.assign()` can assign a number of new properties to an existing object  
```html
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```
<br>

#### Displaying Filtered/Sorted Results
```html
<body>
    <div id="app">
        <ul>
            <li v-for="n in evenNumbers">
                {{ n }}
            </li>
        </ul>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                numbers: [1, 2, 3, 4, 5, 7, 8, 888]
            },
            computed: {
                evenNumbers: function () {
                    return this.numbers.filter(function (number) {
                        return (number & 1) === 0
                    })
                }
            }
        })
    </script>
</body>
```
<br>

```html
<body>
    <div id="app">
        <ul>
            <li v-for="n in even(numbers)">
                {{ n }}
            </li>
        </ul>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                numbers: [1, 2, 3, 4, 5, 7, 8, 888]
            },
            methods: {
                even: function (numbers) {
                    return this.numbers.filter(function (number) {
                        return (number & 1) === 0
                    })
                }
            }
        })
    </script>
</body>
```
<br>

#### v-for in range
```html
<body>
    <div id="app">
        <ul>
            <li v-for="n in 10">
                {{ n }}
            </li>
        </ul>
    </div>
    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
            }
        })
    </script>
</body>
```
<br>

#### v-for on a \<template\>
```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```
<br>

#### v-for with a Component
In 2.2.0+, when using v-for with a component, a key is now required.  
```html
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
```
<br>

### Event Handling
#### Listening to Events
```html
<body>
    <div id="app">
        <button v-on:click="counter += 1">Add 1</button>
        <p>The button above has been clicked {{ counter }} times.</p>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                counter: 0
            }
        })
    </script>
</body>
```
<br>

#### Method Event Handlers
```html
<body>
    <div id="app">
        <button @click="greet">Click</button>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
            },
            methods: {
                greet: function () {
                    alert("Hello World!")
                }
            }
        })
    </script>
</body>
```
<br>
```html
<body>
    <div id="app">
        <button @click="greet('HHHH')">Click</button>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
            },
            methods: {
                greet: function (msg) {
                    alert("Hello " + msg + "!")
                }
            }
        })
    </script>
</body>
```
<br>

#### Access the original DOM event
`$event`:  pass it into a method using the special $event variable in an inline statement  
```html
<body>
    <div id="app">
        <button v-on:click="warn('Form cannot be submitted yet.', $event)">
            Submit
        </button>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
            },
            methods: {
                warn: function (message, event) {
                    // now we have access to the native event
                    if (event) event.preventDefault()
                    alert(message)
                }
            }
        })
    </script>
</body>
```
<br>

#### Event Modifiers
Vue provides event modifiers for `v-on`. Recall that modifiers are directive postfixes denoted by a dot.  
- .stop  
- .prevent  
- .capture  
- .self  
- .once  
- .passive  

```html
<!-- the click events propagation will be stopped -->
<a v-on:click.stop="doThis"></a>

<!-- the submit event will no longer reload the page -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- modifiers can be chained -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- just the modifier -->
<form v-on:submit.prevent></form>

<!-- use capture mode when adding the event listener -->
<!-- i.e. an event targeting an inner element is handled here before being handled by that element -->
<div v-on:click.capture="doThis">...</div>

<!-- only trigger handler if event.target is the element itself -->
<!-- i.e. not from a child element -->
<div v-on:click.self="doThat">...</div>

<!-- New in 2.1.4+ -->
<!-- the click event will be triggered at most once -->
<a v-on:click.once="doThis"></a>

<!-- New in 2.3.0+ -->
<!-- the scroll event's default behavior (scrolling) will happen -->
<!-- immediately, instead of waiting for `onScroll` to complete  -->
<!-- in case it contains `event.preventDefault()`                -->
<div v-on:scroll.passive="onScroll">...</div>
```
<br>

#### Key Modifiers
Vue allows adding key modifiers for v-on when listening for key events:  
Vue provides aliases for the most commonly used key codes when necessary for legacy browser support:  
- .enter  
- .tab  
- .delete (captures both “Delete” and “Backspace” keys)  
- .esc   
- .space  
- .up  
- .down  
- .left  
- .right  
- .ctrl  
- .alt  
- .shift  
- .meta  
- .exact (new in 2.5.0+, allows control of the exact combination of system modifiers needed to trigger an event)   

```html
<body>
    <div id="app">
        <!-- only call `vm.submit()` when the `key` is `Enter` -->
        <input v-on:keyup.enter="submit">
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            methods: {
                submit: function () {
                    alert('success!')
                }
            },

        })
    </script>
</body>
```
<br>

```html
<body>
    <div id="app">
        <textarea cols="30" rows="10" v-on:keyup.67.ctrl="perCopy">

        </textarea>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            methods: {
                perCopy: function () {
                    alert('Don\'t Copy!')
                }
            },

        })
    </script>
</body>
```
<br>

#### Mouse Button Modifiers
(New in 2.2.0+)  
- .left  
- .right  
- .middle  

<br>

### Form Input Bindings
#### Basic Usage
`v-model`: binding to input, textarea, checkbox, ratio, select, etc  
<br>
e.g. Bind to an array  
```html
<body>
    <div id="app">
        <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
        <label>Jack</label>
        <input type="checkbox" id="john" value="John" v-model="checkedNames">
        <label>John</label>
        <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
        <label>Mike</label>
        <br>
        <span>Checked names: {{ checkedNames }}</span>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                checkedNames: []
            }
        })
    </script>
</body>
```
<br>
```html
<body>
    <div id="app">
        <select v-model="selected" multiple>
            <option>A</option>
            <option>B</option>
            <option>C</option>
        </select>
        <br>
        <span>Selected: {{ selected }}</span>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                selected: []
            }
        })
    </script>
</body>
```
<br>

#### Value Bindings
```html
<!-- `picked` is a string "a" when checked -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` is either true or false -->
<input type="checkbox" v-model="toggle">

<!-- `selected` is a string "abc" when the first option is selected -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>

<!-- When toggle is true, the value is 'yes'. Otherwise, it is 'no' -->
<input type="checkbox" v-model="toggle" true-value="yes" false-value="no">
```
<br>

#### Modifiers
`.lazy`:  add the `lazy` modifier to instead sync after `change` events  
```html
<body>
    <div id="app">
        <input v-model.lazy="msg" @change="show">
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: '#app',
            data: {
                msg: ''
            },
            methods: {
                show: function () {
                    alert(this.msg);
                }
            }
        })
    </script>
</body>
```
<br>
`.number`: automatically typecast as a number  
```html
<input v-model.number="age" type="number">
```
<br>
`.trim`: make user input be trimmed automatically  
```html
<input v-model.trim="msg">
```
<br>

### Components Basics
#### data must be a function
In order to make component independent, data must be a function  
```html
data: function () {
  return {
    count: 0
  }
}
```
<br>

#### Passing Data to Child Components with Props
```html
<body>
    <div id="app">
        <sample title="HELLO WORLD!"></sample>
    </div>

    <script type="text/javascript">
        Vue.component('sample', {
            props: ['title'],
            template: `<button>{{ title }}</button>`
        });

        var app = new Vue({
            el: '#app',
            data: {
                msg: ''
            },
            methods: {
                show: function () {
                    alert(this.msg);
                }
            }
        })
    </script>
</body>
```
<br>

#### A Single Root Element
Every component must have a single root element  
<br>

#### Passing Data to Parent Component with $emit
```html
<body>
    <div id="app">
        <div :style="{ fontSize: postFontSize + 'em' }">
            <blog-post
                    v-for="post in posts"
                    v-bind:key="post.id"
                    v-bind:post="post"
                    v-on:enlarge-text="postFontSize += $event"
            ></blog-post>
        </div>
    </div>

    <script type="text/javascript">
        Vue.component('blog-post', {
            props: ['post'],
            template: `
                <div class="blog-post">
                  <h3>{{ post.title }}</h3>
                  <button v-on:click="$emit('enlarge-text', 0.1)">
                      Enlarge text
                  </button>
                  <div v-html="post.content"></div>
                </div>`
        });

        var app = new Vue({
            el: '#app',
            data: {
                postFontSize: 1,
                posts: [
                    {
                        title: 'Hello World',
                        id: 0,
                        content: 'NOTHING'
                    }
                ]
            }
        })
    </script>
</body>
```
<br>

#### Dynamic Components
```html
<!-- Component changes when currentTabComponent changes -->
<component v-bind:is="currentTabComponent"></component>
```
