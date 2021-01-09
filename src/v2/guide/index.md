---
title: Ներածություն
type: Ուղեցույց
order: 2
---

## Ի՞նչ Է Vue.js-ը

Vue-ն (արտասանվում է որպես վյու, ինչպես **view** Անգլերեն) այն **առաջադեմ framework** է որը նախատեսված է օգտագործողի ինտերֆայս ստեղծելու համար։ Ի տարբերություն ուրիշ մոնոլիթային framework-երի, Vue-ն դիզայնավորված է զրոյից որպեսզի աստիճանաբար ադապտացվի։ Հիմնական գրադարանը կենտրոնացված է միայն դիտման շերտի վրա, և հեշտ է վերցնել և ինտեգրացնել այլ գրադարանների կամ գոյություն ունեցող նախագծերի հետ։
Մյուս կողմից, Vue-ն իդեալականորեն ի վիճակի է նաև գործարկել բարդ մեկ էջ ծրագրեր, երբ օգտագործվում է զուգահեռ  [ժամանակակից գործիքների](single-file-components.html) և [աջակցող գրադարանների](https://github.com/vuejs/awesome-vue#components--libraries) հետ։

Եթե ցանկանում եք իմանալ ավելի Vue-ի մասին նախքան սուզվելը, մենք <a id="modal-player"  href="#">ստեղծելենք տեսահոլովակ</a> ցույց տալով հիմնական սկզբունքների և նմուշային ծրագրի միջոցով․

Եթե դուք փորձառու frontend ծրագրավորող եք և ցանկանում եք իմանալ, թե ի՞նչպես է Vue-ն համամատվում այլ գրադարանների/framework-երի հետ, դուք կարող կարդալ [Համեմատումը այլ framework-երի հետ](comparison.html) բաժինը.

<div class="vue-mastery"><a href="https://www.vuemastery.com/courses/intro-to-vue-js/vue-instance/" target="_blank" rel="sponsored noopener" title="Free Vue.js Course">Դիտել անվճար տեսահոլովակ կուրս Vue Mastery-ում</a></div>

## Առաջին քայլերը

<a class="button" href="installation.html">Տեղադրում</a>

<p class="tip">
Պաշտոնական ուղեցույցը ենթադրում է HTML, CSS և JavaScript-ի միջանկյալ մակարդակի գիտելիքներ: Եթե դուք միանգամայն նոր եք frontend ծրագրավորման մեջ, ապա գուցե լավագույն գաղափարը չլինի որևէ framework-ի անցնել, քանի որ ձեր առաջին քայլը հիմունքները իմացությունն է: Այլ framework-երի հետ նախնական փորձը օգնում է, բայց պարտադիր չէ:

Ամենահեշտ ձևը փորձելու Vue.js-ը դա [Բարև Աշխարհ օրինակն է](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-hello-world)։ Ազատորեն բացեք այն մեկ այլ պատուհանում և հետևեք դրան, երբ մենք անցնում ենք մի քանի հիմնական օրինակների միջով։ Կամ, դուք կարոք էք <a href="https://github.com/vuejs/vuejs.org/blob/master/src/v2/examples/vue-20-hello-world/index.html" target="_blank" download="index.html" rel="noopener noreferrer">ստեղծել <code>index.html</code>ֆայլ</a> և ընդգրկել Vue-ն հետևյալ ձևով:

``` html
<!-- զարգացման տարբերակ, ներառում է օգտակար console-ի նախազգուշացումներ -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

կամ:

``` html
<!-- արտադրության տարբերակը, օպտիմիզացված չափի և արագության համար -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

[Տեղադրում](installation.html) էջում ներկայացված են Vue-ի տեղադրման ավելի շատ տարբերակներ։ Նշում․ Մենք խորհուրդ **չենք** տալիս որ սկսնակները սկսեն `vue-cli`-ի հետ, հատկապես եթե դուք ծանոթ չէք Node.js-ի վրա հիմնված գործիքների հետ։

Եթե նախընտրում եք ավելի ինտերակտիվ ինչ-որ բան, կարող եք նաև նայել [այս ձեռնարկների շարքը Scrimba-ում](https://scrimba.com/g/gvuedocs), որը տալիս է ձեզ հնարավորություն ստեղծել էկրանների և կոդերի «խաղահրապարակ» որը կարող եք դադարեցնել և միացնել ցանկացած պահին։

## Հայտարարագրերի ստեղծում

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/cQ3QVcr" target="_blank" rel="noopener noreferrer">Փորձեք այս դասընթացը Scrimba-ում</a></div>

Vue.js-ի հիմքում կա մի համակարգը որը մեզ թույլ է տալիս հայտարարելով ստեղծել տվյալների փոփոխականեր որոնք կարող է փոխանցվել DOM-ին օգտագործելով ձևանմուշի գրելաձևը։

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Բարև Աշխարհ՜'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Բարև Աշխարհ՜'
  }
})
</script>
{% endraw %}

Մենք արդեն ստեղծեցինք մեր առաջին Vue ծրագիրը՜ այն բավականին նման է բառային ձևանմուշ տպելուն, սակայն Vue ավելի շատ աշխատանք է կատարել համակարգի ետևում։ Տվյալները և DOM-ը հիմա կապված են, և ամեն ինչ **ռեակտիվ** է։ Ի՞նչպես իմացանք մենք դա․ Բացեք ձեր բրաուզերի JavaScript console-ը (հիմա, և այս էջում) և փոփոխեք `app.message`-ի արժեքը։ Դուք պետք է տեսնեք արտատպված տարբերակում փոփոխությունները։

Նշում․ մենք այլևս ուղղակիորեն չպետք է շփվենք HTML-ի հետ։ Vue-ի ծրագիրը միացնում է իրեն միայնակ DOM էլեմենտների հետ (`#app` մեր դեպքում) այնուհետև ամբողջությամբ կառավարում է այն։ HTML-ը մեր սկզբնակետն է, բայց մնացացը կատարվում է նոր Vue-ի կազմի մեջ։

Բացի տեքստի միջնորդությունից, մենք կարող ենք նաև այսպիսի տարրական ատրիբուտներ կապել․

``` html
<div id="app-2">
  <span v-bind:title="message">
    Պահիր մկնիկը իմ վրա մի քանի վայրկյանով
    որպեսզի տեսնես դինամիկորեն կապված տեքստը՜
  </span>
</div>
```
``` js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Դուք բեռնել եք այս էջը հետևյալ ամսաթվին ' + new Date().toLocaleString()
  }
})
```
{% raw %}
<div id="app-2" class="demo">
  <span v-bind:title="message">
    Պահիր մկնիկը իմ վրա մի քանի վայրկյանով
    որպեսզի տեսնես դինամիկորեն կապված տեքստը՜
  </span>
</div>
<script>
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Դուք բեռնել եք այս էջը հետևյալ ամսաթվին ' + new Date().toLocaleString()
  }
})
</script>
{% endraw %}

Այստեղ մենք հանդիպում ենք մի նոր բանի: Ձեր տեսած `v-bind` հատկանիշը կոչվում է **ուղղորդիչ** (directive): Ուղղորդիչները նախածանցվում են `v-` բառով՝ նշելով, որ դրանք Vue-ի կողմից տրամադրված հատուկ ատրիբուտներ են, և ինչպես գուցե կռահել եք, նրանք հատուկ ռեակտիվ վարք են գործում մատուցված DOM-ի համար: Այստեղ, ըստ էության, ասվում է. «Պահիր այս տարրի `title` ատրիբուտը թարմեցվաց Vue-ի `message` հատկանիշի հետ»:

Եթե բացեք ձեր JavaScript console-ը նորից և գրեք `app2.message = 'some new message'` և սեղմեք enter, դուք կտեսնեք ևս մեկ անգամ որ կապակցված HTML-ը այս դեպքում `title` ատրիբուտը թարմեցվել է:

## Պայմաններ և Ցիկլներ

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/cEQe4SJ" target="_blank" rel="noopener noreferrer">Փորձեք այս դասընթացը Scrimba-ում</a></div>

Նույնպես, հեշտ է անջատել էլեմենտի ներկայությունը։

``` html
<div id="app-3">
  <span v-if="seen">Հիմա ինձ տեսնում ես</span>
</div>
```

``` js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

{% raw %}
<div id="app-3" class="demo">
  <span v-if="seen">Հիմա ինձ տեսնում ես</span>
</div>
<script>
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
</script>
{% endraw %}

Փորձեք գրել `app3.seen = false` console-ի մեջ և սեղմեք enter. Դուք պետք է տեսնեք որ նամակը անհետացել է։

Այս օրինակը ցույց է տալիս որ մենք կարող ենք միացնել տվյալներ ոչ միայն մեր տեքստին և ատրիբուտներին, բայց նայև DOM-ի **կառուցվածքին**։ Ավելին, Vue-ն նաև տրամադրում է հզոր անցումային էֆեկտ որը կարող է ավտոմատ կիրառել [անցումային էֆեկտներ](transitions.html) երբ տարրերը տեղադրված/թարմեցված/ջնջված են Vue-ի կողմից։

Կան նաև մի քանի ուրիշ ուղղորդիչներ, ամեն մեկը ունի հատուկ ֆունկցիոնալություն։ Օրինակ՝ `v-for` ուղղորդիչը կարող է օգտագործվել ցանկ ցույց տալու համար օգտագործելով տվյալները զանգվածի միջից։

``` html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
``` js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Սովորել JavaScript' },
      { text: 'Սովորել Vue' },
      { text: 'Կառուցել հիանալի մի բան' }
    ]
  }
})
```
{% raw %}
<div id="app-4" class="demo">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Սովորել JavaScript' },
      { text: 'Սովորել Vue' },
      { text: 'Կառուցել հիանալի մի բան' }
    ]
  }
})
</script>
{% endraw %}

Console-ի մեջ աշխատացրեք `app4.todos.push({ text: 'New item' })`։ Դուք պետք է տեսնեք նոր ավելացված իրը ցանկի մեջ։

## Օգտագործողի Մուտքագրման Հետ Առնչվելը

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/czPNaUr" target="_blank" rel="noopener noreferrer">Փորձեք այս դասընթացը Scrimba-ում</a></div>

Որպեսզի թողնել օգտվողներին շփվել ձեր ծրագրի հետ, մենք կարող ենք օգտագործել `v-on` ուղղորդիչը որպեսզի միացնենք event listeners որոնք կկանչեն մեթոդներ մեր Vue-ի մեջ։

``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Հակադարձ</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Բարև Vue.js՜'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app-5" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Հակադարձ</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Բարև Vue.js՜'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Նշեմ նաև որ այս մեթոդում մենք թարմացնում ենք վիճակը (state-ը) մեր ծրագրի, առանց դիպչելու DOM-ին․ բոլոր DOM-ի մանիպուլացիաները գործածում է Vue-ն, և այն կոդը որ դուք գրել եք կենտրոնացված է հիմքում ընկած տրամաբանության վրա։

Vue-ն նաև տրամադրում է `v-model` ուղղորդիչը որը ստեղծում է երկկողմանի կապ մուտքագրման դաշտի և ծրագրի վիճակի մեջ:

``` html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Բարև Vue՜'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Բարև Vue՜'
  }
})
</script>
{% endraw %}

## Կոմպոնենտների հետ Ստեղծագործումը

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/cEQVkA3" target="_blank" rel="noopener noreferrer">Փորձեք այս դասընթացը Scrimba-ում</a></div>

Կոմպոնենտների համակարգը ևս կարևոր գաղափար է Vue-ում, որովհետև այն աբստրակցիա է որը մեզ թույլ է տալիս կառուցել մեծածավալ ծրագրեր կազմված փոքր, ինքնամփոփ, և վերօգտագործվող կոմպոնենտներից։
Եթե մենք մտածենք դրա մասին, գրեթե ցանկացած տիպի ծրագրի տեսական մասը կարող է բաժանվել կոմպոնենտների ծառի։

![Կոմպոնենտների Ծառ](/images/components.png)

Vue-ում, կոմպոնենտը կազմում է մի մաս որը ունի նախապես սահմանված ընտրանքներ։ Գրանցելը կոմպոնենտը Vue-ի մեջ պարզ է։

``` js
// Ստեղծենք նոր կոմպոնենտ todo-item անունով
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})

var app = new Vue(...)
```

Հիմա դուք կարող եք ավելացնել դա ուրիշ կոմպոնենտի ձևանմուշի մեջ։

``` html
<ol>
  <!-- Ավելացնել todo-item կոմպոնենտը -->
  <todo-item></todo-item>
</ol>
```

Բայց այս դեպքում այն կելքագրի նույն տեքստը ամեն անգամ երբ կոմպոնենտը ավելացրած է, որը այդքանել հետաքրքիր չէ։ Մենք պետք է հնարավորություն ունենանք փոխանցել տվյալներ դեպի ժառանգող կոմպոնենտներ։ Եկեք փոփոխենք կոմպոնենտի նկարագրությունը որպեսզի այն ընդունի [հատկություն](components.html#Props) (prop):

``` js
Vue.component('todo-item', {
  // todo-item կոմպոնենտը հիմա ընդհունում է
  // «prop» (հատկություն), որը նման է ատրիբուտի։
  // Այս հատկության անունն է todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Հիմա մենք կարող ենք փոխանցել տվյալ ամեն կրկնվող կոմպոնենտի մեջ օգտագործելով `v-bind`:

``` html
<div id="app-7">
  <ol>
    <!--
      Հիմա մենք տրամադրում ենք ամեն todo-item-ին
      todo օբյեկտ որը իրենից ներկայացնում է, որ նրա
      բովանդակությունը կարող է լինել դինամիկ։
      Մենք նաև տրամադրում ենք ամեն կոմպոնենտին "key" (բանալի),
      որը կբացատրվի ավելի ուշ։
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
```
``` js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Բանջարեղեններ' },
      { id: 1, text: 'Պանիր' },
      { id: 2, text: 'Ինչ որ բան որ մարդիկ ուտում են' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item" :key="item.id"></todo-item>
  </ol>
</div>
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Բանջարեղեններ' },
      { id: 1, text: 'Պանիր' },
      { id: 2, text: 'Ինչ որ բան որ մարդիկ ուտում են' }
    ]
  }
})
</script>
{% endraw %}

Սա հակասական օրինակ է, բայց մեզ հաջողվել է առանձնացնել մեր ծրագիրը երկու փոքր բաժինների, և ժառանգողը ճիշտ անջատված է նրա ծնողից props ինտերֆեյսի միջոցով:
Հիմա մենք կարող ենք ավելի բարելավել մեր `<todo-item>` կոմպոնենտը ավելի բարդ ձևանմուշով և տրամաբանությամբ, առանց ծնող ծրագրի վրա ազդելու։

Մեծ ծրագրում, անհրաժեշտ է բաժանել ծրագիրը կոմպոնենտների որպեսզի զարգացումը ավելի կառավարելի լինի։ Մենք կխոսանք ավելի կոմպոնենտների մասին [այս ուղեցույցում ավելի ուշ](components.html), բայց ահա (երևակայական) օրինակ, թե ինչ կարող է լինել ծրագրի ձևանմուշը բաղադրիչներով։

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### Հարաբերությունը Custom Էլեմենտների Հետ

Գուցե նկատել եք, որ Vue-ի կոմպոնենտները շատ նման են **Custom Էլեմենտների**-ին, որոնք [Web Components Spec](https://www.w3.org/wiki/WebComponents/)-ի մի մասն են կազմում։
Որովհետև Vue-ի գրելաձևը ստեղծվել է հիմք ընդհունելով այն։ Օրինակ՝ Vue-ի կոմպոնենտը ունի [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) և `is`-ը հատուկ ատրիբուտ է։
Սակայն, մի քանի տարբերություններ կան՝

1. The Web Components Spec-ը ավարտված է, բայց այն չեն տեղադրել բոլոր բրաուզերներում. Safari 10.1+, Chrome 54+ և Firefox 63+ ունեն վեբ կոմպոնենտները. Ի համեմատ, Vue-ի կոմպոնենտները չեն պահանջում որև polyfill-ներ և աշխատում են կայուն բոլոր այն բրաուզերների մեջ որոնք աշխատում են վեբ կոմպոնենտների հետ (IE9 և ավելի նոր). Երբ անհրաժեշտ է, Vue-ի կոմպոնենտները կարող են նաև «փաթաթվել» այլ custom էլեմենտների մեջ։

2. Vue-ի կոմպոնենտները տրամադրում են կարևոր հատկություններ որոնք գոյություն չունեն այլ custom էլեմենտների մեջ, ամենանկատելին կոմպոնենտից կոմպոնենտ տվյալնների հոսքն է, հատուկ event-ների կապը և կառուցման գործիքների ինտեգրացիաները։

Չնայած Vue-ն ներքինում չի օգտագործում սովորական էլեմենտներ, այն ունի [հիանալի փոխգործունակություն](https://custom-elements-everywhere.com/#vue), երբ խոսքը վերաբերում է ստեղծված էլեմենտի սպառման կամ տարածման մասին: Vue CLI-ը աջակցում է նաև Vue-ի կոմպոնենտների կառուցմանը, որոնք իրենց գրանցում են որպես ստեղծաված Էլեմենտներ։

## Ուզում է՞ք ավելին

Մենք հակիրճ ներկայացրեցինք Vue.js-ի հիմնական հատկանիշները։ Այս ուղեցույցի մնացած մասերը ավելի մանրակրկիտ մանրամասներով կբացատրի դրանք և այլ առաջադեմ առանձնահատկությունները, այնպես որ համոզված եղեք որ կկարդաք բոլորը:

<div id="video-modal" class="modal"><div class="video-space" style="padding: 56.25% 0 0 0; position: relative;"><iframe src="https://player.vimeo.com/video/247494684?dnt=1" style="height: 100%; left: 0; position: absolute; top: 0; width: 100%; margin: 0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div><script src="https://player.vimeo.com/api/player.js"></script><p class="modal-text">Video by <a href="https://www.vuemastery.com" target="_blank" rel="sponsored noopener" title="Vue.js Courses on Vue Mastery">Vue Mastery</a>. Դիտեք Vue Mastery-ի անվճար <a href="https://www.vuemastery.com/courses/intro-to-vue-js/vue-instance/" target="_blank" rel="sponsored noopener" title="Vue.js Courses on Vue Mastery">Ներածություն Vue դասընթացին</a>.</div>
