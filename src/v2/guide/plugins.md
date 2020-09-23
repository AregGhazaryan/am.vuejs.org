---
title: Plugin֊ներ
type: ուղեցույց
order: 304
---

Plugin֊ները սովորականում ավելացնում են գլոբալ աստիճանի ֆունկցիոնալություն Vue֊ին։ Խիստ հայտարարված scope չկա plugin֊ի համար - կան մի քանի տիպեր plugin֊ների․

1. Ավելացնել որոշ գլոբալ մեթոդներ կամ հատկություններ օրինակ [vue-custom-element֊ը](https://github.com/karol-f/vue-custom-element)

2. Ավելացնել մեկ կամ ավելին գլոբալ asset֊ներ․ ուղղորդիչներ/ֆիլտրներ/անցումներ և այլն։ Օրինակ [vue-touch֊ը](https://github.com/vuejs/vue-touch)

3. Ավելացնել որոշ կոմպոնենտի ընտրանքներ գլոբալ mixin֊ով։ Օրինակ [vue-router֊ը](https://github.com/vuejs/vue-router)

4. Ավելացնել որոշ Vue instance֊ի մեթոդներ միացնելով նրանց Vue.prototype֊ին։

5. Գրադարան որը տրամադրում է իր API֊ը, միաժամանակ ներառելով վերևում նշվածներից որպես համադրություն։ Օրինակ [vue-router֊ը](https://github.com/vuejs/vue-router)

## Plugin֊ի Օգտագործումը

Օգտագործեք plugin֊ները կանչելով `Vue.use()` գլոբալ մեթոդը։ Սա կարող է կատարվել նախքան սկսելով ձեր ծրագիրը կանչելով `new Vue()`֊ն․

``` js
// կանչում է `MyPlugin.install(Vue)`
Vue.use(MyPlugin)

new Vue({
  //... ընտրանքներ
})
```

Դուք կարող էք փոխանցել որոշ ընտրանքներ․

``` js
Vue.use(MyPlugin, { someOption: true })
```

`Vue.use`֊ը ավտոմատ կերպով խոսւափում է նույն plugin֊ի վերօգոագործումից, այնպես որ կանչելով մի քանի անգամ նույն plugin֊ի վրա կտեղադրի plugin֊ը ընդհամենը մեկ անգամ։

Որոշ plugin֊ներ տրամադրված Vue.js֊ի պաշտոնական plugin֊ներից ինչպիսին է `vue-router` ավտոմատ կերպով կանչում է `Vue.use()`֊ը եթե `Vue`֊ն հասանելի է որպես գլոբալ փոփոխական։ Սակայն մոդուլյար environment֊ում ինչպիսին է CommonJS, դուք միշտ պետք է կանչեք `Vue.use()` պարտադիր․

``` js
// Երբ օգտագործում ենք CommonJS Browserify֊ով կամ Webpack֊ով
var Vue = require('vue')
var VueRouter = require('vue-router')

// Մի մոռացեք կանչել սա
Vue.use(VueRouter)
```

Նայեք [awesome-vue](https://github.com/vuejs/awesome-vue#components--libraries) մեծածավալ համայնքի կողմից ստեղծված plugin֊ների և գրադարանների համաքածուին:

## Plugin֊ի Ստեղծումը

Vue.js֊ի plugin֊ը պետք է ունենա `install` մեթոդ։ Այդ մեթոդը կկանչվի `Vue`֊ի կոնստրուկտորի հետ որպես առաջին արգումենտ, այլ հասանելի ընտրանքների հետ հանդերձ․

``` js
MyPlugin.install = function (Vue, options) {
  // 1. ավելացնել գլոբալ մեթոդ կամ հատկություն
  Vue.myGlobalMethod = function () {
    // որոշ տրամաբանություն ...
  }

  // 2. ավելացնել գլոբալ asset
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // որոշ տրամաբանություն ...
    }
    ...
  })

  // 3. ներարկենք որևէ կոմպոնենտի ընտրանքները
  Vue.mixin({
    created: function () {
      // որոշ տրամաբանություն ...
    }
    ...
  })

  // 4. ավելացնենք instance֊ի մեթոդ
  Vue.prototype.$myMethod = function (methodOptions) {
    // որոշ տրամաբանություն ...
  }
}
```
