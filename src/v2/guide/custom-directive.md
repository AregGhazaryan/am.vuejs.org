---
title: Custom Ուղղորդիչներ
type: ուղեցույց
order: 302
---

## Ներածություն

<div class="vueschool"><a href="https://vueschool.io/lessons/create-vuejs-directive?friend=vuejs" target="_blank" rel="sponsored noopener" title="Անվճար Vue.js Custom Ուղղորդիչների դաս">Դիտեք անվճար դասը Vue School-ում</a></div>

Հավելյալ հիմնական ուղղորդիչների շարքին (`v-model` և `v-show`), Vue-ն նաև թույլ է տալիս ձեզ գրանցելու custom ուղղորդիչներ։ Նշում որ Vue 2.0-ի մեջ, հիմնական կազմը կոդի վերօգտագործելու և աբստրակցիայի դա կոմպոնենտներն են - սակայն կան դեպքեր երբ որ ձեզ անհրաժեշտ է ցածր աստիճանի DOM access հասարակ էլեմենտների վրա, և այստեղ է երբ custom ուղղորդիչները կարող են ձեզ օգնել։ Օրինակի համար focus անել input էլեմենտի վրա, ինչպես այստեղ՝

{% raw %}
<div id="simplest-directive-example" class="demo">
  <input v-focus>
</div>
<script>
Vue.directive('focus', {
  inserted: function (el) {
    el.focus()
  }
})
new Vue({
  el: '#simplest-directive-example'
})
</script>
{% endraw %}

Երբ էջը բեռնվում է, էլեմենտը ստանում է focus (նշում․ `autofocus`-ը չի աշխատում Safari Mobile-ի վրա)։ Եթե դուք չեք սեղմել ցանկացած մի բան մինչ այս էջում էիք, input-ը վերևի պետք է focus եղած լինի։ Հիմա եկեք կառուցենք ուղղորդիչ որը կատարում է նույն բանը։

``` js
// Գրանցեք գլոբալ custom ուղղորդիչ `v-focus` անվանումով
Vue.directive('focus', {
  // Երբ կապված էլեմենտը դրվել է DOM-ում...
  inserted: function (el) {
    // Focus լինի էլեմենտի վրա
    el.focus()
  }
})
```

Եթե դուք ցանկանում եք գրանցել ուղղորդիչ լոկալ կերպով, կոմպոնենտները նաև ընդհունում են `directives` ընտրանքը․

``` js
directives: {
  focus: {
    // ուղղորդչի հայտարարումը
    inserted: function (el) {
      el.focus()
    }
  }
}
```

Այնուհետև ձևանմուշում, դուք կարող եք օգտագործել նոր `v-focus` ատրիբուտը ցանկացած էլեմենտի վրա, այսպես․

``` html
<input v-focus>
```

## Hook Ֆունկցիաներ

Ուղղորդչի հայտարարման օբյեկտը կարող է տրամադրել մի քանի hook ֆունկցիաներ (բոլորը կարող են օգտագործվել ըստ ցանկության)․

- `bind`: աշխատում է միայն մեկ անգամ, երբ ուղղորդիչը կապված է էլեմենտին։ Այստեղ դուք կարող եք կատարել մեկ անգամյա տեղադրման աշխտանանքեր։

- `inserted`: աշխատում է երբ կապված էլեմենտը տեղադրվել է իր ծնող node-ում (միայն երաշխավորում է ծնող node-ի ներկայությունը, որը պարտադիր չէ, որ փաստաթղթում լինի)։

- `update`: աշխատում է երբ պարունակող կոմպոնենտի VNode-ը թարմացվել է, __բայց հնարավոր է, որ մինչ նրա ժառանգողները թարմացվեն__։ Ուղղորդչի արժեքը հնարավոր է որ փոխված կամ չփոխված լինի, բայց դուք կարող եք բաց թողնել անիմաստ թարմացումները համեմատելով կապման նոր և հին արժեքները (նայեք ներքևում արգումենտների համար)։

<p class="tip">Մենք ավելի մանրամասն կցուցադրենք VNodes-ը [ավելի ուշ](./render-function.html#The-Virtual-DOM), երբ որ մենք քննարկենք [render ֆունկցիաները](./render-function.html).</p>

- `componentUpdated`: աշխատում է երբ պարունակող կոմպոնենտի VNode-ը __և ժառանգողների VNode-երը__ թարմացվել են։

- `unbind`: աշխատում է միայն մեկ անգամ, երբ ուղղորդիչը պոկվել է էլեմենտից։

Մենք կուսումնասիրենք hook-երին փոխանցված արգումենտները (օրինակ․ `el`, `binding`, `vnode`, և `oldVnode`) հաջորդ բաժնում։

## Ուղղորդիչի Hook-ի Արգումենտները

Ուղղորդիչի hook-երին փոխանցված են հետևյալ արգումենտները․

- `el`: Էլեմենտը որին ուղղորդիչը կապված է։ Սա կարող է օգտագործվել որպեսզի ուղիղ փոփոխել DOM-ը։
- `binding`: Օբյեկտ պարունակող հետևյալ հակտությունները։
  - `name`: Անունը ուղղորդիչի, առանց `v-` prefix-ի։
  - `value`: Արժեքը փոխանցված ուղղորդչին։ Օրինակ `v-my-directive="1 + 1"`-ի մեջ, արժեքը կլինի `2`։
  - `oldValue`: Նախորդ արժեքը, միայն հասանելի է `update` և `componentUpdate`-ում։ Այն հասանելի է չկախված թե արժեքը փոփոխվել է թե ոչ։
  - `expression`: Արտահայտությունը կապումը որպես string։ Օրինակ `v-my-directive="1 + 1"`-ի մեջ, արտահայտությունը կլինի `"1 + 1"`։
  - `arg`: Արգումենտը փոխանցված ուղղորդիչին, եթե տրամադրված է։ Օրինակ `v-my-directive:foo`-ի մեջ, արգումենտը կլինի `"foo"`։
  - `modifiers`: Օբյեկտը որը պարունակում է փոփոխիչները, եթե տրամադրված է։ Օրինակ `v-my-directive.foo.bar`-ի մեջ, փոփոխիչների օբյեկտը կլինի `{ foo: true, bar: true }`։
- `vnode`: Վիրտուալ node-ը ստեղծված Vue-ի compiler-ի կողմից։ Նայեք [VNode API](../api/#VNode-Interface) ավելի մանրամասների համար։
- `oldVnode`: Նախորդ վիրտուալ node-ը, միայն հասանելի է `update` և `componentUpdated` hook-երում։

<p class="tip">Բացառությամբ `el`-ից, դուք պետք է վերաբերվեք այս արգումենտներին որպես միայն read-only (կարդալու համար) և երբեք չփոփոխեք նրանց։ Եթե ձեզ անհրաժեշտ է փոխանցել ինֆորմացիա hook-երի միջև, խորհուրդ է տրվում անցնել էլեմենտի [տվյալների կազմի](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset) միջով։</p>

Custom ուղորդիչի օրինակ օգտագործելով այդ հատակություններից որոշը․

``` html
<div id="hook-arguments-example" v-demo:foo.a.b="message"></div>
```

``` js
Vue.directive('demo', {
  bind: function (el, binding, vnode) {
    var s = JSON.stringify
    el.innerHTML =
      'անուն: '       + s(binding.name) + '<br>' +
      'արժեք: '      + s(binding.value) + '<br>' +
      'արտահայտություն: ' + s(binding.expression) + '<br>' +
      'արգումենտ: '   + s(binding.arg) + '<br>' +
      'փոփոխիչներ: '  + s(binding.modifiers) + '<br>' +
      'vnode բանալիներ: ' + Object.keys(vnode).join(', ')
  }
})

new Vue({
  el: '#hook-arguments-example',
  data: {
    message: 'բարև՜'
  }
})
```

{% raw %}
<div id="hook-arguments-example" v-demo:foo.a.b="message" class="demo"></div>
<script>
Vue.directive('demo', {
  bind: function (el, binding, vnode) {
    var s = JSON.stringify
    el.innerHTML =
      'անուն: '       + s(binding.name) + '<br>' +
      'արժեք: '      + s(binding.value) + '<br>' +
      'արտահայտություն: ' + s(binding.expression) + '<br>' +
      'արգումենտ: '   + s(binding.arg) + '<br>' +
      'փոփոխիչներ: '  + s(binding.modifiers) + '<br>' +
      'vnode: ' + Object.keys(vnode).join(', ')
  }
})
new Vue({
  el: '#hook-arguments-example',
  data: {
    message: 'բարև՜'
  }
})
</script>
{% endraw %}

### Դինամիկ Ուղղորդիչների Արգումենտներ

Ուղղորդիչների արգումենտները կարող են լինել դինամիկ։ Օրինակի համար, `v-mydirective:[argument]="value"`-ի մեջ, `argument`-ը կարող է թարմացվել կախված տվյալնների հատկություններից որը գտնվում է մեր կոմպոնենտի instance-ում՜ Սա դարձնում է մեր custom ուղղորդիչները ավելի ճկուն օգտագործման համար մեր ծրագրի մեջ։

Ենթադրենք դուք ցանկանում եք ստեղծել custom ուղղորդիչ որը ձեզ թույլ է տալիս պահել էլեմենտները ձեր էջում օգտագործելով ֆիքսված դիրք: Մենք կստեղծենք custom ուղղորդիչ որտեղ արժեքը թարմացվում է ուղղահայաց դիրքը պիքսելներով, այսպես․

```html
<div id="baseexample">
  <p>Իջեք ներքև scroll անելով</p>
  <p v-pin="200">Պահիր ինձ 200px էջի վերևից հաշված</p>
</div>
```

```js
Vue.directive('pin', {
  bind: function (el, binding, vnode) {
    el.style.position = 'fixed'
    el.style.top = binding.value + 'px'
  }
})

new Vue({
  el: '#baseexample'
})
```

Սա կպահի էլեմենտը 200px էջի վերևից։ Բայց ի՞նչ կլինի եթե մենք հանդիպենք մի դեպք երբ որ մենք պետք է պահենք էլեմենտը ձախ կողմից այլ ոչ թե վերևից: Այս դեպքում դինամիկ արգումենտները որոնք կարող են թարմացվեն ամեն կոմպոնենտի instance-ում շատ մեզ կօգնեն։


```html
<div id="dynamicexample">
  <h3>Իջեք ներքև այս բաժնում ↓</h3>
  <p v-pin:[direction]="200">Ես պահված եմ այս էջում 200px ձախից։</p>
</div>
```

```js
Vue.directive('pin', {
  bind: function (el, binding, vnode) {
    el.style.position = 'fixed'
    var s = (binding.arg == 'left' ? 'left' : 'top')
    el.style[s] = binding.value + 'px'
  }
})

new Vue({
  el: '#dynamicexample',
  data: function () {
    return {
      direction: 'left'
    }
  }
})
```

Result:

{% raw %}
<iframe height="200" style="width: 100%;" class="demo" scrolling="no" title="Դինամիկ Ուղղորդիչի Արգումենտները" src="//codepen.io/team/Vue/embed/rgLLzb/?height=300&theme-id=32763&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  Դիտեք Pen-ը <a href='https://codepen.io/team/Vue/pen/rgLLzb/'>Դինամիկ Ուղղորդիչի Արգումենտները</a> Vue-ի կողմից
  (<a href='https://codepen.io/Vue'>@Vue</a>)<a href='https://codepen.io'>CodePen-ում</a>.
</iframe>
{% endraw %}

Մեր custom ուղղորդիչը հիմա բավականին ճկուն է որ համապատասխանի մի քանի տարբեր օգտագործման դեպքերի հետ։

## Ֆունկցիայի Կարճ Գրելաձև

Շատ դեպքերում, դուք կցանկանաք նույն բանը ինչ `bind`-ում և `update`-ը տրամադրում են, բայց ձեզ անհրաժեշտ չեն այլ hook-երը։ Օրինակի համար․

``` js
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})
```

## Օբյեկտ Literal

Եթե ձեր ուղղորդչին անհրաժեշտ է մի քանի արժեք, դուք կարող եք նաև փոխանցել JavaScript օբյեկտ literal: Հիշեք, որ ուղղորդիչները կարող են ստանալ ցանկացած ճիշտ JavaScript արտահայտություն։

``` html
<div v-demo="{ color: 'սպիտակ', text: 'բարև!' }"></div>
```

``` js
Vue.directive('demo', function (el, binding) {
  console.log(binding.value.color) // => "սպիտակ"
  console.log(binding.value.text)  // => "բարև!"
})
```
