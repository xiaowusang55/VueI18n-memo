# Custom directive localization

Support Version

🆕 7.3+

You can translate not only with `v-t` custom directive, but also with `$t` method.

## String syntax

You can pass the keypath of locale messages with string syntax.

Javascript:

```js
new Vue({
  i18n: new VueI18n({
    locale: 'en',
    messages: {
      en: { hello: 'hi there!' },
      ja: { hello: 'こんにちは！' }
    }
  }),
  data: { path: 'hello' }
}).$mount('#string-syntax')

```

Templates:

```html
<div id="string-syntax">
  <!-- string literal -->
  <p v-t="'hello'"></p>
  <!-- keypath binding via data -->
  <p v-t="path"></p>
</div>

```

Outputs:

```html
<div id="string-syntax">
  <p>hi there!</p>
  <p>hi there!</p>
</div>

```

## Object syntax

You can use object syntax.

Javascript:

```js
new Vue({
  i18n: new VueI18n({
    locale: 'en',
    messages: {
      en: { hello: 'hi {name}!' },
      ja: { hello: 'こんにちは、{name}！' }
    }
  }),
  computed: {
    nickName () { return 'kazupon' }
  },
  data: { path: 'hello' }
}).$mount('#object-syntax')

```

Templates:

```html
<div id="object-syntax">
  <!-- literal -->
  <p v-t="{ path: 'hello', locale: 'ja', args: { name: 'kazupon' } }"></p>
  <!-- data binding via data -->
  <p v-t="{ path: path, args: { name: nickName } }"></p>
</div>

```

Outputs:

```html
<div id="object-syntax">
  <p>こんにちは、kazupon！</p>
  <p>hi kazupon!</p>
</div>

```

## Use with transitions

Support Version

🆕 8.7+

When v-t directive is applied to an element inside `<transition>` component

, you may notice that translated message will disappear during the transition. This behavior is related to the nature of the `<transition>` component implementation – all directives in disappearing element inside `<transition>` component will be destroyed before transition starts. This behavior may result in content flickering on short animations, but most noticable on long transitions.

To make sure directive content will stay un-touched during transition just add `.preserve` modifier to `v-t` directive defintion.

Javascript:

```js
new Vue({
  i18n: new VueI18n({
    locale: 'en',
    messages: {
      en: { preserve: 'with preserve' },
    }
  }),
  data: { toggle: true }
}).$mount('#in-transitions')

```

Templates:

```html
<div id="in-transitions">
  <transition name="fade">
    <span v-if="toggle" v-t.preserve="'preserve'"></span>
  </transition>
  <button @click="toggle = !toggle">Toggle</button>
</div>

```

```js
new Vue({
  i18n: new VueI18n({
    locale: 'en',
    messages: {
      en: { preserve: 'with preserve' },
    },
    preserveDirectiveContent: true
  }),
  data: { toggle: true }
}).$mount('#in-transitions')

```

Templates:

```html
<div id="in-transitions">
  <transition name="fade">
    <span v-if="toggle" v-t="'preserve'"></span>
  </transition>
  <button @click="toggle = !toggle">Toggle</button>
</div>

```

## `$t` vs `v-t`

### $t

`$t` is extended Vue instance method. It has the following pros and cons:

#### Pros

You can flexibly use mustash syntax {{}} in templates and also computed props and methods in Vue instance.

#### Cons

$t is executed every time when re-render occurs, so it does have a translation costs.

### v-t

v-t is a custom directive. It has the following pros and cons:

#### Pros

v-t has better performance than the $t method due to its cache with the custom directive, when translated once. Also, pre-translation is possible with the Vue compiler module which was provided by vue-i18n-extensions .

Therefore it's possible to make more performance optimizations.

#### Cons

v-t can not be flexibly used like $t, it's rather complex. The translated content with v-t is inserted into the textContent of the element. Also, when you use server-side rendering, you need to set the custom directive to directives option of the createRenderer function.

