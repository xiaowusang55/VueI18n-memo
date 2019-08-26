# DateTime localization

Support Version

🆕 7.0+

You can localize the datetime with your definition formats.

DateTime formats the below:

```js
const dateTimeFormats = {
    'en-US': {
        short: {
            year: 'numeric', month: 'short', day: 'numeric'
        },
        long: {
            year: 'numeric', month: 'short', day: 'numeric',
            weekday: 'short', hour: 'numeric', minute: 'numeric'
        }
    },
    'ja-JP': {
        short: {
            year: 'numeric', month: 'short', day: 'numeric'
        },
        long: {
            year: 'numeric', month: 'short', day: 'numeric',
            weekday: 'short', hour: 'numeric', minute: 'numeric', hour12: true
        }
    },
}
```

As the Above, You can define the datetime format with named (e.g. short, long, etc), and you need to use the options with ECMA-402 Intl.DateTimeFormat

After that like the locale messages, You need to specify the dateTimeFormats option of VueI18n constructor:

```js
const i18n = new VueI18n({
    dateTimeFormats
})
```

Template the below:

```html
<div id="app">
    <P>{{ $d(new Date(), 'short') }}</P>
    <P>{{ $d(new Date(), 'long', 'ja-JP') }}</P>
</div>
```

Output the below:

```html
<div id="app">
  <p>Apr 19, 2017</p>
  <p>2017年4月19日(水) 午前2:19</p>
</div>
```
