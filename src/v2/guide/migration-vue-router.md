---
title: Միգրացիան Vue Router 0.7.x-ից
type: ուղեցույց
order: 702
---

> Միայն Vue Router 2-ն է համապատասխան Vue 2-ի հետ, եթե դուք թարմացնում եք Vue-ն, դուք պետք է թարմացնեք Vue Router-ը նույնպես։ Այդ պատճառով մենք ներառել են մանրամասներ միգրացիայի ուղղում հիմնական փաստաթղթի մեջ։ Ամբողջական ուղեցույցը թե ինչպես օգտագործել նոր Vue Router-ը, նայեք [Vue Router-ի փաստաթղթերը](https://router.vuejs.org/en/).

## Router-ի Գործարկումը

### `router.start` <sup>փոխարինվել է</sup>

Այլևս գոյություն չունի հատուկ API որպեսզի գործարկել ծրագիրը Vue Router-ի հետ հանդերձ։ Դա նշանակում է որ ի փոխարեն․

``` js
router.start({
  template: '<router-view></router-view>'
}, '#app')
```

Դուք փոխանցում եք router հատկությունը դեպի Vue instance.

``` js
new Vue({
  el: '#app',
  router: router,
  template: '<router-view></router-view>'
})
```

Կամ, եթե դուք օգտագործում եք միայն runtime-ը build-ը Vue-ի․

``` js
new Vue({
  el: '#app',
  router: router,
  render: h => h('router-view')
})
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ որտեղ<code>router.start</code>-ը կանչվում է։</p>
</div>
{% endraw %}

## Route-ի Հայտարարումը

### `router.map` <sup>փոխարինված է</sup>

Route-ները հիմա հայտարարվում են որպես զանգված [`routes` ընտրանքի վրա](https://router.vuejs.org/en/essentials/getting-started.html#javascript) router-ի ստեղծման ժամանակ։ Այնպես որ այս route-ները օրինակի համար․

``` js
router.map({
  '/foo': {
    component: Foo
  },
  '/bar': {
    component: Bar
  }
})
```

Կլինեն հայտարարված այսպես․

``` js
var router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo },
    { path: '/bar', component: Bar }
  ]
})
```

Զանգվածի գրելաձևը թույլ է տալիս լինել ավելի կանխագուշակելի route-ի համապատասխանեցման մեջ, մինչ օբյեկտի միջով անցնելուց խորհուրդ չի տրվում օգտագործել նույն հատկությունների հերթականությունը տարբեր բրաուզերներում։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>router.map</code>-ի կանչման մասին։</p>
</div>
{% endraw %}

### `router.on` <sup>ջնջված է</sup>

Եթե դուք պետք է ծրագրավորմամբ գեներացնեք route-ներ երբ աշխատացնում եք ձեր ծրագիրը, դուք կարող էք դինամիկորեն ավելացնեք հայտարարությունները route-ների զանգվածին։ Օրինակի համար․

``` js
// Սովարական հիմնական route-ներ
var routes = [
  // ...
]

// Դինամիկորեն գեներացված route-ներ
marketingPages.forEach(function (page) {
  routes.push({
    path: '/marketing/' + page.slug
    component: {
      extends: MarketingComponent
      data: function () {
        return { page: page }
      }
    }
  })
})

var router = new Router({
  routes: routes
})
```

Եթե դուք ցանկանում եք ավելացներ նոր route-ներ router-ի ստեղծումից հետո, դուք պետք է փոխարինեք router—ի համատասխանող մասը նորով որը ներառում է այն route—ի որը որ դուք ցանկանում եք ավելացնել․

``` js
router.match = createMatcher(
  [{
    path: '/my/new/path',
    component: MyComponent
  }].concat(router.options.routes)
)
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>router.on</code>-ի կանչման վերաբերյալ։</p>
</div>
{% endraw %}

### `router.beforeEach` <sup>փոխված է</sup>

`router.beforeEach` հիմա աշխատում է ասինխռոն կերպով և վերցնում է `next` ֆունկցիան որպես իր երրորդ արգումենտ։

``` js
router.beforeEach(function (transition) {
  if (transition.to.path === '/forbidden') {
    transition.abort()
  } else {
    transition.next()
  }
})
```

``` js
router.beforeEach(function (to, from, next) {
  if (to.path === '/forbidden') {
    next(false)
  } else {
    next()
  }
})
```

### `subRoutes` <sup>վերանվանված է</sup>

[Վերանվանվել է `children`](https://router.vuejs.org/en/essentials/nested-routes.html) Vue-ի և այլ routing-ի գրադարանների հետ ավելի կայուն լինելով նպատակով։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ճանապարհ</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>subRoutes</code> ընտրանքի վերաբերյալ։</p>
</div>
{% endraw %}

### `router.redirect` <sup>փոխարինված է</sup>

Հիմա այն [ընտրանք է route-ի հայտարարումներում](https://router.vuejs.org/en/essentials/redirect-and-alias.html)։ Օրինակի համար, դուք պետք է թարմացնեք․

``` js
router.redirect({
  '/tos': '/terms-of-service'
})
```

ինչպիսին է ներքևում գրված հայտարարումը այնպես էլ ձեր `routes` կոնֆիգուրացիան․

``` js
{
  path: '/tos',
  redirect: '/terms-of-service'
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>router.redirect</code>-ի կանչելու վերաբերյալ։</p>
</div>
{% endraw %}

### `router.alias` <sup>փոխարինված է</sup>

Հիմա այն [ընտրանք է route-ի հայտարարման համար](https://router.vuejs.org/en/essentials/redirect-and-alias.html) ցանկալի է օգտագործել փոխանուն։ Օրինակի համար, դուք պետք է թարմացնեք․

``` js
router.alias({
  '/manage': '/admin'
})
```

ինչպիսին է ներքևում գրված հայտարարումը այնպես էլ ձեր `routes` կոնֆիգուրացիան․

``` js
{
  path: '/admin',
  component: AdminPanel,
  alias: '/manage'
}
```

Եթե դուք պետք է օգտագործեք բազմաթիվ փոխանուններ, դուք պետք է նաև օգտագործեք զանգվածի գրելաձևը․

{% codeblock lang:js %}
alias: ['/manage', '/administer', '/administrate']
{% endcodeblock %}

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>router.alias</code>-ի կանչելու վերաբերյալ։</p>
</div>
{% endraw %}

### Կամայական Route-ի Հատկությունները <sup>փոխարինված է</sup>

Կամայական route-ի հատկությունները պետք է լինեն նոր meta հատկությունների scope-ի ներքո, որպեսզի խուսափենք ապագա նորացումների կոնֆլիկտներից։ Օրինակի համար, եթե դուք ունեք հայտարարած․

``` js
'/admin': {
  component: AdminPanel,
  requiresAuth: true
}
```

Այնուհետև դուք պետք է թարմացնեք այն․

``` js
{
  path: '/admin',
  component: AdminPanel,
  meta: {
    requiresAuth: true
  }
}
```

Այնուհետև երբ հետո մուտք գործենք դեպի այս հատկություն route—ի վրա, դուք պետք է անցնեք meta—ի միջով։ Օրինակի համար․

``` js
if (route.meta.requiresAuth) {
  // ...
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ կամայական route-ի հատկաություների մասին որոնք որ չեն գտնվում meta-ի ներքո։</p>
</div>
{% endraw %}

### [] Զանգվածների Գրելաձևը Հարցումներում <sup>ջնջված է</sup>

Երբ փոխանցում ենք զանգվածներ հարցումների պարամետրերին QueryString գրելաձևը այլևս գոյություն չունի `/foo?users[]=Tom&users[]=Jerry`, փոխարենը, նոր գրելաձևն է `/foo?users=Tom&users=Jerry`։ Ներքուստ, `$route.query.users` դեռ կլնի զանգված, բայց եթե ունենք ընդամենը մեկ պարամետր հարցման մեջ․ `/foo?users=Tom`, երբ ուղիղ մուտք ենք գործում այս route, ոչ մի ձև router-ը չի հասականա որ մենք ենթադրում ենք որ `users`-ը կլինի զանգված։ Այս պատճառով, ավելացրեք հաշվարկված հատկություն և փոխարինեք ամեն դիմումը `$route.query.users`-ին նրանով։

```javascript
export default {
  // ...
  computed: {
    // users-ը միշտ կլինի զանգված
    users () {
      const users = this.$route.query.users
      return Array.isArray(users) ? users : [users]
    }
  }
}
```

## Route-ի Համապատասխանեցում

Route-ի համապատասխանեցումը հիմա օգտագործում է [path-to-regexp](https://github.com/pillarjs/path-to-regexp) ներքինում, դարձնելով այն ավելի ճկուն քան նախկինում։

### Մեկ կամ Ավելին Անվանված Պարամետրեր <sup>փոփոխված է</sup>

Գրելաձևը մի փոքր փոխվել է, օրինակի համար `/category/*tags`-ը, պետք է թարմացվի դեպի `/category/:tags+`։

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the obsolete route syntax.</p>
</div>
{% endraw %}

## Links

### `v-link` <sup>replaced</sup>

The `v-link` directive has been replaced with a new [`<router-link>` component](https://router.vuejs.org/en/api/router-link.html), as this sort of job is now solely the responsibility of components in Vue 2. That means whenever wherever you have a link like this:

``` html
<a v-link="'/about'">About</a>
```

You'll need to update it like this:

``` html
<router-link to="/about">About</router-link>
```

Note that `target="_blank"` is not supported on `<router-link>`, so if you need to open a link in a new tab, you have to use `<a>` instead.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>v-link</code> directive.</p>
</div>
{% endraw %}

### `v-link-active` <sup>replaced</sup>

The `v-link-active` directive has also been replaced by the `tag` attribute on [the `<router-link>` component](https://router.vuejs.org/en/api/router-link.html). So for example, you'll update this:

``` html
<li v-link-active>
  <a v-link="'/about'">About</a>
</li>
```

to this:

``` html
<router-link tag="li" to="/about">
  <a>About</a>
</router-link>
```

The `<a>` will be the actual link (and will get the correct href), but the active class will be applied to the outer `<li>`.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>v-link-active</code> directive.</p>
</div>
{% endraw %}

## Programmatic Navigation

### `router.go` <sup>changed</sup>

For consistency with the [HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API), `router.go` is now only used for [back/forward navigation](https://router.vuejs.org/en/essentials/navigation.html#routergon), while [`router.push`](https://router.vuejs.org/en/essentials/navigation.html#routerpushlocation) is used to navigate to a specific page.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of <code>router.go</code> being used where <code>router.push</code> should be used instead.</p>
</div>
{% endraw %}

## Router Options: Modes

### `hashbang: false` <sup>removed</sup>

Hashbangs are no longer required for Google to crawl a URL, so they are no longer the default (or even an option) for the hash strategy.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>hashbang: false</code> option.</p>
</div>
{% endraw %}

### `history: true` <sup>replaced</sup>

All routing mode options have been condensed into a single [`mode` option](https://router.vuejs.org/en/api/options.html#mode). Update:

``` js
var router = new VueRouter({
  history: 'true'
})
```

to:

``` js
var router = new VueRouter({
  mode: 'history'
})
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>history: true</code> option.</p>
</div>
{% endraw %}

### `abstract: true` <sup>replaced</sup>

All routing mode options have been condensed into a single [`mode` option](https://router.vuejs.org/en/api/options.html#mode). Update:

``` js
var router = new VueRouter({
  abstract: 'true'
})
```

to:

``` js
var router = new VueRouter({
  mode: 'abstract'
})
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>abstract: true</code> option.</p>
</div>
{% endraw %}

## Route Options: Misc

### `saveScrollPosition` <sup>replaced</sup>

This has been replaced with a [`scrollBehavior` option](https://router.vuejs.org/en/advanced/scroll-behavior.html) that accepts a function, so that the scroll behavior is completely customizable - even per route. This opens many new possibilities, but to replicate the old behavior of:

``` js
saveScrollPosition: true
```

You can replace it with:

``` js
scrollBehavior: function (to, from, savedPosition) {
  return savedPosition || { x: 0, y: 0 }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>saveScrollPosition: true</code> option.</p>
</div>
{% endraw %}

### `root` <sup>renamed</sup>

Renamed to `base` for consistency with [the HTML `<base>` element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>root</code> option.</p>
</div>
{% endraw %}

### `transitionOnLoad` <sup>removed</sup>

This option is no longer necessary now that Vue's transition system has explicit [`appear` transition control](transitions.html#Transitions-on-Initial-Render).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>transitionOnLoad: true</code> option.</p>
</div>
{% endraw %}

### `suppressTransitionError` <sup>removed</sup>

Removed due to hooks simplification. If you really must suppress transition errors, you can use [`try`...`catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) instead.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>suppressTransitionError: true</code> option.</p>
</div>
{% endraw %}

## Route Hooks

### `activate` <sup>replaced</sup>

Use [`beforeRouteEnter`](https://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) in the component instead.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>activate</code> hook.</p>
</div>
{% endraw %}

### `canActivate` <sup>replaced</sup>

Use [`beforeEnter`](https://router.vuejs.org/en/advanced/navigation-guards.html#perroute-guard) in the route instead.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>canActivate</code> hook.</p>
</div>
{% endraw %}

### `deactivate` <sup>removed</sup>

Use the component's [`beforeDestroy`](../api/#beforeDestroy) or [`destroyed`](../api/#destroyed) hooks instead.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>deactivate</code> hook.</p>
</div>
{% endraw %}

### `canDeactivate` <sup>replaced</sup>

Use [`beforeRouteLeave`](https://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) in the component instead.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>canDeactivate</code> hook.</p>
</div>
{% endraw %}

### `canReuse: false` <sup>removed</sup>

There's no longer a use case for this in the new Vue Router.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>canReuse: false</code> option.</p>
</div>
{% endraw %}

### `data` <sup>replaced</sup>

The `$route` property is now reactive, so you can use a watcher to react to route changes, like this:

``` js
watch: {
  '$route': 'fetchData'
},
methods: {
  fetchData: function () {
    // ...
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>data</code> hook.</p>
</div>
{% endraw %}

### `$loadingRouteData` <sup>removed</sup>

Define your own property (e.g. `isLoading`), then update the loading state in a watcher on the route. For example, if fetching data with [axios](https://github.com/mzabriskie/axios):

``` js
data: function () {
  return {
    posts: [],
    isLoading: false,
    fetchError: null
  }
},
watch: {
  '$route': function () {
    var self = this
    self.isLoading = true
    self.fetchData().then(function () {
      self.isLoading = false
    })
  }
},
methods: {
  fetchData: function () {
    var self = this
    return axios.get('/api/posts')
      .then(function (response) {
        self.posts = response.data.posts
      })
      .catch(function (error) {
        self.fetchError = error
      })
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run the <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> on your codebase to find examples of the <code>$loadingRouteData</code> meta property.</p>
</div>
{% endraw %}
