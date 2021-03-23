---
title: Միգրացիան Vue 1.x-ից
type: guide
order: 701
---

## Հաճախ Տրվող Հարցեր

> Սա շատ երկար էջ է՜ Դա նշանակում է որ Vue 2.0-ը լրիվ այլ բան է, ես պետք է սովորեմ ամեն ինչ նորից, և միգրացիան կլինի պրակտիկորեն անհնարի՞ն։

Ուրախ եմ որ տվեցիր այդ հարցը, պատասխանն է ոչ։ API-ի 90%-ը նույնն է և իր հիմնական հասկացությունները չեն փոփոխվել։ Այն երկար է որովհետև մենք ցանկանում ենք տրամադրել շատ մանրամասն բացատրություններ և ներառել շատ օրինակներ։ Մնացածում համոզված եղեք, __հարկավոր չէ կարդալ ամեն ինչ սկզբից մինչև վերջ__

> Որտեղի՞ց ես կարող եմ սկել միգրացիան

1. Կարող եք սկսել աշխատացնելով [migration helper-ը](https://github.com/vuejs/vue-migration-helper) ձեր նախագծում։ Մենք զգուշորեն minify և compress ենք արել senior Vue ծրագրավորողին դեպի պարզ CLI։ Երբ նրանք ճանաճում են հնացած հատկություն, նրանք ձեզ տեղյակ կպահեն, կառաջարկեն տարբերակներ, և կտրամադրեն հղումներ ավելի մանրամասն զննելու համար։

2. Դրանից հետո, զննեք բովանդակության ցանկը այդ էջի որը գտնվում է sidebar-ում։ Եթե դուք տեսնում եք որևէ թեմա որով որ ձեր ծրագիրը ազդվել է, բայց migration helper-ը չի նկատել, ապա ուսումնասիրեք այն։

3. Եթե դուք ունեք թեսթեր, աշխատացրեք դրանք և նայեք թե ինչը կձախողվի։ Եթե դուք չունեք թեստեր, ուղակի բացեք ծրագիրը ձեր բրաուզերում և ուշադիր եղեք warning-ների կամ error-ների համար։

4. Այժմ, ձեր ծրագիրը պետք է ամբողջովին միգրացված լինի։ Եթե դուք դեռ ցանկանում եք իմանալ ավելին, դուք կարող եք կարդալ մնացածը - կամ սուզվել դեպի նոր և լավացված ուղեցույցը [սկզբից](index.html)։ Շատ մասեր կլինեն դյուրին, երբ որ դուք արդեն ծանոթ կլինեք հիմնական գաղափարների հետ։

> Որքա՞ն երկար կտևի որպեսզի միգրացնել Vue 1.x ծրագիրը դեպի 2.0

Կախված է մի քանի գործոններից․

- Ձեր ծրագրի չափսից (փոքրից մինչև միջին ծավալի ծրագրերը հավանաբար կտևի մեկ օրից քիչ)

- Քանի անգամ եք դուք շեղվում և սկսում խաղալ նոր հատկությունների հետ։ 😉&nbsp;Չենք դատում ձեզ, նույնը եղել է մեզ հետ երբ կառուցում էինք 2.0-ը՜

- Ո՞ր հնացած հատկություններն եք դուք օգտագործում։ Շատերը կարող են թարմացվեն find-and-replace-ով, բայց մնացածը կարող են տանել մի քանի րոպե։ Եթե դուք չեք հետևում լավագույն պրակտիկաներին, Vue 2.0 կփորձի ավելի շատ ստիպել ձեզ։ Սա երկարաժամկետ հեռանկարում լավ բան է, բայց կարող է նշանակել նաև էական (չնայած հնարավոր է՝ ժամկետանց) ռեֆակտոր։

> Եթե ես թարմացնեմ մինչև Vue 2, ես նաև պե՞տք է թարմացնեմ Vuex-ը և Vue Router-ը

Միայն Vue Router 2-ն է համապատասխանում Vue 2-ին, այնպես որ այո, դուք պետք է հետևեք [Vue Router-ի միգրացիայի ճանապարհին](migration-vue-router.html) նույնպես։ Բարեբախտաբար, շատ ծրագրեր չունեն շատ router-ի կոդ, այնպես որ հավանական է որ այն չի տևի ժամից ավել։

Vuex-ի համար, նույնիսկ 0.8-ը համապատասխանում է Vue 2-ին, այնպես որ հարկավոր չէ թարմացնել։ Միակ պատճառը որ դուք ցանկություն ունենաք միանգամից թարմացնելու դա նոր հատկությունների առավելություններն են Vuex 2-ի մեջ, ինչպիսին են մոդուլները և փոքրացված boilerplate—ները։

## Ձևանմուշներ

### Բաժանված Instance-ներ <sup>ջնջված</sup>

Ամեն կոմպոնենտ պետք է ունենա մեկ արմատային էլեմենտ։ Բաժանված instance-ները այլևս չեն թույլատրվում։ Եթե դուք ունեք այսպիսի ձևանմուշ․

``` html
<p>foo</p>
<p>bar</p>
```

Խորհուրդ է տրվում փաթաթել ամբողջ բովանդակությունը նոր էլեմենտի մեջ, այսպես․

``` html
<div>
  <p>foo</p>
  <p>bar</p>
</div>
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Թարմացնելուց հետո աշխատացրեք ձեր end-to-end թեստերը կամ ծրագիրը և ուշադիր եղեք բազմաթիվ արմատային էլեմենտների վերաբերյալ <strong>console-ի warning-ների համար</strong>։</p>
</div>
{% endraw %}

## Lifecycle Hook-եր

### `beforeCompile` <sup>ջնջված է</sup>

Փոխարեն օգտագործեք `created` hook-ը։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի գտնել բոլոր օրինակները այս hook-ի վերաբերյալ։</p>
</div>
{% endraw %}

### `compiled` <sup>փոխարինված է</sup>

Փոխարենը օգտագործեք նոր `mounted` hook-ը։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի գտնել բոլոր օրինակները այս hook-ի վերաբերյալ։</p>
</div>
{% endraw %}

### `attached` <sup>ջնջված է</sup>

Օգտագործեք custom DOM-ի ներքո ստուգում այլ hook-երում։ Օրինակի համար, որպեսզի փոխարինել․

``` js
attached: function () {
  doSomething()
}
```

Դուք կարող եք օգտագործել․

``` js
mounted: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի գտնել բոլոր օրինակները այս hook-ի վերաբերյալ։</p>
</div>
{% endraw %}

### `detached` <sup>ջնջված է</sup>

Օգտագործեք custom DOM-ի ներքո ստուգում այլ hook-երում։ Օրինակի համար, որպեսզի փոխարինել․

``` js
detached: function () {
  doSomething()
}
```

Դուք կարող եք օգտագործել․

``` js
destroyed: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի գտնել բոլոր օրինակները այս hook-ի վերաբերյալ։</p>
</div>
{% endraw %}

### `init` <sup>վերանվանված է</sup>

Փոխարենը օգտագործեք նոր `beforeCreate` hook-ը, որը պարզապես անում է նույն բանը։ Նրա անունը փոփոխվել է որպեսզի համապատասխանի այն lifecycle մեթոդների հետևողականությանը։

{% raw %}
<div class="upgrade-path">
   <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի գտնել բոլոր օրինակները այս hook-ի վերաբերյալ։</p>
</div>
{% endraw %}

### `ready` <sup>փոխարինված է</sup>

Փոխարենը օգտագործեք նոր `mounted` hook-ը։ Պետք է նշվի որ `mounted`-ի հետ, երաշխիք չկա որ այն կլինի փաստաթղթի մեջ։ Այդ պատճառով, նաև ներառեք `Vue.nextTick`/`vm.$nextTick`-ը։ Օրինակի համար․

``` js
mounted: function () {
  this.$nextTick(function () {
    // կոդը ենթադրում է որ this.$el-ը փաստաթղթի մեջ է
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի գտնել բոլոր օրինակները այս hook-ի վերաբերյալ։</p>
</div>
{% endraw %}

## `v-for`

### `v-for` Արգումենտների Հերթականությունը Զանգվածների Համար <sup>փոփոխված է</sup>

Երբ ներառում ենք `index`, արգումենտի հերթականությունը զանգվածների համար նախկինում `(index, value)` էր։ Հիմա այն `(value,index)` է որպեսզի լինել ավելի հեևողական JavaScript-ի հիմքում գտնվող զանգվածի մեթոդների հետ ինչպիսիք են `forEach` և `map`։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի գտնել օրինակներ հնացված արգումենտների հերթականության համար։ Նշում որ եթե դուք անուն դնեք ձեր index արգումենտներին որևէ մի անսովոր բան օրինակ <code>position</code> կամ <code>num</code>, helper-ը չի նկատի նրանց։</p>
</div>
{% endraw %}

### `v-for` Արգումենտի Հերթականությունը Օբյեկտների Համար <sup>փոփոխված է</sup>

Երբ ներառում ենք հատկության անունը/բանալին, արգումենտի հերթականությունը օբյեկտների համար նախկինում `(name, value)` էր։ Հիմա այն `(value, name)` է որպեսզի լինել ավելի հեևողական JavaScript-ի հիմքում գտնվող հիմնական օբյեկտի ցիկլերի հետ ինչպես lodash—ին է։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի գտնել օրինակներ հնացված արգումենտների հերթականությունների համար։ Նշում եթե դուք անուն եք դրել ձեր բանալի արգումենտներին օրինակ <code>name</code> կամ <code>property</code>, helper-ը չի նկատի նրանց։</p>
</div>
{% endraw %}

### `$index` և `$key` <sup>ջնջված են</sup>

Հատուկ նշանակված  `$index` և `$key` փոփոխականները ջնջվել են հստակ սահմանելու օգտին `v-for`-ի մեջ։ Սա դարձնում է կոդը ավելի հեշտ կարդալ ծրագրավորողների համար որոնք շատ փորձ չունեն Vue-ի հետ նաև արդյունքում լինում է ավելի մաքուր և հանգվում ենք բազմաստիճան ցիկլերի հետ։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի գտնել օրինակներ այդ ջնջված փոփոխականների մասին։ Եթե դուք բաց թողեք որևէ մեկը, դուք նաև պետք է նայեք <strong>console-ի error-ները</strong> ինչպիսին են․ <code>Uncaught ReferenceError: $index is not defined</code></p>
</div>
{% endraw %}

### `track-by` <sup>փոխարինված է</sup>

`track-by` փոխարինվել է `key`-ով, որը աշխատում է ինչպես ցանկացած այլ ատրիբուտ․ առանց `v-bind:` կամ `:` prefix—ի, այն վերաբերվում է որպես պարզ string: Շատ դեպքերում, դուք կցանկանաք օգտագործել դինամիկ կապում որը ակնկալում է ամբողջական արտահայտություն բանալիի փոխարեն։ Օրինակի համար, ի փոխարեն․

{% codeblock lang:html %}
<div v-for="item in items" track-by="id">
{% endcodeblock %}

Դուք պետք է հիմա գրեք․

{% codeblock lang:html %}
<div v-for="item in items" v-bind:key="item.id">
{% endcodeblock %}

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի գտնել օրինակներ <code>track-by-ի</code> վերաբերյալ։</p>
</div>
{% endraw %}

### `v-for` Սահմանային Արժեքներ <sup>փոփոխված է</sup>

Նախկինում, `v-fo="number in 10"`-ը `number`-ը կսկսեր 0-ից և կվերջանար 9-ով։ Հիմա այն սկսում է 1-ից և վերջանում է 10-ով։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Փնտրեք ձեր կոդային բազայում regex-ով <code>/\w+ in \d+/</code>։ Երբ այն կհայտնվի <code>v-for</code>-ում, ստուգեք այն որպեսզի տեսնել եթե ազդվել է ծրագիրը նրանցով։</p>
</div>
{% endraw %}

## Prop-ներ

### `coerce` Prop Ընտրանք <sup>ջնջված է</sup>

Եթե դուք ցանկանում եք coerce անել prop-ը, տեղադրեք տեղային հաշվարկված արժեք նրանից կախված։ Օրինակ, ի փոխարեն․

``` js
props: {
  username: {
    type: String,
    coerce: function (value) {
      return value
        .toLowerCase()
        .replace(/\s+/, '-')
    }
  }
}
```

Դուք պետք է գրեք․

``` js
props: {
  username: String,
},
computed: {
  normalizedUsername: function () {
    return this.username
      .toLowerCase()
      .replace(/\s+/, '-')
  }
}
```

Կան մի քանի առավելություններ․

- Դուք դեռ կունենաք հնարավորություն ստանալու սկզբնական արժեքը prop-ի։
- Դուք ստիպված եք լինել ավելի բացահայտ, տալով ձեր coerced արժեքին անուն որը տարբերվում է այն արժեքից որը փոխանցված է prop-ում։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրեք օրինակներ <code>coerce</code> ընտրանքի վերաբերյալ։</p>
</div>
{% endraw %}

### `twoWay` Prop Ընտրանք <sup>ջնջված է</sup>

Prop-ները հիմա միշտ մեկ ճանապարհ ներքև են։ Որպեսզի արտադրենք կողմնակի ազդեցություններ ծնող scope-ում, կոմպոնենտը պետք է ենթադրաբար կախված լինի արձակված event-ի փոխարեն կապումով այսինքն binding-ով։ Ավելին իմանալու համար, նայեք․

- [Custom կոմպոնենտի event-ներ](components.html#Custom-Events)
- [Custom input կոմպոնենտներ](components.html#Form-Input-Components-using-Custom-Events) (օգտագործելով կեմպոնենտի event-ները)
- [Գլոբալ վիճակի կառավարում](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրեք օրինակներ <code>twoWay</code> ընտրանքի վերաբերյալ։</p>
</div>
{% endraw %}

### `.once` և `.sync` modifier—ները `v-bind`-ի վրա <sup>ջնջված են</sup>

Prop-ները հիմա միշտ մեկ ճանապարհ ներքև են։ Որպեսզի արտադրենք կողմնակի ազդեցություններ ծնող scope-ում, կոմպոնենտը պետք է ենթադրաբար կախված լինի արձակված event-ի փոխարեն կապումով այսինքն binding-ով։ Ավելին իմանալու համար, նայեք․

- [Custom կոմպոնենտի event-ներ](components.html#Custom-Events)
- [Custom input կոմպոնենտներ](components.html#Form-Input-Components-using-Custom-Events) (օգտագործելով կեմպոնենտի event-ները)
- [Գլոբալ վիճակի կառավարում](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրեք օրինակներ <code>.once</code> և <code>.sync</code> փոփոխիչների վերաբերյալ։</p>
</div>
{% endraw %}

### Prop-ի Մուտացիա <sup>արժեզրկված է</sup>

Prop-ը մուտացիայի ենթարկելը հիմա համարվում է հակա-pattern, օրինակ հայտարարելով prop և հետո վերագրելով `this.myProp = 'someOtherValue'` կոմպոնենտում։ Rendering-ի մեխանիզմների պատճառով, երբ ծնող կոմպոնենտը re-render է լինում, ժառանգող կոմպոնենտի լոկալ փոփոխությունները կվերագրվեն։

Հաճախ դեպքերում երբ մուտացիայի ենք ենթարկում prop-ը կարող է փոխարինվել հետևյալ ընտրանքներից մեկի հետ։

- տվյալների հատկություն, prop—ի հետ դրված որպես իր հիմնական արժեք
- հաշվարկված հատկություն

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Թարմացնելուց հետո աշխատացրեք ձեր end-to-end թեստերը կամ ծրագիրը և ուշադիր եղեք prop-ի մուտացիայի <strong>console-ում գտնվող warning—ների համար</strong>։</p>
</div>
{% endraw %}

### Prop-ները Արմատի Instance-ում <sup>փոխարինված է</sup>

Արմատի Vue instance—ներում (օրինակ instance—ներ ստեղծված `new Vue({ ... })`—ով), դուք պետք է օգտագործեք `propsData` props-ի փոխարեն։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք ձեր end-to-end թեստերը եթե դուք ունեք։<strong>ձախողված թեստերը</strong> պետք է նախազգուշացնեն ձեզ որ prop-ները փոխանցված արմատի instance-ներում այլևս չեն աշխատում։</p>
</div>
{% endraw %}

## Հաշվարկված հատկություններ

### `cache: false` <sup>արժեզրկված է</sup>

Հաշվարկված հատկությունների անվավեր caching-ը կհանվի Vue-ի ապագա հիմնական տարբերակներում։ Փոխարինեք ցանկացած uncached հաշվարկված հատկությունները մեթոդներով, որոնք կարտադրեն նույն արդյունքը։

Օրինակի համար․

``` js
template: '<p>նամակը: {{ timeMessage }}</p>',
computed: {
  timeMessage: {
    cache: false,
    get: function () {
      return Date.now() + this.message
    }
  }
}
```

Կամ կոմպոնենտի մեթոդների հետ․

``` js
template: '<p>նամակը: {{ getTimeMessage() }}</p>',
methods: {
  getTimeMessage: function () {
    return Date.now() + this.message
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>cache: false</code> ընտրանքի վերաբերյալ։</p>
</div>
{% endraw %}

## Ներքին Ուղղորդիչները

### Ճշմարտությունը/Կեղծիքը `v-bind`-ի հետ <sup>փոփոխված է</sup>

Երբ օգտագործվում է `v-bind`-ի հետ, միակ կեղծ արժեքները հիմա դրանք․ `null`, `undefined`, և `false` են։ Սա նշանակում է `0`-ն և դատարկ string—ները render կլինեն որպես ճշմարիտ։ Այնպես որ օրինակի համար, `v-bind:draggable="''"` render կլինի որպես `draggable="true"`։

Հաշվարկված հատկությունների համար, ի հավելումն վերևում նշված կեղծ արժեքների, string `"false"`-ը նաև render կլինի որպես `attr="false"`։

<p class="tip">Նշում որ այլ ուղղորդիչներ (օրինակ `v-if`-ը և `v-show`—ը), JavaScript-ի հիմնական ճշմարտությունը կկիրառվի։</p>

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք ձեր end-to-end թեստերը եթե դուք ունեք։<strong>ձախողված թեստերը</strong> պետք է նախազգուշացնեն ձեզ ձեր ծրագրում գտնվող ցանկացած մասի վերաբերյալ որը ազդվել է այս փոփոխության պատճառով։</p>
</div>
{% endraw %}

### Լսելով Native Event-ներին Կոմպոնենտների Վրա `v-on`-ի Հետ <sup>փոփոխված է</sup>

երբ օգտագործվում է կոմպոնենտի վրա, `v-on`-ը հիմա միայն լսում է custom event-ներին `$emit` եղած այդ կոմպոնենտի կողմից։ Որպեսզի լսենք native DOM event—ին արմատի էլեմենտի վրա, դուք կարող եք օգտագործել `.native` modifier-ը։ Օրինակի համար․

{% codeblock lang:html %}
<my-component v-on:click.native="doSomething"></my-component>
{% endcodeblock %}

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք ձեր end-to-end թեստերը եթե դուք ունեք։<strong>ձախողված թեստերը</strong> պետք է նախազգուշացնեն ձեզ ձեր ծրագրում գտնվող ցանկացած մասի վերաբերյալ որը ազդվել է այս փոփոխության պատճառով։</p>
</div>
{% endraw %}

### `debounce` Պարամետր Ատրիբուտ `v-model`-ի համար <sup>ջնջված է</sup>

Debouncing-ը օգտագործվում է որպեսզի սահմանափակել թե որքան հաճախ է Ajax հարցումները և այլ թանկ գործողությունները կատարվում։ Vue-ի `debounce` ատրիբուտ պարամետրը `v-model`-ի համար դարձրել է սա հեշտ ավելի պարզ դեպքերում, բայց այն debounce է անում __վիճակի թարմացումները__ քան թանկ գործողությունները։ Դա նուրբ տարբերություն է, բայց տալիս է սահմանափակումներ երբ ծրագիրը աճում է։

Այս սահմանափակումները դառնում են ակնհայտ երբ կառուցում ենք որոնման ցուցիչ, ինչպես այս օրինակում է․

{% raw %}
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.js"></script>
<div id="debounce-search-demo" class="demo">
  <input v-model="searchQuery" placeholder="Type something">
  <strong>{{ searchIndicator }}</strong>
</div>
<script>
new Vue({
  el: '#debounce-search-demo',
  data: {
    searchQuery: '',
    searchQueryIsDirty: false,
    isCalculating: false
  },
  computed: {
    searchIndicator: function () {
      if (this.isCalculating) {
        return '⟳ Fetching new results'
      } else if (this.searchQueryIsDirty) {
        return '... Typing'
      } else {
        return '✓ Done'
      }
    }
  },
  watch: {
    searchQuery: function () {
      this.searchQueryIsDirty = true
      this.expensiveOperation()
    }
  },
  methods: {
    expensiveOperation: _.debounce(function () {
      this.isCalculating = true
      setTimeout(function () {
        this.isCalculating = false
        this.searchQueryIsDirty = false
      }.bind(this), 1000)
    }, 500)
  }
})
</script>
{% endraw %}

Օգտագործելով `debounce` ատրիբուտը, «typing» վիճակը հայտնաբերելու որևէ միջոց չի լինի, որովհետև մենք կորցնում ենք մուտքը դեպի input-ի իրական ժամանակի վիճակ։ Սակայն բաժանելով debounce ֆունկցիան Vue-ից, մենք կարող ենք debounce անել միայն այն գործողությունը որը մենք ցանկանում ենք սահմանափակել, ջնջելով սահմանափակումները այն հատկությունների որոնք մենք զարգացնում ենք․

``` html
<!--
Օգտոգործելով debounce ֆունկցիան lodash-ից կամ այլ առանձին
գործիքների գրադարանից, մենք գիտենք որ կոնկրետ debounce-ի տեղադրումը որը
մենք կօգտագործենք կլինի լավագույնը - և մենք կարող ենք օգտագործել այն ՑԱՆԿԱՑԱԾ ՏԵՂ։
Եվ ոչ միայն մեր ձևանմուշներում։
-->
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.js"></script>
<div id="debounce-search-demo">
  <input v-model="searchQuery" placeholder="Type something">
  <strong>{{ searchIndicator }}</strong>
</div>
```

``` js
new Vue({
  el: '#debounce-search-demo',
  data: {
    searchQuery: '',
    searchQueryIsDirty: false,
    isCalculating: false
  },
  computed: {
    searchIndicator: function () {
      if (this.isCalculating) {
        return '⟳ Fetching new results'
      } else if (this.searchQueryIsDirty) {
        return '... Typing'
      } else {
        return '✓ Done'
      }
    }
  },
  watch: {
    searchQuery: function () {
      this.searchQueryIsDirty = true
      this.expensiveOperation()
    }
  },
  methods: {
    // This is where the debounce actually belongs.
    expensiveOperation: _.debounce(function () {
      this.isCalculating = true
      setTimeout(function () {
        this.isCalculating = false
        this.searchQueryIsDirty = false
      }.bind(this), 1000)
    }, 500)
  }
})
```

Մեկ այլ առավելություն այս մոտեցման մեջ դա որ կլինի ժամանակներ երբ debouncing-ը չի լինի ճիշտ փաթաթվող ֆունկցիան։ Օրինակի համար, երբ հարցում ենք կատարում API-ին որոնման առաջարկների համար, սպասելով առաջարկներին մինչև օգտագործողը դադարեցրել է գրելը որոշ ժամանակով միշտ լավագույն փորձը չէ։ Այն ինչ որ դուք պետք է փոխարենը անեք դա __throttling__ ֆունկցիայի օգտագործումն է։ Հիմա երբ դուք արդեն օգտագործում եք գործիքների գրադարան ինչպիսին է lodash-ը, գրելով `throttle` ֆունկցիան որպեսզի փոխարինենք հինը ընդհամենը տևում է մի քանի վայրկյան։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>debounce</code> ատրիբուտի վերաբերյալ։</p>
</div>
{% endraw %}

### `lazy` կամ `number` Պարամետր Ատրիբուտներ `v-model`-ի համար <sup>փոխարինված</sup>

`lazy` և `number` պարամետր ատրիբուտները հիմա միայն modifier-ներ են, որպեսզի դարձնենք այն ավելի մաքուր մենք պետք է փոխարենը գրելով․

``` html
<input v-model="name" lazy>
<input v-model="age" type="number" number>
```

Օգտագործենք սա․

``` html
<input v-model.lazy="name">
<input v-model.number="age" type="number">
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ այս պարամետր ատրիբուտների վերաբերյալ։</p>
</div>
{% endraw %}

### `value` Ատրիբուտը `v-model`-ի հետ <sup>ջնջված է</sup>

`v-model`-ը այլևս կախված չէ inline `value` ատրիբուտի սկզբնական արժեքից։ Կանխատեսելիության համար, այն փոխարենը միշտ կվարվի Vue instance-ի տվյալի հետ որպես ճշմարիտ սկզբնաղբյուր։

Սա նշանակում է որ այս էլեմենտը․

``` html
<input v-model="text" value="foo">
```

աջակցված է այս տվյալով․

``` js
data: {
  text: 'bar'
}
```

Այն render կլինի «bar»-ի արժեքի փոխարեն «foo»։ Նույնը կլինի `<textarea>`-ի համար նույն բովանդակության հետ հանդերձ։ Ի փոխարեն․

``` html
<textarea v-model="text">
  բարև աշխարհ
</textarea>
```

Դուք պետք է համոզվեք որ սկզբնական արժեքը `text`-ի դա «բարև աշխարհ» է։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about inline value attributes with <code>v-model</code>.</p>
  <p>Թարմացնելուց հետո աշխատացրեք ձեր end-to-end թեստերը կամ ծրագիրը և ուշադիր եղեք <strong>console-ի նախազգուշացումների</strong> համար որոնք վերաբերվում են inline արժեքների ատրիբուտների օգտագործումը <code>v-model-ի</code> հետ։</p>
</div>
{% endraw %}

### `v-model`-ը `v-for`-ի հետ Ընթացիկ Պարզ Արժեքներ <sup>ջնջված է</sup>

Այսպիսի դեպքերը այլևս չեն աշխատի․

``` html
<input v-for="str in strings" v-model="str">
```

Պատճառը դա որ այն հավասար է JavaScript-ի `<input>`-ին որին այն compile կլինի․

``` js
strings.map(function (str) {
  return createElement('input', ...)
})
```

ԻՆչպես դուք տեսնում եք, `v-model`-ի երկու ճանապարհ կապը իմաստ չունի այստեղ։ Դնելով `str` այլ արժեքին ներքին ֆունկցիայում ոչինչ չի լինի քանի որ միայն լոկալ փոփոխական է ֆունկցիայի scope-ում։

Փոխարենը, դուք պետք է օգտագործեք զանգվածներ բաղկացած __օբյեկտներից__ այնպես որ `v-model`-ը կկարողանա թարմացնել դաշտերը օբյեկտում։ Օրինակի համար․

{% codeblock lang:html %}
<input v-for="obj in objects" v-model="obj.str">
{% endcodeblock %}

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Եթե ունեք թեստեր, աշխատացրեք դրանք։ <strong>Ձախողված թեստերը</strong> պետք է ձեզ զգուշացնեն ձեր ծրագրի ցանկացած մասերից որոնք ազդվել են այս փոփոխությունից։</p>
</div>
{% endraw %}

### `v-bind:style`-ը Օբյեկտի Գրելաձևի հետ և `!important` <sup>ջնջված է</sup>

Սա այլևս չի աշխատի․

``` html
<p v-bind:style="{ color: myColor + ' !important' }">բարև</p>
```

Եթե դուք իրականում ցանկանում եք վերագրել այն մեկ այլ `!important`-ով, դուք պետք է օգտագործեք string-ի գրելաձևը․

``` html
<p v-bind:style="'color: ' + myColor + ' !important'">բարև</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղին</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ ոճային կապումների որոնք ունեն <code>!important</code> օբյեկտներում։</p>
</div>
{% endraw %}

### `v-el` և `v-ref` <sup>փոխարինված է</sup>

Պարզության համար, `v-el` և `v-ref` ձուլվել են դեպի `ref` ատրիբուտ, հասանելի լինելով կոմպոնենտի instance-ում որպես `$refs`։ Սա նշանակում է `v-el:my-element` կդառնա `ref="myElement"` և `v-ref:my-component` կդառնա `ref="myComponent"`։ Երբ օգտագործվում է հասարակ էլեմենտի վրա, `ref` ատրիբուտը կլինի DOM էլեմենտը, և երբ օգտագործվում է կոմպոնենտի վրա, `ref`-ը կլինի կոմպոնենտի instance-ը։

Մինչ `v-ref`-ը այլևս directive չէ, բայց ունի հատուկ ատրիբուտ, այն նաև կարող է դինամիկորեն հայտարարվել։ Սա հատկապես օգտակար է `v-for`-ի հետ համատեղ։ Օրինակի համար․

``` html
<p v-for="item in items" v-bind:ref="'item' + item.id"></p>
```

Նախկինում, `v-el`/`v-ref`-ը համատեղ `v-for`-ի հետ կարտադրեր զանգված բաղկացած էլեմենտներից/կոմպոնենտներից, որովհետև ոչ մի հնարավորություն չկար որպեսզի տալ ամեն էլեմենտին հատուկ անուն։ Դուք կարող եք դեռ հասնել այս behavior-ին ամեն մեկին տալով նույն `ref`-ը․

``` html
<p v-for="item in items" ref="items"></p>
```

Ի տարբերություն 1.x-ի մեջ, այս `$ref`-ները այլևս ռեակտիվ չեն, որովհետև նրանք գրանցվում/թարմացվում են render պրոցեսի ժամանակ։ Դարձնելով նրանց ռեակտիվ ձեզանից կպահանջվի կրկնօրինակել render-ներ ամեն փոփոխության ժամանակ։

Մյուս կողմից, `$refs`-ը  ստեղծված է հիմնականում ծրագրային մուտքի համար JavaScript-ում - խորհուրդ չի տրվում կախված լինել նրանցով ձևանմուշների մեջ, որովհետև նրանցով դուք կդիմեք ընդհանուր state-ին որը չի պատկանում instance—ին։ Սա կխախտի Vue-ի տվյալներով շարժվող view model-ը։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման Ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>v-el-ի</code> և <code>v-ref-ի</code> վերաբերյալ։</p>
</div>
{% endraw %}

### `v-else`-ը `v-show`-ի հետ <sup>ջնջված է</sup>

`v-else`-ը այլևս չի աշխատում `v-show`-ի հետ։ Փոխարենը օգտագործեք `v-if`-ը հակասող արտահայտությամբ։ Օրինակի համար, փոխարենը․

``` html
<p v-if="foo">Foo</p>
<p v-else v-show="bar">Not foo, but bar</p>
```

Դուք կարող եք օգտագործել․

``` html
<p v-if="foo">Foo</p>
<p v-if="!foo && bar">Not foo, but bar</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման Ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել <code>v-else-ը</code> <code>v-show-ի</code> հետ օգտագործման օրինակներ։</p>
</div>
{% endraw %}

## Custom Ուղղորդիչներ <sup>պարզեցված է</sup>

Ուղղորդիչները ունեն փոքրացված պատասխանատվության scope․ նրանք հիմա միայն օգտագործվում են որպեսզի կիրառել ցածր աստիճանի ուղիղ DOM մանիպուլացիաներ։ Շատ դեպքերում, դուք պետք է նախընտրեք օգտագործելու կոմպոնենտները որպես հիմնական կոդի վերօգտագործման աբստրակցիա։

Որոշ հաճախ նշվող տարբերությունները ներառում են․

- Ուղղորդիչները այլևս չունեն instance—ներ։ Սա նշանակում է որ `this`-ի հասանելի չէ ուղղորդիչի hook-երում։ Փոխարենը, նրանք կստանան այն ինչ որ պետք է որպես արգումենտ։ Եթե դուք իրականում ցանկանում պահպանել state-ը hook-երում, դուք կարող եք անել դա `el`-ում։
- Ընտրանքներ ինչպիսին են `acceptStatement`, `deep`, `priority` և մնացածը, նրանք բոլորը ջնջված են։ Որպեսզի փոխարինել `twoWay` ուղղորդիչները, նայեք [այս օրինակը](#Two-Way-Filters-replaced)։
- Որոշները այս hook-երի ունեն տարբեր behavior-ներ և կան նաև մի քանի նոր hook-եր։

Հաջողաբար, մինչ այս նոր ուղղորդիչները ավելի պարզ են, դուք կարող եք ամբողջովին հասկանալ նրանց գործողությունները ավելի հեշտ։ Կարդացեք նոր [Custom Ուղղորդիչների ուղեցույցը](custom-directive.html) որպեսզի իմանալ ավելին։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման Ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ հայտարարված ուղղորդիչների մասին։ Helper-ը կնշի բոլորը, և ինչպես հաճախ դեպքերում դուք կցանկանաք ռեֆատոր անդել դեպի կոմպոնենտ։</p>
</div>
{% endraw %}

### Ուղղորդիչ `.literal` Փոփոխչիը <sup>ջնջված է</sup>

`.literal` փոփոխիչը ջնջվել է, նույնին կարող եք հեշտությամբ հասնել տրամադրելով string արժեքին։

Օրինակի համար, դուք կարող եք թարմացնել․

``` html
<p v-my-directive.literal="foo bar baz"></p>
```

դեպի․

``` html
<p v-my-directive="'foo bar baz'"></p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման Ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել `.literal` փոփոխիչի օգտագործման օրինակներ ուղղորդիչի վրա։</p>
</div>
{% endraw %}

## Անցումներ

### `transition` ատրիբուտը <sup>փոխարինված է</sup>

 Vue-ի անցման համակարգը փոփոխվել է շատ և հիմա օգտագործում է `<transition>` և `<transition-group>` էլեմենտնի  wrapper-ները, քան `transition` ատրիբուտը։ Խորհուրդ է տրվում կարդալ նոր [Անցման ուղեցույցը](transitions.html) որպեսզի իմանալ ավելին։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման Ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել <code>transition</code> ատրիբուտի օգտագործման օրինակներ։</p>
</div>
{% endraw %}

### `Vue.transition` Վերօգտագործվող Անցումների համար <sup>փոխարինված է</sup>

Նոր անցման համակարգի հետ, դուք կարող եք հիմա [օգտագործել կոմպոնենտներ անցումների վերօգտագործման համար](transitions.html#Reusable-Transitions)։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման Ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>Vue.transition</code>-ի վերաբերյալ։</p>
</div>
{% endraw %}

### Անցման `stagger` ատրիբուտը <sup>ջնջված է</sup>

Եթե ձեզ պետք է stagger անել անցումների ցանկը, դուք կարող եք կառավարել ժամանակը տեղադրելով և մուտք գործելով `data-index` (կամ նման ատրիբուտի մեջ) էլեմենտի վրա։ Նայեք [օրինակը այստեղ](transitions.html#Staggering-List-Transitions)։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման Ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>transition</code> ատրիբուտի վերաբերյալ։ Թարմացման ժամանակ, դուք կարող եք նույնպես անցում կատարել դեպի նոր կողմնորոշիչ ստրատեգիա։</p>
</div>
{% endraw %}

## Event-ներ

### `events` ընտրանք <sup>ջնջված է</sup>

`events`-ը ընտրանքը ջնջվել է։ Event-ի handler-ները հիմա պետք է գրանցվեն `created` hook-ում փոխարենը։ Նայեք [`$dispatch` և `$broadcast` միգրացիայի որղեցույցը](#dispatch-and-broadcast-replaced) ավելի մանրամասն օրինակի համար։

### `Vue.directive('on').keyCodes` <sup>փոխարինված է</sup>

Նոր, ավելի հակիրճ ճանապարհը կառավարելու `keyCodes`-ը դա `Vue.config.keyCodes`-ի միջոցով է։ Օրինակի համար․

``` js
// միացնել v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```
{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման Ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ հին <code>keyCode</code> կոնֆիգուրացիոն գրելաձևի վերաբերյալ։</p>
</div>
{% endraw %}

### `$dispatch` և `$broadcast` <sup>փոխարինված է</sup>

`$dispatch` և `$broadcast`-ը ջնջվել են օգուտ ավելի պարզ կոմպոնենտից-կոմպոնենտ կապի և ավելի պահպանվող վիչակի կառավարման լուծումների համար, ինչպիսին [Vuex](https://github.com/vuejs/vuex) է։

Խնդիրը դա event-ի հոսքն է որը կախված է կոմպոնենտի ծառի կառուցվածքի հետ, որի հետ կարող է դժվար լինել աշխատելը և ավելի բարդ է դառնում երբ այն մեծանում է։ Նրանք լավ չեն կազմվում իրար հետ և կարող է ավելի բարդություններ ստեղծել հետագայում։ `$dispatch` և `$broadcast`-ը նաև չեն լուծում կապի խնդիրները հերևան կոմպոնենտների միջև։

Հաճախ դեպքերից մեկում երբ օգտագործում ենք այս մեթոդները որ թույլ տանք ծնողին կապնվել իր ուղիղ ժառանգողի հետ։ Շատ դեպքերում, դուք կարող եք [լսել `$emit`-ին ժառանգողից `v-on`-ի շնորհիվ](components.html#Form-Input-Components-using-Custom-Events)։ Սա թույլ է տալիս ձեզ որպեսզի պահպանել հարմարությունը event-ների ավելացված հետևողականության շնորհիվ։

Սակայն, երբ կապնվում ենք ժառանգողների/ծնողների միջև, `$emit`-ը չի օգնի ձեզ։ Փոխարենը, հասարակ հնարավոր զարգացումը կլինի օգտագործումը կենտրոնացած event—ի կետի։ Սա կարող է ավելացնել օգուտը որը ձեզ թույլ է տալիս կապ հաստատել կոմպոնենտների միջև առանց նայելու թե որտեղ են նրանք կոմպոնենտի ծառում - նույնիսկ եթե նրանք հարևաններ են՜ որովհետև Vue—ի instance—ները ներառում են event արձակող interface, դուք կարող եք օգտագործել դատարկ Vue-ի instance այս պատճառով։

Օրինակի համար, եկեք պատկերացնենք որ մենք ունենք todo ծրագիր կազմված այսպես․

```
Todos
├─ NewTodoInput
└─ Todo
   └─ DeleteTodoButton
```

Մենք կարող ենք կառավարել կապը կոմպոնենտների միջև այս event-ների կետում․

``` js
// Սա է event-ների կետը որը մենք կօգտագործենք ամեն
// կոմպոնենտում որպեսզի կապ հաստատենք նրանց միջև։
var eventHub = new Vue()
```

Այնուհետև մեր կոմպոնենտներում, մենք կարող ենք օգտագործել `$emit`, `$on`, `$off` որպեսզի արձակել event-ներ, լսել event-ներին, և մաքրել event-ի լսողներին։

``` js
// NewTodoInput
// ...
methods: {
  addTodo: function () {
    eventHub.$emit('add-todo', { text: this.newTodoText })
    this.newTodoText = ''
  }
}
```

``` js
// DeleteTodoButton
// ...
methods: {
  deleteTodo: function (id) {
    eventHub.$emit('delete-todo', id)
  }
}
```

``` js
// Todos
// ...
created: function () {
  eventHub.$on('add-todo', this.addTodo)
  eventHub.$on('delete-todo', this.deleteTodo)
},
// Լավ կլինի որպեսզի մաքրել event-ի լսողներին նախքան
// կոմպոնենտը վերացվել է։
beforeDestroy: function () {
  eventHub.$off('add-todo', this.addTodo)
  eventHub.$off('delete-todo', this.deleteTodo)
},
methods: {
  addTodo: function (newTodo) {
    this.todos.push(newTodo)
  },
  deleteTodo: function (todoId) {
    this.todos = this.todos.filter(function (todo) {
      return todo.id !== todoId
    })
  }
}
```

Այս pattern-ը կարող է մատուցվել որպես փոխարինում `$dispatch` և `$broadcast`-ի համար հասարակ դեպքերում, բայց ավելի բարդ դեպքերում, խորհուրդ է տրվում օգտագործել առանձին վիճակի կառավարման շերտ ինչպիսին է [Vuex-ը](https://github.com/vuejs/vuex)։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման Ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>$dispatch</code> և <code>$broadcast</code>-ի վերաբերյալ։</p>
</div>
{% endraw %}

## Ֆիլտրներ

### Ֆիլտրները Տեքստային Կապումից Դուրս <sup>ջնջված է</sup>

Ֆիլտրները հիմա կարղ են միայն օգտագործվել տեքստային կապման մեջ (`{% raw %}{{ }}{% endraw %}` tag-եր)։ Նախկինում մենք օգտագործում էինք ֆիլտրները ուղղորդիչների միջև ինչպիսին են `v-model`, `v-on`, և այլն, որը մեզ հանգեցրեց ավելի բարդությանը քան հարմարությանը։ Ցանկի համար որը կֆիլտրի `v-for`-ի ժամանակ, նաև ավելի լավ կլինի տեղափոխել այդ տրամաբանությունը դեպի JavaScript որպես հաշվարկված հատկություններ, այնպես որ այն կլինի վերօգտագործելի ձեր կոմպոնենտում։

Հիմնականում, երբ ինչ որ մի բան կարող է լինել հասարակ JavaScript-ում, մենք ցանկանում ենք խոուսափել ներկայացնելուց հատուկ գրելաձև ինչպես ֆիլտրերնեն որպեսզի հոգ տանել նույն մտահոգության մասին։ Այստեղ է թէ ինչպես դուք կարող եք փոխարինել Vue-ի ներքինում կառուցված ուղղորդիչների ֆիլտրները։

#### Փոխարինելով `debounce` Ֆիլտերը

Փոխարենը․

``` html
<input v-on:keyup="doStuff | debounce 500">
```

``` js
methods: {
  doStuff: function () {
    // ...
  }
}
```

Օգտագործեք [lodash-ի `debounce`-ը](https://lodash.com/docs/4.15.0#debounce) (կամ հավանականորեն [`throttle`](https://lodash.com/docs/4.15.0#throttle)) որպեսզի ուղիղ սահմանափակել թանկ մեթոդի կանչը։ Դուք կարող եք հասնել նույնին ինչ վերևում է այսպես․

``` html
<input v-on:keyup="doStuff">
```

``` js
methods: {
  doStuff: _.debounce(function () {
    // ...
  }, 500)
}
```

Այս ստրատեգիայի ավելի առավելությունների համար, նայեք [այս օրինակը այստեղ `v-model`-ի հետ](#debounce-Param-Attribute-for-v-model-removed)։

#### Փոխարինումը `limitBy` Ֆիլտերի

Ի Փոխարեն․

``` html
<p v-for="item in items | limitBy 10">{{ item }}</p>
```

Օգտագործեք JavaScript-ի ներքին [`.slice` մեթոդը](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples) հաշվարկված հատկությունում․

``` html
<p v-for="item in filteredItems">{{ item }}</p>
```

``` js
computed: {
  filteredItems: function () {
    return this.items.slice(0, 10)
  }
}
```

#### `filterBy`-ի Փոխարինումը

Ի Փոխարեն․

``` html
<p v-for="user in users | filterBy searchQuery in 'name'">{{ user.name }}</p>
```

Օգտագործեք JavaScript-ի ներքին [`.filter` մեթոդը](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter#Examples) հաշվարկված հատկությունում․

``` html
<p v-for="user in filteredUsers">{{ user.name }}</p>
```

``` js
computed: {
  filteredUsers: function () {
    var self = this
    return self.users.filter(function (user) {
      return user.name.indexOf(self.searchQuery) !== -1
    })
  }
}
```

JavaScript-ի ներքին `.filter`-ը կարող է նաև կառավարել ավելի բարդ ֆիլտրացման գործողությունները, որովհետև դուք ունեք JavaScript-ի ամբողջ ուժը հաշվարկված հատկության մեջ։ Օրինակի համար, եթե դուք ցանկանում եք փնտրել բոլոր ակտիվ օգտատերերին և չնայելով մեծատառ կամ փոքրատառ լինելով նրանց անունը և էլ-փոստը․

``` js
var self = this
self.users.filter(function (user) {
  var searchRegex = new RegExp(self.searchQuery, 'i')
  return user.isActive && (
    searchRegex.test(user.name) ||
    searchRegex.test(user.email)
  )
})
```

#### `orderBy`-ի Փոխարինումը

Ի Փոխարեն․

``` html
<p v-for="user in users | orderBy 'name'">{{ user.name }}</p>
```

Օգտագործեք [lodash-ի `orderBy`-ը](https://lodash.com/docs/4.15.0#orderBy) (կամ հավանականորեն [`sortBy`](https://lodash.com/docs/4.15.0#sortBy)) հաշվարկված հատկությունում․

``` html
<p v-for="user in orderedUsers">{{ user.name }}</p>
```

``` js
computed: {
  orderedUsers: function () {
    return _.orderBy(this.users, 'name')
  }
}
```

Դուք կարող եք նույնիսկ դասավորել ըստ բազմաթիվ սյուների․

{% codeblock lang:js %}
_.orderBy(this.users, ['name', 'last_login'], ['asc', 'desc'])
{% endcodeblock %}

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման Ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ օգտագործված ֆիլտերերի վերաբերյալ որոնք գտնվում են ուղղորդիչների մեջ։ Եթե դուք բաց եք թողել որևէ մեկը, դուք պետք է նաև նայեք <strong>console-ի error-ները</strong>։</p>
</div>
{% endraw %}

### Ֆիլտրի Արգումենտի Գրելաձևը <sup>փոփոխված է</sup>

Ֆիլտրների արգումենտների գրելաձևը հիմա ավելի լավ է հավասարվում JavaScript—ի ֆունկցիաների կանչին։ Այնպես որ փոխարեն վերցնելով բացատներով կազմված արգումենտները։

``` html
<p>{{ date | formatDate 'YY-MM-DD' timeZone }}</p>
```

Մենք շրջապատում ենք արգումենտները փակագծերով և առանձնացնում ենք արգումենտները ստորակետներով․

``` html
<p>{{ date | formatDate('YY-MM-DD', timeZone) }}</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման Ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ հին ֆիլտրների գրելաձևերի վերաբերյալ։ Եթե բաց թողնեք որևէ մեկը, դուք պետք է նայեք <strong>console-ի error-ները</strong>։</p>
</div>
{% endraw %}

### Ներքին Տեքստային Ֆիլտները <sup>ջնջված է</sup>

Չնայած որ ֆիլտները տեքստային կազմվածքում դեռ թույլատրվում են, այդ բոլոր ֆիլտրները ջնջվել են։ Փոխարենը, խորհուրդ է տրվում օգտագործել ավելի հատուկ գրադարաններ որպեսզի լուծել այդ խնդիրները ամեն տեղում (օրինակ՝ [`date-fns`](https://date-fns.org/) որպեսզի կազմել ամսաթվերը և [`accounting.js-ի`](http://openexchangerates.github.io/accounting.js/) արժույթների համար)։

Ամեն Vue-ի ներքին տեքստային ֆիլտների համար, մենք մանրամասնում ենք թե ինչպես նրանց կարող եք դուք փոխարինել։ Կոդի օրինակը կարող է գոյություն ունենալ custom helper ֆունկցիաներում, մեթոդներում, կամ հաշվարկված հատկություններում։

#### `json` Ֆիլտրի Փոխարինումը

Ձեզ այլևս պետք չի գալու debug անելու անհրաժեշտությունը, որովհետև Vue-ն ավտոմատ կերպով format է անում ելքագրումը ձեզ համար, առանց նայելու եթե այն string է, թիվ, զանգված կամ պարզ օբյեկտ։ Եթե դուք ցանկանում եք նույնը ինչ JavaScript-ի `JSON.stringify`-ն է, ուրեմն դուք պետք է օգտագործեք այն մեթոդի կամ հաշվարկված հատկության մեջ։

#### `capitalize` Ֆիլտրի Փոխարինումը

``` js
text[0].toUpperCase() + text.slice(1)
```

#### `uppercase` Ֆիլտրի Փոխարինումը

``` js
text.toUpperCase()
```

#### `lowercase` Ֆիլտրի Փոխարինումը

``` js
text.toLowerCase()
```

#### `pluralize` Ֆիլտրի Փոխարինումը

[Pluralize](https://www.npmjs.com/package/pluralize) փաթեթը NPM—ում հիանալի է ծառայում է այս նպատակի համար, բայց եթե դուք միայն ցանկանում եք օգտագործել pluralize հատուկ բառի կամ եթե դուք ունեք հատուկ ելքագրում ինչպիսին է `0`, ուրեմն դուք նաև պետք է հեշտորեն հայտարարեք ձեր pluralize ֆունկցիաները։ Օրինակի համար․

``` js
function pluralizeKnife (count) {
  if (count === 0) {
    return 'no knives'
  } else if (count === 1) {
    return '1 knife'
  } else {
    return count + 'knives'
  }
}
```

#### `currency` Ֆիլտերի Փոխարինումը


Շատ միամիտ իրագործման համար կարող էիք նման բան անել։

{% codeblock lang:js %}
'$' + price.toFixed(2)
{% endcodeblock %}

Չնայած շատ դեպքերում, դուք կտեսնեք շատ տարօրինակ արդյունքներ (օրինակ՝ `0.035.toFixed(2)` կլորանում է դեպի `0.04`, բայց `0.045` կլորանում է դեպի `0.04`)։ Որպեսզի շրջանցեք այս խնդիրները, դուք պետք է օգտագործեք [`accounting`](http://openexchangerates.github.io/accounting.js/) գրադարանը որպեսզի ավելի ճիշտ format անեք արժույթները։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ հին տեքստային ֆիլտերների։ Եթե դուք բաց եք թողել որևէ մեկը, դուք պետք է նաև նայեք <strong>console-ի error-ները</strong>։</p>
</div>
{% endraw %}

### Երկուղի Ֆիլտերներ <sup>փոխարինված</sup>

Որոշ օգտագործողներ վայելել են երկուղի ֆիլտերները `v-model`-ի հետ որպեսզի ստեղծել հետաքրքիր մուտքագրումներ ավելի քիչ կոդով։ Երբ _թվում է_ որ ավելի պարզ է, երկուղի ֆիլտերները կարող են նաև թաքցնել շատ մեծ բարդություն - և նույնիսկ խրախուսել վատ UX-ը հետաձգելով state—ի թարմացումները։ Փոխարենը, կոմպոնենտները որոնք փաթաթվում են input-ի վրա խորհուրդ են տրվում որպես ավելի հստակ և հատկություններով հարուստ ուղի custom input-ներ պատրաստելու համար։

Որպես օրինակ, մենք հիմա կմանրամասնենք միգրացիան երկուղի արժույթի ֆիլտրների համար․

<iframe src="https://codesandbox.io/embed/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-10-two-way-currency-filter?codemirror=1&hidedevtools=1&hidenavigation=1&theme=light" style="width:100%; height:300px; border:0; border-radius: 4px; overflow:hidden;" title="vue-20-template-compilation" allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

Հաճախ այն աշխատում է լավ, բայց հետաձգված state—ի թարմացումները կարող են հանգեցնել տարօրինակ արդյունքների։ Օրինակի համար, մուտքագրելով `9.999` ձեր input-ներից որևէ մեկի մեջ։ Երբ input-ը կորցնում է focus-ը, նրա արժեքը կթարմացվի դեպի `$10.00`։ Երբ նայում ենք ընդհանուր հաշվարկված արժեքին, դուք կտեսնեք որ `9.999`-ը դա այն է ինչ պահպանված է մեր տվյալներում։ Տարբերակը որը օգտագործողը տեսնում է սինխռոնիզացված չէ։

Որպեսզի սկսել շարժվել դեպի ավելի ամուր լուծման օգտագործելով Vue 2.0-ը, եկեք փաթաթենք այս ֆիլտերը նոր `<currency-input>` կոմպոնենտում․

<iframe src="https://codesandbox.io/embed/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-10-two-way-currency-filter-v2?codemirror=1&hidedevtools=1&hidenavigation=1&theme=light" style="width:100%; height:300px; border:0; border-radius: 4px; overflow:hidden;" title="vue-20-template-compilation" allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

Սա թույլ է տալիս մեզ ավելացնել փոփոխություն առանց որի ֆիլտերը չի կարող մեկուսացնել, ինչպիսին է բովանդակության նշումը երբ input-ը focus է լինում։ Հիմա հաջորդ քայլը կլինի բիզնեսի տրամաբանության առանձնացումը ֆիլտերից։ Ներքևում, մենք կտանենք ամեն ինչ դեպի արտաքին [`currencyValidator` օբյեկտ](https://gist.github.com/chrisvfritz/5f0a639590d6e648933416f90ba7ae4e)․

<iframe src="https://codesandbox.io/embed/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-10-two-way-currency-filter-v3?codemirror=1&hidedevtools=1&hidenavigation=1&theme=light" style="width:100%; height:300px; border:0; border-radius: 4px; overflow:hidden;" title="vue-20-template-compilation" allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

Այս զարգացված մոդուլարությունը ոչ միայն դարձնում է ավելի հեշտ դեպի Vue 2 միգրացիոն պրոցեսը, բայց նաև թույլ է տալիս մեզ որպեսզի արժույթի parse-ը և format-ը լինել․

- unit թեստված մեկուսացման մեջ առանձին ձեր Vue կոդից
- օգտագործվել այլ մասերի կողմից որոնք գտնվում են ձեր ծրագրում, ինչպիսին է payload-ի վավերացումը և ստացված API—ի endpoint-ը։

Ունենալով այս validator-ը առանձնացված վիճակում, մենք նաև կարող ենք ավելի հարմարավետ կերպով զարգացնել այն դեպի ավելի կայուն լուծման։ State-ի մասնիկները հեռացվել են և անհնարին է որ օգտագործողները մուտքագրեն որևէ սխալ բան, նման լինելով բրաուզերի ներքին number input-ին։

Մենք սակայն դեռ սահմանափակված ենք, ֆիլտերներով և Vue 1.0-ով հիմնականում, այնպես որ եկեք ավարտենք դեպի Vue 2.0—ի զարգացումը․

<iframe src="https://codesandbox.io/embed/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-two-way-currency-filter?codemirror=1&hidedevtools=1&hidenavigation=1&theme=light" style="width:100%; height:300px; border:0; border-radius: 4px; overflow:hidden;" title="vue-20-template-compilation" allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

Դուք հնարավոր է որ կնկատեք․

- Ամեն մասը մեր input-ի ավելի հստակ է, օգտագործելով lifecycle hook-երը և DOM event-ները որտեղ նրա երկուղի ֆիլտերները թաքցված են։
- Մենք հիմա կարող ենք օգտագործել `v-model`-ը ուղիղ մեր custom input-ներում, որը ոչ միայն ավելի հարմարավետ է հասարակ input—ների հետ, բայց նաև նշանակում է որ մեր կոմպոնենտը Vuex-ի հետ համատեղելի է։
- Մինչ մենք այլևս չենք կարող օգտագործել ֆիլտերի ընտրանքները որոնք պահանջում են որպեսզի արժեք վերադարձվի, մեր արժույթի աշխատանքը կարող է կատարվել ասինխռոն։ Դա նշանակում է եթե մենք ունենաինք շատ ծրագրեր որոնք պետք է աշխատեն արժույթների հետ, մենք կարող ենք հեշտորեն ռեֆակտոր անել այս տրամաբանությունը դեպի բաժանված միկրոսերվիզ։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ ֆիլտրների որոնք օգտագործվում են ուղղորդիչներում ինչպիսին է <code>v-model</code>-ը։ Եթե դուք բաց թողնեք որևէ մեկը, դուք պետք է նաև նայեք <strong>console-ի error-ները</strong>։</p>
</div>
{% endraw %}

## Սլոտներ

### Կրկնօրինակված Սլոտները <sup>ջնջված են</sup>

Այլևս հասանելի չէ ունենալ `<slot>`-ներ նույն անունով նույն ձևանմուշի մեջ։ Երբ սլոտը render է լինում այն «օգտագործվում է» և չի կարող render լինել որևէ այլ տեղ նույն render-ի ծառի մեջ։ Եթե դուք պետք է render անեք նույն բովանդակությունը մի քանի տեղում, փոխանցեք բովանդակությունը որպես prop:

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Թարմացնելուց հետո աշխատացրեք ձեր end-to-end թեստերը կամ ծրագիրը և ուշադիր եղեք <strong>console-ի warning-ների համար</strong> որոնք վերաբերվում են կրկնօրինակված սլոտներին <code>v-model</code>-ում։</p>
</div>
{% endraw %}

### `slot` Ատրիբուտի Styling-ը <sup>ջնջված է</sup>

Բովանդակությունը որը տեղադրվել է անվանված `<slot>`-ում այլևս չի պահպանում `slot` ատրիբուտը։ Օգտագործեք փաթաթող էլեմենտ որպեսզի ոճավորել այն, կամ ավելի զարգացված օգտագործման համար, փոփոխեք տեղադրված բովանդակությունը ծրագրային կերպով օգտագործելով [render ֆունկցիաները](render-function.html):

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղ</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել CSS selector-ները ուղված դեպի սլոտներին (օրինակ՝ <code>[slot="my-slot-name"]</code>)։</p>
</div>
{% endraw %}

## Հատուկ ատրիբուտներ

### `keep-alive` Ատրիբուտը <sup>փոխարինված է</sup>

`keep-alive`-ը այլևս հատուկ ատրիբուտ չէ, փոխարենը փաթաթվող կոմպոնենտ, նման լինելով `<transition>`-ին։ Օրինակի համար՝

``` html
<keep-alive>
  <component v-bind:is="view"></component>
</keep-alive>
```

Սա մեզ հնարավորություն է տալիս օգտագործել `<keep-alive>` մի քանի պայմանական ժառանգողների վրա․

``` html
<keep-alive>
  <todo-list v-if="todos.length > 0"></todo-list>
  <no-todos-gif v-else></no-todos-gif>
</keep-alive>
```

<p class="tip">Երբ `<keep-alive>`-ը ունի մի քանի ժառանգող, նրանք ի վերջո հաշվարկվում են դեպի մեկ ժառանգողի։ Ցանկացած ժանագող բացի առաջինից կանտեսվի։</p>

Երբ օգտագործվում է `<transition>`-ի հետ, համոզվեք որ այն տեղադրված է իր ներքում․

``` html
<transition>
  <keep-alive>
    <component v-bind:is="view"></component>
  </keep-alive>
</transition>
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզսի փնտրել օրինակներ <code>keep-alive</code> ատրիբուտների վերաբերյալ։</p>
</div>
{% endraw %}

## Ինտերպոլացիա

### Ինտերպոլացիա Ատրիբուտներում <sup>ջնջված է</sup>

Ինտերպոլացիան ատրիբուտներում այլևս վավեր չէ։ Օրինակի համար․

``` html
<button class="btn btn-{{ size }}"></button>
```

Կամ պետք է թարմացվի որպեսզի օգտագործի inline արտահայտություն

``` html
<button v-bind:class="'btn btn-' + size"></button>
```

Կամ պետք է լինի տվյալ/հաշվարկված հատկություն․

``` html
<button v-bind:class="buttonClasses"></button>
```

``` js
computed: {
  buttonClasses: function () {
    return 'btn btn-' + size
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ ինտերպոլացիայի որը օգտագործվել է ատրիբուտներում։</p>
</div>
{% endraw %}

### HTML Ինտերպոլացիա <sup>ջնջված է</sup>

HTML ինտերպոլացիա (`{% raw %}{{{ foo }}}{% endraw %}`) ջնջվել է ի օգուտ [`v-html` ուղղորդիչի](../api/#v-html).

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ HTML ինտերպոլացիաների վերաբերյալ։</p>
</div>
{% endraw %}

### Մեկ Անգամյա Կապումները <sup>փոխարինվել է</sup>

Մեկ անգամյա կապուները (`{% raw %}{{* foo }}{% endraw %}`) փոխարինվել են նոր [`v-once` ուղղորդիչով](../api/#v-once)։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել մեկ անգամյա կապումները։</p>
</div>
{% endraw %}

## Ռեակտիվություն

### `vm.$watch` <sup>փոփոխվել է</sup>

Watcher-ները ստեղծված `vm.$watch`-ի կողմի հիմա արձակվում են նախքան կապված կոմպոնենտի վերամատուցումը։ Սա տալիս է ձեր հնարավորություն որպեսզի հավելյալ վիճակի թարմացումներ իրականացնել նախքան կոմպոնենտի վերամատուցումը, որի շնորհիվ դուք կարող եք խուսափել անիմաստ թարմացումներից։ Օրինակի համար, դուք կարող եք watch անել կոմպոնենտի prop-ը և թարմացնել կոմպոնենտի տվյալները երբ prop-ը փոփոխվում է։

Եթե դուք նախկինում օգտագործում էիք `vm.$watch`-ը որպեսզի ինչ որ մի բան անել DOM—ի հետ երբ կոմպոնենտը թարմացվում էր, դուք կարող եք փոխարենը անել այն `updated` lifecycle hook-ում։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք ձեր end-to-end թեստերը եթե ունեք։ <strong>Ձախողված թետերը</strong> պետք է զգուշացնեն ձեզ որ watcher—ը կախված էր հին behavior-ին։</p>
</div>
{% endraw %}

### `vm.$set` <sup>փոփոխվել է</sup>

`vm.$set` հիմա այլ անուն է [`Vue.set`-ի համար](../api/#Vue-set)։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ անվավեր օգտագործման վերաբերյալ։</p>
</div>
{% endraw %}

### `vm.$delete` <sup>փոփոխվել է</sup>

`vm.$delete`-ը հիմա այլ անուն է [`Vue.delete`-ի համար](../api/#Vue-delete)։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայուն որպեսզի փնտրել օրինակներ անվավեր օգտագործման վերաբերյալ։</p>
</div>
{% endraw %}

### `Array.prototype.$set` <sup>ջնջվել է</sup>

Փոխարենը օգտագործեք `Vue.set`-ը։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>.$set</code>-ի որը օգտագործվում է զանգվածի վրա։ Եթե դուք բաց թողնեք որևէ մեկը, դուք պետք է նայեք <strong>console-ի error-ները</strong> բացակայող մեթոդից։</p>
</div>
{% endraw %}

### `Array.prototype.$remove` <sup>ջնջվել է</sup>

Փոխարենը օգտագործեք `Array.prototype.splice`։ Օրինակի համար․

``` js
methods: {
  removeTodo: function (todo) {
    var index = this.todos.indexOf(todo)
    this.todos.splice(index, 1)
  }
}
```

Կամ ավելի լավ տարբերակ, փոխանցելով ջնջելու մեթոդները որպես index․

``` js
methods: {
  removeTodo: function (index) {
    this.todos.splice(index, 1)
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>.$remove</code>-ի որը օգտագործվում է զանգվածի վրա։ Եթե դուք բաց թողնեք որևէ մեկը, դուք պետք է նայեք <strong>console-ի error-ները</strong> բացակայող մեթոդից։</p>
</div>
{% endraw %}

### `Vue.set`-ը և `Vue.delete`-ը Vue instance-ներում <sup>ջնջվել է</sup>

`Vue.set`-ը և `Vue.delete`-ը այլևս չեն աշխատի Vue instance-ներում։ Հիմա պարտադիր է ճիշտ կերպով հայտարարել բոլոր բարձր կարգի ռեակտիվ հատկությունները data ընտրանքում։ Եթե դուք ցանկանում եք ջնջել հատկությունները Vue instance-ից կամ իր `$data`-ից, դրեք այն null:

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>Vue.set</code>-ի կամ <code>Vue.delete</code>-ի վերաբերյալ որոնք օգտագործվում են Vue-ի instance-ում։ Եթե դուք բաց թողնեք որևէ մեկը, նրանք կարձակեն <strong>console-ի warning-ներ</strong>։</p>
</div>
{% endraw %}

### `vm.$data`-ի Փոխարինումը <sup>ջնջվել է</sup>

Հիմա արգելվում է փոխարինել կոմպոնենտի instance-ի արմատային $data-ն։ Սա կանխում է որոշ հազվադեպ դեպքերը ռեակտիվության համակարգում և դարձնում է կոմպոնենտի վիճակը ավելի կանխատեսելի (հատկապես մուտքագրման համակարգերի հետ)։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>vm.$data</code>-ի վերագրման վերաբերյալ։ Եթե բաց թողնեք որևէ մեկը,  <strong>console-ի նախազգուշացումներ</strong> կարձակվեն։</p>
</div>
{% endraw %}

### `vm.$get` <sup>ջնջվել է</sup>

Փոխարենը, ուղիղ ստացեք ռեակտիվ տվյալները։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>vm.$get</code>-ի վերաբերյալ։ Եթե դուք բաց թողնեք որևէ մեկը, դուք կտեսնեք <strong>console-ի error-ներ</strong>։</p>
</div>
{% endraw %}

## DOM-Կենտրոնացված Instance-ի Մեթոդներ

### `vm.$appendTo` <sup>ջնջվել է</sup>

Օգտագործեք ներքին DOM API-ը․

{% codeblock lang:js %}
myElement.appendChild(vm.$el)
{% endcodeblock %}

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>vm.$appendTo</code>-ի։ Եթե դուք բաց թողնեք որևէ մեկը, դուք կտեսնեք <strong>console-ի error-ները</strong>։</p>
</div>
{% endraw %}

### `vm.$before` <sup>ջնջվել է</sup>

Օգտագործեք ներքին DOM API-ը․

{% codeblock lang:js %}
myElement.parentNode.insertBefore(vm.$el, myElement)
{% endcodeblock %}

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>vm.$before</code>-ի օգտագործման վերաբերյալ։ Եթե դուք բաց թողնեք որևէ մեկը, դուք կտեսնեք <strong>console-ի error-ներ</strong>։</p>
</div>
{% endraw %}

### `vm.$after` <sup>ջնջվել է</sup>

Օգտագործեք ներքին DOM API-ը․

{% codeblock lang:js %}
myElement.parentNode.insertBefore(vm.$el, myElement.nextSibling)
{% endcodeblock %}

Կամ եթե `myElement`-ը վերջին ժառանգողն է․

{% codeblock lang:js %}
myElement.parentNode.appendChild(vm.$el)
{% endcodeblock %}

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>vm.$after</code>։ Եթե դուք բաց թողնեք որևէ մեկը, դուք կտեսնեք <strong>console-ի error-ներ</strong>։</p>
</div>
{% endraw %}

### `vm.$remove` <sup>ջնջվել է</sup>

Օգտագործեք ներքին DOM API-ը․

{% codeblock lang:js %}
vm.$el.remove()
{% endcodeblock %}

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>vm.$remove</code>-ի։ Եթե դուք բաց թողնեք որևէ մեկը, դուք կտեսնեք <strong>console-ի error-ներ</strong>։</p>
</div>
{% endraw %}

## Meta Instance-ի Մեթոդները

### `vm.$eval` <sup>ջնջվել է</sup>

Ոչ մի ռեալ օգտագործում։ Եթե դուք դեռ կախված եք այս հատկությունից և համոզված չեք թե ինչպես շրջանցեք այն, հրատարակեք խնդիրը այս [ֆորումում](https://forum.vuejs.org/) օգնության համար։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>vm.$eval</code>-ի։ Եթե դուք բաց թողնեք որևէ մեկը, դուք կտեսնեք <strong>console-ի error-ներ</strong>։</p>
</div>
{% endraw %}

### `vm.$interpolate` <sup>ջնջվել է</sup>

Ոչ մի ռեալ օգտագործում։ Եթե դուք դեռ կախված եք այս հատկությունից և համոզված չեք թե ինչպես շրջանցեք այն, հրատարակեք խնդիրը այս [ֆորումում](https://forum.vuejs.org/) օգնության համար։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>vm.$interpolate</code>-ի։ Եթե դուք բաց թողնեք որևէ մեկը, դուք կտեսնեք <strong>console-ի error-ներ</strong>։</p>
</div>
{% endraw %}

### `vm.$log` <sup>ջնջվել է</sup>

Օգտագործեք [Vue Devtools-ը](https://github.com/vuejs/vue-devtools) օպտիմալ debugging-ի համար։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>vm.$log</code>-ի։ Եթե դուք բաց թողնեք որևէ մեկը, դուք կտեսնեք <strong>console-ի error-ներ</strong>։</p>
</div>
{% endraw %}

## Instance-ի DOM Ընտրանքները

### `replace: false` <sup>ջնջվել է</sup>

Կոմպոնենտները հիմա միշտ փոխարինում են էլեմենտը որին նրանք կամպված են։ Որպեսզի ձևացնել `replace: false`-ի վարքը, դուք կարող եք փաթաթել ձեր արմատային կոմպոնենտը էլեմենտով որը նման կլինի այն էլեմենտին որը որ դուք ցանկանում եք փոխարինել։ Օրինակի համար․

``` js
new Vue({
  el: '#app',
  template: '<div id="app"> ... </div>'
})
```

Կամ render ֆունկիցայով․

``` js
new Vue({
  el: '#app',
  render: function (h) {
    h('div', {
      attrs: {
        id: 'app',
      }
    }, /* ... */)
  }
})
```

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>replace: false</code>-ի վերաբերյալ։</p>
</div>
{% endraw %}

## Գլոբալ Կոնֆիգուրացիա

### `Vue.config.debug` <sup>ջնջվել է</sup>

Այլևս անհրաժեշտ չէ, մինչ նախազգուշացումները գալիս են stack trace-երի հետ հանդերձ հիմնականում։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>Vue.config.debug</code>-ի վերաբերյալ։</p>
</div>
{% endraw %}

### `Vue.config.async` <sup>ջնջվել է</sup>

Async-ը հիմա պահանջվում է render-ի performance—ի համար։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>Vue.config.async</code>-ի վերաբերյալ։</p>
</div>
{% endraw %}

### `Vue.config.delimiters` <sup>ջնջվել է</sup>

Սա վերանայվել է որպես [component-ի աստիճան ընտրանք](../api/#delimiters)։ Սա թույլ է տալիս ձեզ օգտագործել այլ դելիմետրներ ձեր ծրագրում առանց կոտրելու 3-րդ կողմի կոմպոնենտները։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>Vue.config.delimiters</code>-ի վերաբերյալ։</p>
</div>
{% endraw %}

### `Vue.config.unsafeDelimiters` <sup>ջնջվել է</sup>

HTML ինտերպոլացիան [ջնջվել է օգուտ `v-html`-ի](#HTML-Interpolation-removed)։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>Vue.config.unsafeDelimiters</code>-ի վերաբերյալ։ Այնուհետև, helper-ը նաև կգտնի HTML-ի instance-ի ինտերպոլացիաներ որ դուք փոխարինեք նրանց `v-html`-ով։</p>
</div>
{% endraw %}

## Գլոբալ API

### `Vue.extend`-ը `el`-ի հետ <sup>ջնջվել է</sup>

el ընտրանքը այլևս չի կարող օգտագործվել `Vue.extend`-ում։ Այն միայն վավեր է որպես instance-ի ստեղծման ընտրանք։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Թարմացնելուց հետո աշխատացրեք ձեր end-to-end թեստերի հավաքածուն կամ ծրագիրը և նայեք <strong>console-ի warning-ները</strong> որը վերաբերվում է <code>el</code> ընտրանքի օգտագործումը <code>Vue.extend</code>-ի հանդերձ։</p>
</div>
{% endraw %}

### `Vue.elementDirective` <sup>ջնջվել է</sup>

Փոխարենը օգտագործեք կոմպոնենտներ։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>Vue.elementDirective</code>։</p>
</div>
{% endraw %}

### `Vue.partial` <sup>ջնջվել է</sup>

Partial-ները ջնջվել են օգուտ կոմպոնենտների միջև ավելի հստակ տվյալների հոսքի համար, օգտագործելով prop-ներ։ Չնայած դուք օգտագործում եք մասամբ իրագործման կրիտիկական մասերում, խորհուրդ է տրվում օգտագործել [հասարակ կոմպոնենտ](components.html) փոխարենը։ Եթե դուք դինամիկորեն կապում եք `name`-ը partial-ի, դուք կարող եք օգտագործել [դինամիկ կոմպոնենտ](components.html#Dynamic-Components)։

Եթե պատահի որ դուք օգտագործում եք partial-ներ ձեր ծրագրի իրագործման կրիտիկական մասերում, ուրեմն դուք պետք է զարգացնեք դեպի [ֆունկցիոնալ կոմպոնենտներ](render-function.html#Functional-Components)։ Նրանք պետք է լինել հասարակ JS/JSX ֆայլում (ի փոխարեն `.vue` ֆայլում) և նրանք state և instance չունեն, ինչպես partial-ները։ Սա դարձնում է rendering-ը բավականին արագ։

Առավելությունը ֆունկցիոնալ կոմպոնենտների partial—ների փոխարեն դա նրանց ավելի դինամիկ լինելու հնարավորությունն է, որովհետև նրանք թույլ են տալիս ձեզ օգտագործել ամբողջ ուժը JavaScript-ի։ Սակայն կա ծախս այս ուժի համար։ Եթե դուք երբեք չեք օգտագործել կոմպոնենտի framework render ֆունկցիաներով նախկինում, նրանք կարող են որոշ ժամանակ տանել ձեզանից սովորելու համար։

{% raw %}
<div class="upgrade-path">
  <h4>Թարմացման ուղի</h4>
  <p>Աշխատացրեք <a href="https://github.com/vuejs/vue-migration-helper">migration helper-ը</a> ձեր կոդային բազայում որպեսզի փնտրել օրինակներ <code>Vue.partial</code>-ի վերաբերյալ։</p>
</div>
{% endraw %}
