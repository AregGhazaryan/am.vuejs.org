---
title: Միգրացիան Vuex 0.6.x֊ից դեպի 1.0
type: ուղեցույց
order: 703
---

> Vuex 2.0 թողարկվել է, բա՞յց այս ուղեցույցը միայն նշում է միգրացիան 1.0, մի թե՞ սխալ է գրվալ։ Նաև կարծես թե Vuex 1.0 և 2.0֊ը թողարկվել են միաժամանակ։ Ի՞նչ է կատարվում, որը պետք է ես օգտագործեմ և ի՞նչն է համապատասխանում Vue 2.0֊ին

Եվ Vuex 1.0 և 2.0․

- ամբողջովին համապատասխանում են և Vue 1.0 և 2.0
- կպահպանվեն մոտ ապագայում

Ուղված են մի փոքր տարբեր օգտագործողների համար, սակայն։

__Vuex 2.0__֊ը ռադիկալ վերափոխում և պարզեցումն է API֊ի, նրանց համար ովքեր սկսում են նոր նախագծեր և ցանկանում են ունենալ ամենավերջին և հզոր client-side state֊ի կառավարումը։ __Այն չի նշվում այս միգրացիային ուղեցույցի մեջ__, այնպես որ դուք պետք է նայեք [Vuex 2.0֊ի փաստաթղթերը](https://vuex.vuejs.org/en/index.html) եթե ցանկանում էք իմանալ ավելին։

__Vuex 1.0__֊ը հիմնականում backwards-compatible է, այնպես որ այն պահանջում է մի քանի փոփոխություններ որպեսզի upgrade լինի։ խորհուրդ է տրվում նրանց ովքեր ունեն մեծ կոդային բազաներ կամ ովքեր ցանկանում են ամենահեշտ և հասանելի զարգացման ուղին դեպի Vue 2.0։ Այս ուղեցույցը նվիրված է ներկայացնելու այդ գործընթացը, բայց միայն ներառում է միգրացիոն նշումներ։ Ավելի կազմված օգտագործման ուղեցույցի համար, նայեք [Vuex 1.0 փաստաթղթերը](https://github.com/vuejs/vuex/tree/1.0/docs/en)։

## `store.watch` String Հատկության Ճանապարհ <sup>փոխարինված է</sup>

`store.watch` հիմա միայն ընդհունում է ֆունկցիաներ։ Այնպես որ օրինակի համար, դուք կցանկանաք փոխարինել․

``` js
store.watch('user.notifications', callback)
```

հետևյալով․

``` js
store.watch(
  // Երբ վերադարձված արդյունքը փոփոխվում է...
  function (state) {
    return state.user.notifications
  },
  // Աշխատացրեք այս callback
  callback
)
```

Սա տալիս է ձեզ ավելի ամբողջական կառավարում այն ռեակտիվ հատկություններին որոնք որ դուք ցանկանում էք դիտարկել։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>store.watch</code>֊ի որը պարունակում է string որպես իր առաջին արգումենտ։</p>
</div>
{% endraw %}

## Store֊ի Event Արձակող <sup>ջնջված է</sup>

Store instance֊ը այլևս չունի event արձակող ինտերֆեյսը (`on`, `off`, `emit`)։ Եթե դուք նախկինում օգտագործում էիք store֊ը որպես գլոբալ event֊ների գործակից, [նայեք այս բաժինը](migration.html#dispatch-and-broadcast-removed) միգրացիայի ցուցումների համար։

Փոխարենը օգտագործելով այս ինտերֆեյսը որպեսզի դիտարկել event֊ներ որոնք արձակվել են store֊ի կողմից (օրինակ՝ `store.on('mutation', callback)`), նոր մեթոդ `store.subscribe`֊ն է ներկայացվել։ Սովորական օգտագործումը plugin֊ին ներքո կլիներ այսիպիսին․

``` js
var myPlugin = store => {
  store.subscribe(function (mutation, state) {
    // Մի գործողություն այստեղ...
  })
}

```

Նայեք օրինակը [plugin֊ների փաստաթղթերի](https://github.com/vuejs/vuex/blob/1.0/docs/en/plugins.md) որպեսզի իմանալ ավելին։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>store.on</code>, <code>store.off</code>, և <code>store.emit</code>֊ի վերաբերյալ։</p>
</div>
{% endraw %}

## Middleware֊ներ <sup>փոխարինված է</sup>

Middleware֊ները հիմա փոխարինված են plugin֊ներով։ Plugin-ը ֆունկցիա է որը ստանում է store֊ը որպես միակ արգումենտ, և կարող է լսել mutation event֊ին store֊ում․

``` js
const myPlugins = store => {
  store.subscribe('mutation', (mutation, state) => {
    // Մի գործողություն այստեղ...
  })
}
```

Ավելի մանրամասների համար, նայեք [plugin֊ների փաստաթղթերը](https://github.com/vuejs/vuex/blob/1.0/docs/en/plugins.md)։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper֊ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>middlewares</code> ընտրանքի վերաբերյալ որը գտնվում է store֊ի վրա։</p>
</div>
{% endraw %}
