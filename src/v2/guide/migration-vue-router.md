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
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ հին route֊ի գրելաձևի վերաբերյալ։</p>
</div>
{% endraw %}

## Link֊եր

### `v-link` <sup>փոխարինված է</sup>

`v-link` ուղղորդիչը փոխարինվել է նոր [`<router-link>` կոմպոնենտով](https://router.vuejs.org/en/api/router-link.html), որովհետև այս տիպի գործը հիմա միայն հենված է կոմպոնենտների վրա Vue 2֊ում։ Սա նշանակում է երբ որ դուք օրինակի համար ունեք այսպիսի հղում․

``` html
<a v-link="'/about'">About</a>
```

Դուք պետք է թարմացնեք այն այսպես․

``` html
<router-link to="/about">About</router-link>
```

Նշում որ `target="_blank"`֊ը այլևս չի համապատասխանում `<router-link>`֊ին, այնպես որ եթե ձեզ պետք է նոր պատուհան բացել հղման համար, դուք պետք է օգտագործեք `<a>`֊ն փոխարենը։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>v-link</code> ուղորդիչի վերաբերյալ։</p>
</div>
{% endraw %}

### `v-link-active` <sup>փոխարինված է</sup>

`v-link-active` ուղղորդիչը փոխարինվել է `tag` ատրիբուտով [`<router-link>` կոմպոնենտում](https://router.vuejs.org/en/api/router-link.html)։ Օրինակի համար, դուք պետք է թարմացնեք սա․

``` html
<li v-link-active>
  <a v-link="'/about'">About</a>
</li>
```

դեպի

``` html
<router-link tag="li" to="/about">
  <a>About</a>
</router-link>
```

`<a>`֊ն կլինի իրանական հղումը (և կստանա ճիշտ href֊ը), բայց ակտիվ class֊ը կկիրառվի արտաքին `<li>`֊ի վրա։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>v-link-active</code> ուղորդիչի վերաբերյալ։</p>
</div>
{% endraw %}

## Ծրագրավորված Navigation

### `router.go` <sup>փոխված է</sup>

[HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API)֊ի հետ կայուն լինելու պատճառով, `router.go`֊ն հիմա միայն կօգտագործվի [հետ/առաջ navigation֊ի համար](https://router.vuejs.org/en/essentials/navigation.html#routergon), երբ [`router.push`](https://router.vuejs.org/en/essentials/navigation.html#routerpushlocation)֊ը օգտագործվում է որպեսզի անցնել որևէ կոնկրետ էջի։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք<a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտերլ օրինակներ <code>router.go</code>֊ի օգտագործման որտեղ <code>router.push</code>֊ը պետք է օգտագործվի փոխարենը։</p>
</div>
{% endraw %}

## Router֊ի Ընտրանքներ: Mode

### `hashbang: false` <sup>ջնջված է</sup>

Hashbang֊երը այլևս չեն պահանջվում որպեսզի Google֊ը դիտարկի URL֊ը, այնպես որ նրանք այլևս հիմնական (կամ նույնիսկ ընտրանք) չեն hash ստրատեգիայի համարԼ

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>hashbang: false</code> ընտրանքի վերաբերյալ։</p>
</div>
{% endraw %}

### `history: true` <sup>փոխարինված է</sup>

Բոլոր route֊ների mode ընտրանքները ձուլվել են դեպի մեկ [`mode` ընտրանք](https://router.vuejs.org/en/api/options.html#mode)։ Թարմացրեք․

``` js
var router = new VueRouter({
  history: 'true'
})
```

դեպի․

``` js
var router = new VueRouter({
  mode: 'history'
})
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>history: true</code> ընտրքանի վերաբերյալ։</p>
</div>
{% endraw %}

### `abstract: true` <sup>փոխարինված է</sup>

Բոլոր route֊ների mode ընտրանքները ձուլվել են դեպի մեկ [`mode` ընտրանք](https://router.vuejs.org/en/api/options.html#mode)։ Թարմացրեք․

``` js
var router = new VueRouter({
  abstract: 'true'
})
```

դեպի․

``` js
var router = new VueRouter({
  mode: 'abstract'
})
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>abstract: true</code> ընտրանքի վերաբերյալ։</p>
</div>
{% endraw %}

## Route֊ի Ընտրանքներ: Այլ Ընտրանքներ

### `saveScrollPosition` <sup>փոխարինված է</sup>

Սա հիմա փոխարինված է [`scrollBehavior` ընտրանքով](https://router.vuejs.org/en/advanced/scroll-behavior.html) որը ընդունում է ֆունկցիա, այնպես որ scroll֊ի վարքը ամբողջովին վերափոխելի է - նույնիսկ ամեն route֊ի համար։ Սա բացում է շատ նոր հնարավորություններ, բայց հին օրինակը փոխելու համար դուք պետք է․

``` js
saveScrollPosition: true
```

Փոխարինեք այն հետևյալով․

``` js
scrollBehavior: function (to, from, savedPosition) {
  return savedPosition || { x: 0, y: 0 }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>saveScrollPosition: true</code> ընտրանքի վերաբերյալ։</p>
</div>
{% endraw %}

### `root` <sup>վերանվանված է</sup>

Վերանվանվել է դեպի `base` որպեսզի կայուն լինի [HTML `<base>` էլեմենտի հետ](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base)։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>root</code> ընտրանքի վերաբերյալնպմ option.</p>
</div>
{% endraw %}

### `transitionOnLoad` <sup>ջնջված է</sup>

Այս ընտրանքը այլևս պարտադիր չէ քանի որ հիմա Vue-ի անցումային համակարգը ունի հատուկ [`appear` անցման կառավարում](transitions.html#Transitions-on-Initial-Render)։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>transitionOnLoad: true</code> ընտրանքի վերաբերյալ։</p>
</div>
{% endraw %}

### `suppressTransitionError` <sup>ջնջված է</sup>

Ջնջված է օգուտ hook֊երի պարզության համար։ Եթե դուք իրոք պետք է անցումային error֊ները բռնեք, փորձեք [`try`...`catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) փոխարենը։ 

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>suppressTransitionError: true</code> ընտրանքի վերաբերյալ։</p>
</div>
{% endraw %}

## Route֊ի Hook֊եր

### `activate` <sup>փոխարինված է</sup>

Փոխարենը օգտագործեք [`beforeRouteEnter`](https://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) կոմպոնենտում։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>activate</code> hook֊ի վերաբերյալ։</p>
</div>
{% endraw %}

### `canActivate` <sup>փոխարինված է</sup>

Փոխարենը օգտագործեք [`beforeEnter`](https://router.vuejs.org/en/advanced/navigation-guards.html#perroute-guard) route֊ում։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>canActivate</code> hook֊ի վերաբերյալ։</p>
</div>
{% endraw %}

### `deactivate` <sup>ջնջված է</sup>

Փոխարենը օգտագործեք կոմպոնենտի [`beforeDestroy`](../api/#beforeDestroy) կամ [`destroyed`](../api/#destroyed) hook֊երը։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>deactivate</code> hook֊ի վերաբերյալ։</p>
</div>
{% endraw %}

### `canDeactivate` <sup>փոխարինված է</sup>

Փոխարենը օգտագործեք [`beforeRouteLeave`](https://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) կոմպոնենտում։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>canDeactivate</code> hook֊ի վերաբերյալ։</p>
</div>
{% endraw %}

### `canReuse: false` <sup>ջնջված է</sup>

Այլևս չկա օգտագործման դեպք նոր Vue Router֊ում։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>canReuse: false</code> ընտրանքի վերաբերյալ։</p>
</div>
{% endraw %}

### `data` <sup>փոխարինված է</sup>

`$route` հատկությունը հիմա ռեակտիվ է, այնպես որ դուք կարող էք օգտագործել watcher որպեսզի route֊ի փոփոխությունները դարձնեք ռեակտիվ, այս կերպ․

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
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>data</code> hook֊ի վերաբերյալ։</p>
</div>
{% endraw %}

### `$loadingRouteData` <sup>ջնջված է</sup>

Հայտարարեք ձեր հատկությունը (օրինակ՝ `isLoading`), այնուհետև թարմացրեք բեռնման state֊ը watcher֊ում route֊ի վրա։ Օրինակի համար, եթե ստանում ենք տվյալներ [axios֊ով](https://github.com/mzabriskie/axios)․

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
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>$loadingRouteData</code> meta հատկության վերաբերյալ։</p>
</div>
{% endraw %}
