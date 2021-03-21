---
title: Կոմպոնենտների Հիմունքները
type: ուղեցույց
order: 11
---

<div class="vueschool"><a href="https://vueschool.io/courses/vuejs-components-fundamentals?friend=vuejs" target="_blank" rel="sponsored noopener" title="Անվծար Vue.js Կոմպոնենտների Հիմունքների Կուրս">Դիտեք անվճար վիդեո կուրս Vue School-ում</a></div>

## Հիմքի Օրինակ

Ահա Vue կոմպոնենտի օրինակ․

``` js
// Հայտարարենք նոր կոմպոնենտ button-counter անվանումով
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">Դուք սեղմել եք ինձ {{ count }} անգամ։</button>'
})
```

Կոմպոնենտները վերօգտագործվող Vue-ի instance-ներ են անունով․ այս դեպքում, `<button-counter>`-ը։ Մենք կարող ենք օգտագործել այս կոմպոնենտը որպես custom էլեմենտ Vue-ի արմատային instace-ում որը ստեղծվել է `new Vue`-ի շնորհիվ․

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

{% codeblock lang:js %}
new Vue({ el: '#components-demo' })
{% endcodeblock %}

{% raw %}
<div id="components-demo" class="demo">
  <button-counter></button-counter>
</div>
<script>
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count += 1">Դուք սեղմել եք ինձ {{ count }} անգամ։</button>'
})
new Vue({ el: '#components-demo' })
</script>
{% endraw %}

Ի վեր կոմպոնենտները վերօգտագործելի են Vue-ի instance-ներում, նրանք ընդհունում են նույն ընտրանքները ինչպես `new Vue`-ն է, ինչպիսիք են `data`, `computed`, `watch`, `methods`, և lifecycle hook-երը։ Մի քանի արմատին հատուկ ընտրանքներ բացառությամբ ինչպիսին է `el`-ը։

## Կոմպոնենտների Վերօգտագործումը

Կոմպոնենտները կարող են վերօգտագործվել այնքան անգամ, որքան ցանկանում եք․

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

{% raw %}
<div id="components-demo2" class="demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
<script>
new Vue({ el: '#components-demo2' })
</script>
{% endraw %}

Նշում որ երբ սղմում էք կոչակները, ամեն մեկը ունի իր, առանձին `count`-ը։ Որովհետև ամեն անգամ երբ որ դուք օգտագործում եք կոմպոնենտը, նոր **instance** է ստեղծվում իրենից։

### `data` Պետք է Լինի Ֆունկցիա

Երբ մենք հայտարարեցինք `<button-counter>` կոմպոնենտը, դուք հնարավոր է որ նկատել էք որ `data`-ին ուղիղ տրամադրված չէր օբյեկտ, ինչպես այստեղ․

```js
data: {
  count: 0
}
```

Փոխարենը, **կոմպոնենտի `data` ընտրանքը պետք է լինի ֆունկցիա**, որպեսզի ամեն instance կարող է պահպանի անկախ կրկնորինակ վերադարձված օբյեկտից․

```js
data: function () {
  return {
    count: 0
  }
}
```

Եթե Vue-ն չունի այս կանոնը, կոճակին մեկ անգամ սեղմելով կազդի _բոլոր instance-ների_ տվյալներին, ինչպես ներքևում է նշված․

{% raw %}
<div id="components-demo3" class="demo">
  <button-counter2></button-counter2>
  <button-counter2></button-counter2>
  <button-counter2></button-counter2>
</div>
<script>
var buttonCounter2Data = {
  count: 0
}
Vue.component('button-counter2', {
  data: function () {
    return buttonCounter2Data
  },
  template: '<button v-on:click="count++">Դուք սեղմել եք ինձ {{ count }} անգամ։</button>'
})
new Vue({ el: '#components-demo3' })
</script>
{% endraw %}

## Կոմպոնենտների Կազմակերոպումը

Դա սովորական է որ ծրագիրը պետք է կազմակերպված լինի ծառի տեսքով որը պարունակում է ներդրված կոմպոնենտները․

![Կոմպոնենտի Ծառ](/images/components.png)

Օրինակի համար, դուք կարող է ունենաք կոմպոնենտներ նախատեսված header-ի, sidebar-ի, և բովանդակության բաժնի համար, ամեն մեկը պարունակում է այլ կոմպոնենտներ նավիգացիոն հղումների, բլոգի գրառումներ, և այլ մասերի համար։

Որպեսզի օգտագործենք այդ կոմպոնենտները ձևանմուշներում, նրանք պետք է գրանցված լինեն որ Vue-ն նկատի իրենց։ Մենք ունենք երկու տիպի կոմպոնենտի գրանցում․ **գլոբալ** և **լոկալ**։ Մինչ դեռ, մենք միայն գրանցել ենք կոմպոնենտներ գլոբալ կերպով, օգտագործելով `Vue.component`-ը․

```js
Vue.component('my-component-name', {
  // ... ընտրանքները ...
})
```

Գլոբալ գրանցված կոմպոնենտները կարող են օգտագործվել ձևանմուշում որը գտնվում է ցանկցած արմատային Vue instance-ում (`new Vue`) ստեղծումից հետո -- և նույնիսկ բոլոր սուբկոմպոնենտներում որոնք գտնվում են այդ Vue instance-ի կոմպոնենտի ծառում։

Սա այն ամենն է ինչ որ դուք պետք է իմանաք գրանցման մասին, բայց երբ որ դուք վերջացրել էք կարդալ այս էջը և հարմարավետ էք ըզգում իր բովանդակությամբ, մենք խորհուրդ ենք տալիս վերադառնալ ավելի ուշ ամբողջությամբ կարդալու ուղեցույցը [Կոմպոնենտի Գրանցումում](components-registration.html)։

## Տվյալների Փոխանցումը Ժառանգող Կոմպոնենտներին Prop-ների միջոցով

Ավելի վաղ, մենք նշել ենք կոմպոնենտի ստեղծումը բլոգի գրառումների համար։ Խնդիրը այն է, որ կոմպոնենտը օգտակար չի լինի եթե դուք տվյալներ չէք փոխանցում նրան, ինչպիսին է վերնագիրը և բովանդակությունը կոնկրետ գրառման որը մենք ցանկանում ենք ցույց տալ։ Այստեղ է երբ prop-ները մեզ կարող են աջակցել։

Prop-ները custom ատրիբուտներ են որոնք կարող եք գրանցել կոմպոնենտում։ Երբ արժեքը փոխանցված է prop ատրիբուտին, այն դառնում է հատկություն այդ կոմպոնենտի instance-ում։ Որպեսզի փոխանցենք վերնագիր մեր բլոգ գրառման կոմպոնենտին, մենք կարող ենք այն ներառել prop-ների ցանկում որը այս կոմպոնենտը ընդհունում է, օգտագործելով `props` ընտրանքը․

```js
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```

Կոմպոնենտը կարող է ունանալ այնքան prop-ներ որքան որ դուք ցանկանում եք և հիմնականում, ցանկացած արժեք կարող է փոխանցվել ցանկացած prop-ի։ Վերևում նշված ձևանմուշում, դուք կտեսնեք որ մենք կարող ենք օգտագործել այս արժեքը կոմպոնենտի instance-ում, ինչպես `data`-ն։

Երբ prop-ը գրանցված է, դուք կարող եք նրան փոխանցել տվյալներ որպես custom ատրիբուտ, այսպես․

```html
<blog-post title="Իմ ճամփորդությունը Vue-ի հետ"></blog-post>
<blog-post title="Բլոգումը Vue-ի հետ"></blog-post>
<blog-post title="Ի՞նչու Vue-ն այսքան զվարճալի է"></blog-post>
```

{% raw %}
<div id="blog-post-demo" class="demo">
  <blog-post1 title="Իմ ճամփորդությունը Vue-ի հետ"></blog-post1>
  <blog-post1 title="Բլոգումը Vue-ի հետ"></blog-post1>
  <blog-post1 title="Ի՞նչու Vue-ն այսքան զվարճալի է"></blog-post1>
</div>
<script>
Vue.component('blog-post1', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
new Vue({ el: '#blog-post-demo' })
</script>
{% endraw %}

Սովորական ծրագրում, շատ հավանական է որ կունենաք գրառումների զանգված `data`-ում․

```js
new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'Իմ ճամփորդությունը Vue-ի հետ' },
      { id: 2, title: 'Բլոգումը Vue-ի հետ' },
      { id: 3, title: 'Ի՞նչու Vue-ն այսքան զվարճալի է' }
    ]
  }
})
```

Հետո ցանկանաք render անել ամեն կոմպոնենտը․

```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>
```

Վերևում, դուք կտեսնեք որ մենք օգտագործում ենք `v-bind`-ը որպեսզի դինամիկորեն փոխանցենք prop-ներ։ Սա կարող է հատկապես օգտակար լինել երբ որ դուք չգիտեք կոնկրետ բովանդակությունը որը պետք է render լինի ժամանակից առաջ, ինչպես [գրառումների ստացումն է API-ից](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-component-blog-post-example)։

Սա ամենն է ինչ որ պետք է իմանաք prop-ների մասին այժմ, բայց երբ որ դուք վերջացրել էք կարդալ այս էջը և հարմարավետ էք ըզգում իր բովանդակությամբ, մենք խորհուրդ ենք տալիս վերադառնալ ավելի ուշ ամբողջությամբ կարդալու ուղեցույցը [Prop-ներ](components-props.html)։

## Մեկ Արմատային Էլեմենտ

Երբ կառուցում ենք `<blog-post>` կոմպոնենտ, ձեր ձևանմուշը ի վերջո կպարունակի ավելի քան մեկ վերնագիր․

```html
<h3>{{ title }}</h3>
```

ԵՎ ամենավերջում, դուք կցանկանաք ներառել գրառման բովանդակությունը․

```html
<h3>{{ title }}</h3>
<div v-html="content"></div>
```

Եթե դուք փորձեք սա ձեր ձևանմուշում, Vue-ն ցույց կտա սխալ, բացատրելով որ **every component must have a single root element (ամեն կոմպոնենտ պետք է ունենա մեկ արմատային էլեմենտ)**։ Որպեսզի ուղղենք այս սխալը պարզապես փաթաթեք ձևանմուշը ծնող էլեմենտում, ինչպես այստեղ․

```html
<div class="blog-post">
  <h3>{{ title }}</h3>
  <div v-html="content"></div>
</div>
```

Քանի որ մեր կոմպոնենտը աճում է, հավանական է մեզ միայն վերնագիրը և բովանդակությունը հերիք չէ գրառման համար, մեզ նաև պետք է հրատարակության ամսաթիվը, մեկնաբանությունները և ավելին։ Հայտարարելով prop ամեն հարաբերական ինֆորմացիային մասնիկի կարող է շատ նյարդայնացնել․

```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
  v-bind:content="post.content"
  v-bind:publishedAt="post.publishedAt"
  v-bind:comments="post.comments"
></blog-post>
```

Այս ժամանակ ճիշտ լինի ռեֆակտոր անել `<blog-post>` կոմպոնենտը որպեսզի ընդհունի մեկ `post` prop փոխարենը․

```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:post="post"
></blog-post>
```

```js
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <div v-html="post.content"></div>
    </div>
  `
})
```

<p class="tip">Վերևում նշված օրինակը և որոշ ապագա օրինակները օգտագործում են JavaScript-ի [template literal-ը](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) որպեսզի դարձնեն բազմագիծ ձևանմուշները ավելի կարդալի. Նրանք չեն համապատասխանում Internet Explorer-ի հետ (IE), այնպես որ եթե դուք պետք է համապատասխանացնեք IE-ի և transpile չէք անում (օրինակ՝ Babel կամ TypeScript), օգտագործեք [newline escape-ներ](https://css-tricks.com/snippets/javascript/multiline-string-variables-in-javascript/) փոխարենը։</p>

Հիմա, երբ նոր հատկություն է ավելացված `post` օբյեկտին, այն ավտոմատ կերպով հասանելի կլինի `<blog-post>`-ի մեջ։

## Ժառանգող Կոմպոնենտի Event-ների Լսումը

Երբ որ մենք զարգացնում ենք մեր `<blog-post>` կոմպոնենտը, որոշ հատկություններ պետք է կապ հաստատեն ծնողի հետ։ Օրինակի համար, մենք հնարավոր է որոշենք ներառել accessibility հատկություն որպեսզի մեծածնենք բլոգի գրառումների տեքստը, միաժամանակ թողնելով էջի մնացած մասը հիմնական չափով․

Ծնողում, մենք կարող ենք ստանալ այս հատկությունը ավելացնելով `postFontSize` տվյալների հատկություն․

```js
new Vue({
  el: '#blog-posts-events-demo',
  data: {
    posts: [/* ... */],
    postFontSize: 1
  }
})
```

Որը կարող է օգտագործվել ձևանմուշում որպեսզի կառավարել տեքստի չափսը բոլոր բլոգի գրառումների համար․

```html
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
    ></blog-post>
  </div>
</div>
```

Հիմա եկեք ավելացնենք կոճակ որպեսզի մեծածնենք տեքստը ամեն գրառման բովանդակությունից առաջ․

```js
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <button>
        Մեծացնել տեքստը
      </button>
      <div v-html="post.content"></div>
    </div>
  `
})
```

Խնդիրը այն է, որ այս կոճակը ոչ մի բան չի անի․

```html
<button>
  Մեծացնել տեքստը
</button>
```

Երբ որ մենք սեղմում ենք կոճակը, մենք պետք է կապնվենք ծնողի հետ որ այն մեծացնի տեքստը մեր բոլոր գրառումներում։ Բարեբախտաբար, Vue-ի instance-ները տրամադրում են custom event-ների համակարգ որպեսզի լուծվի այս խնդիրը։ Ծնողը կարող է ընտրել որպեսզի լսել ցանկացած event-ի որը գտնվում է ժառանգող կոմպոնենտի instance-ում `v-on`-ի շնորհիվ, ինչպես սովորական native DOM event-ում։

```html
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```

Այնուհետև ժառանգող կոմպոնենտը կարող է բաց թողնել event իր վրա կանչելով ներքին [**`$emit`** մեթոդը](../api/#vm-emit), փոխանցելով event-ի անունը․

```html
<button v-on:click="$emit('enlarge-text')">
  Մեծացնել տեքստը
</button>
```

Շնորհակալ լինելով `v-on:enlarge-text="postFontSize += 0.1"` լսողին, ծնողը կստանա event-ը և կթարմացնի `postFontSize`-ի արժեքը։

{% raw %}
<div id="blog-posts-events-demo" class="demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
      v-on:enlarge-text="postFontSize += 0.1"
    ></blog-post>
  </div>
</div>
<script>
Vue.component('blog-post', {
  props: ['post'],
  template: '\
    <div class="blog-post">\
      <h3>{{ post.title }}</h3>\
      <button v-on:click="$emit(\'enlarge-text\')">\
        Enlarge text\
      </button>\
      <div v-html="post.content"></div>\
    </div>\
  '
})
new Vue({
  el: '#blog-posts-events-demo',
  data: {
    posts: [
      { id: 1, title: 'Իմ ճամփորդությունը Vue-ի հետ', content: '...բովանդակություն...' },
      { id: 2, title: 'Բլոգումը Vue-ի հետ', content: '...բովանդակություն...' },
      { id: 3, title: 'Ի՞նչու Vue-ն այսքան զվարճալի է', content: '...բովանդակություն...' }
    ],
    postFontSize: 1
  }
})
</script>
{% endraw %}

### Արժեքի Բաց Թողումը Event-ի Հետ

Շատ դեպքերում օգտակար է բաց թողնել կոնկրետ արժեք event-ի հետ։ Օրինակի համար, մենք ցանկանում ենք `<blog-post>` կոմպոնենտը պատասխանատու լինի թե ինչքանով տեքստը պետք է մեծանա։ Այս դեպքերում, մենք կարող ենք օգտագործել `$emit`-ի 2-րդ պարամետրը որպեսզի տրամադրենք այս արժեքը․

```html
<button v-on:click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>
```

Հետո մենք կլսենք այս event-ին ծնողում, մենք պետք է ստանանք բաց թողնված event-ի արժեքը `$event`-ով․

```html
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
```

Կամ, եթե event-ի handler-ը մեթոդ է․

```html
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>
```

Հետո արժեքը կփոխանցվի որպես առաջին պարամետր այդ մեթոդի․

```js
methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```

### Օգտագործելով `v-model`-ը Կոմպոնենտների Վրա

Custom event—ներ կարող են նաև օգտագործվել որպեսզի ստեղծել custom input-ներ որոնք աշխատում են `v-model`-ի հետ։ Հիշեք դա․

```html
<input v-model="searchText">
```

անում է նույն բանը ինչ․

```html
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```

Երբ օգտագործվում է կոմպոնենտում, `v-model` անում է հետևյալը․

``` html
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```

Որպեսզի սա աշխատի, `<input>` կոմպոնենտի ներսում պետք է․

- Կապի `value` ատրիբուտը `value` prop-ին
- `input`-ի ժամանակ, բաց թողնի իր custom `input` event-ը նոր արժեքով

Այստեղ է օրինակը․

```js
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```

Հիմա `v-model`-ը պետք է աշխատի հիանալի այս կոմպոնենտի հետ․

```html
<custom-input v-model="searchText"></custom-input>
```

Սա ամենն է ինչ որ պետք է իմանաք custom կոմպոնենտի event-ների մասին այժմ, բայց երբ որ դուք վերջացրել էք կարդալ այս էջը և հարմարավետ եք ըզգում իր բովանդակությամբ, մենք խորհուրդ ենք տալիս վերադառնալ ավելի ուշ ամբողջությամբ կարդալու ուղեցույցը [Custom Event-ներում](components-props.html)։

## Բովանդակության Բաշխումը Սլոտների Հետ

Ինչպես HTML էլեմենտներում, շատ հաճախ օգտակար ունենալ հնարավորություն փոխանցելու բովանդակություն կոմպոնենտին, ինչպես այստեղ․

``` html
<alert-box>
  Ինչ որ մի վատ բան եղավ։
</alert-box>
```

Որը կարող է render անել նման մի բան․

{% raw %}
<div id="slots-demo" class="demo">
  <alert-box>
    Ինչ որ մի վատ բան եղավ։
  </alert-box>
</div>
<script>
Vue.component('alert-box', {
  template: '\
    <div class="demo-alert-box">\
      <strong>Սխալ!</strong>\
      <slot></slot>\
    </div>\
  '
})
new Vue({ el: '#slots-demo' })
</script>
<style>
.demo-alert-box {
  padding: 10px 20px;
  background: #f3beb8;
  border: 1px solid #f09898;
}
</style>
{% endraw %}

Բարեբախտորեն, այս հանձնարարությունը ավելի է հեշտանում Vue-ի custom `<slot>` էլեմենտով․

```js
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Սխալ!</strong>
      <slot></slot>
    </div>
  `
})
```

Ինչպես տեսնում եք վերևում, մենք ուղակի ավելացրեցին սլոտը որտեղ մենք ցանկանում ենք որ այն լինի -- և վերջ։ Մենք ավարտեցին՜

Սա ամենն է ինչ որ պետք է իմանաք սլոտների մասին այժմ, բայց երբ որ դուք վերջացրել էք կարդալ այս էջը և հարմարավետ եք ըզգում իր բովանդակությամբ, մենք խորհուրդ ենք տալիս վերադառնալ ավելի ուշ ամբողջությամբ կարդալու ուղեցույցը [Սլոտներում](components-props.html)։

## Դինամիկ Կոմպոնենտներ

Հաճախ, օգտակար է դինամիկորեն փոփոխել կոմպոնենտները, ինչպես tab-ված interface-ում․

{% raw %}
<div id="dynamic-component-demo" class="demo">
  <button
    v-for="tab in tabs"
    v-bind:key="tab"
    class="dynamic-component-demo-tab-button"
    v-bind:class="{ 'dynamic-component-demo-tab-button-active': tab === currentTab }"
    v-on:click="currentTab = tab"
  >
    {{ tab }}
  </button>
  <component
    v-bind:is="currentTabComponent"
    class="dynamic-component-demo-tab"
  ></component>
</div>
<script>
Vue.component('tab-home', { template: '<div>Home component</div>' })
Vue.component('tab-posts', { template: '<div>Posts component</div>' })
Vue.component('tab-archive', { template: '<div>Archive component</div>' })
new Vue({
  el: '#dynamic-component-demo',
  data: {
    currentTab: 'Home',
    tabs: ['Home', 'Posts', 'Archive']
  },
  computed: {
    currentTabComponent: function () {
      return 'tab-' + this.currentTab.toLowerCase()
    }
  }
})
</script>
<style>
.dynamic-component-demo-tab-button {
  padding: 6px 10px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  border: 1px solid #ccc;
  cursor: pointer;
  background: #f0f0f0;
  margin-bottom: -1px;
  margin-right: -1px;
}
.dynamic-component-demo-tab-button:hover {
  background: #e0e0e0;
}
.dynamic-component-demo-tab-button-active {
  background: #e0e0e0;
}
.dynamic-component-demo-tab {
  border: 1px solid #ccc;
  padding: 10px;
}
</style>
{% endraw %}

Վերևում օրինակը հաջողվել է օգտագործելով Vue-ի `<component>` էլեմենտը `is` հատուկ ատրիբուտի հետ․

```html
<!-- Կոմպոնենտը փոփոխվում է երբ currentTabComponent-ը փոփոխվում է -->
<component v-bind:is="currentTabComponent"></component>
```

Վերևի օրինակում, `currentTabComponent`-ը կարող է պարունակի կամ․

- անունը գրանցված կոմպոնենտի, կամ
- կոմպոնենտնի ընտրանքների օբյեկտը

Նայեք [այս օրինակը](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-dynamic-components) որպեսզի փորձարկել ամբողաջական կոդի հետ, կամ [այս տարբերակը](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-dynamic-components-with-binding) կոմպոնենտի ընտրանքների կապման օրինակի համար, ի փոխարեն գրանցված անունի։

Մտքում պահեք որ այս ատրիբուտը կարող է օգտագործվել սովորական HTML էլեմենտներում, սակայն նրա հետ պետք է վարվել որպես կոմպոնենտ, որը նշանակում է որ բոլոր ատրիբուտները **կկապնվեն որպես DOM ատրիբուտներ**։ Որոշ հատկություններ ինչպիսին է `value`-ն որպեսզի աշխատի ինչպես որ պատկերացնում ենք, դուք պետք է կապեք նրանց օգտագործելով [`.prop` փոփոխիչը](../api/#v-bind)։

Սա ամենն է ինչ որ պետք է իմանաք դինամիկ կոմպոնենտների մասին այժմ, բայց երբ որ դուք վերջացրել էք կարդալ այս էջը և հարմարավետ եք ըզգում իր բովանդակությամբ, մենք խորհուրդ ենք տալիս վերադառնալ ավելի ուշ ամբողջությամբ կարդալու ուղեցույցը [Դինամիկ և Async Կոմպոնենտներում](components-props.html)։


## DOM Ձևանմուշի Parsing-ի Զգուշացումները

Որոշ HTML էլեմենտներ, ինչպիսին են `<ul>`-ը, `<ol>`-ը, `<table>`-ը և `<select>`-ը ունեն սահմանափակումներ թե ինչ էլեմենտներ կարող են հայտնվել իրենց մեջ, և որոշ էլեմենտներ ինչպիսին են `<li>`-ն, `<tr>`-ը, և `<option>`—ը կարող են միայն հայտնվել միայն որոշ այլ էլեմենտների մեջ։

Սա կառաձացնի խնդիրներ երբ օգտագործում ենք կոմպոնենտներ էլեմենտների հետ որոնք ունեն այդպիսի սահմանափակումներ։ Օրինակի համար․

``` html
<table>
  <blog-post-row></blog-post-row>
</table>
```

`<blod-post-row>` Custom կոմպոնենտ-ը կհեռացվի որպես անվավեր բովանդակություն, ստեղծելով սխալներ ի վերջո մատուցված ելքում։ Բարեբախտորեն, `is` հատուկ ատրիբուտը առաջարկում է շրջանցում․

``` html
<table>
  <tr is="blog-post-row"></tr>
</table>
```

Պետք է նշվի որ **այս սահմանափակումը __չի__ կիրառվում եթե դուք օգտագործում եք տեքստային ձևանմուշներ հետևյալ աղբյուրնեից մեկից**․

- String ձևանմուշներ (օրինակ՝ `template: '...'`)
- [Մեկ ֆայլ (`.vue`) կոմպոնենտներ](single-file-components.html)
- [`<script type="text/x-template">`](components-edge-cases.html#X-Templates)

Սա այն ամենն է ինչ որ պետք է իմանաք DOM ձևանմուշների parsing-ի զգուշացումների մասին այժմ -- և իրականում, սա վերջն է Vue-ի _Հիմնական Նյութերի_։ Շնորհավորում եմ Ձեզ՜ Ավելի շատ բան կա սովորելու համար, բայց սկզբում, մենք խորհուրդ ենք տալիս ընդմիջում տալ որպեսզի խախալ Vue-ի հետ և ստեղծել ինչ որ մի զվարճալի մի բան։

Երբ որ դուք հանգիստ էք ըզգում գիտելիքների մասին որոնք դուք մարսելեք, մենք խորհուրդ ենք տալիս վերադառնալ որպեսզի կարդալ աբողջ ուղեցույցը [Դինամիկ և Async Կոմպոնենտներում](components-dynamic-async.html), ինչպես նաև այլ էջերը Կոմպոնենտները Խորացված բաժնում որը գտնվում է sidebar-ում։
