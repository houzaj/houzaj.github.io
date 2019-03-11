---
layout: post
title: '刷题记（无向图的极大团与最大团）'
date: 2019-03-07
author: HouZAJ
cover: 'https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190307%203-21/20190307-01.png'
tags: Programming
---

> CONTENT:  无向图的极大团与最大团的Bron–Kerbosch算法  
> DETAIL: Bron–Kerbosch算法个人的一点理解
> PROBLEMS:  
> ZOJ 1492  
> CF 1105E  

<br>

<iframe type="text/html" src="http://music.163.com/outchain/player?type=2&id=28377225&auto=0&height=66" frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86"></iframe>      

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

###
