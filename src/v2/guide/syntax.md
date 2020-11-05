---
title: Ձևանմուշի Գրելաձև
type: ուղեցույց
order: 4
---

Vue.js֊ը օգտագործում է HTML-ով հիմնված ձևանմուշի գրելաձև որը թույլ է տալիս ձեզ հայտարարման կերպով կապել render եղած DOM֊ը դեպի Vue instance֊ի ներքին տվյալներին։ Բոլոր Vue.js֊ի ձևանմուշները ճիշտ HTML֊ներ են որոնք կարող են կարդացվել համապատասխանող բրաուզերներում և HTML կարդացողներում։

Ներքինում, Vue֊ն compile է անում ձևանմուշը դեպի Virtual DOM render ֆունկցիաներ։ Ի հանդերձ ռեակտիվության համակարգի, Vue֊ն կարողանում է խելացիորեն հասկանալու մինիմալ քանակը կոմպոնենտների որոնք պետք է կրկին render արվեն և կիրառել մինիմալ քանակի DOM մանիպուլացիաներ երբ ծրագրի state֊ը փոփոխվում է։

Եթե դուք ծանոթ եք Virtual DOM֊ի հասկացությունների հետ և նախընտրում եք JavaScript֊ի անմշակ ուժը, դուք նաև կարող եք [ուղիղ գրել render ֆունկցիաներ](render-function.html) ձևանմուշների փոխարեն, ոչ պարտադիր JSX֊ի համատասխանության հետ։

## Ինտերպոլացիաներ

### Տեքստ

Ամենահիմնական ձևը տվյալների կապման դա տեքստի ինտերպոլացիան է օգտագործելով «Բեղավոր» գրելաձևը (զույգ ձևավոր փակագծեր)․

``` html
<span>Նամակ: {{ msg }}</span>
```

Բեղավոր tag֊ը կփոխարինվի `msg` հատկության արժեքով համապտասխան տվյալների օբյեկտի վրա։ Այն նաև կթարմացնի երբ տվյալների օբյեկտի `msg` հատկությունը փոփոխվի։

Դուք կարող եք կատարել մեկ անգամյա ինտերպոլացիաներ որոնք չեն թարմացվում տվյալների փոփոխություններում օգտագործելով [v-once ուղղորդիչը](../api/#v-once), բայց մտքում պահեք որ սա նաև կազդի ցանկացած այլ կապումների վրա որոնք կատարվում են նույն node֊ի վրա․

``` html
<span v-once>Սա երբեք չի փոփոխվի: {{ msg }}</span>
```

### Չոր HTML

Զույգ բեղերը վերարտադրում է տվյալները որպես չոր տեքստ, այլ ոչ թե HTML։ Որպեսզի ելքագրել իրական HTML, դուք պետք է օգտագործեք [`v-html` ուղղորդիչը](../api/#v-html)․

``` html
<p>Օգտագործելով բեղերը: {{ rawHtml }}</p>
<p>Օգտագործելով v-html ուղղորդիչը: <span v-html="rawHtml"></span></p>
```

{% raw %}
<div id="example1" class="demo">
  <p>Օգտագործելով բեղերը: {{ rawHtml }}</p>
  <p>Օգտագործելով v-html ուղղորդիչը: <span v-html="rawHtml"></span></p>
</div>
<script>
new Vue({
  el: '#example1',
  data: function () {
    return {
      rawHtml: '<span style="color: red">Սա պետք է լինի կարմիր։</span>'
    }
  }
})
</script>
{% endraw %}

`span`-ի բովանդակությունը կփոխարինվի `rawHtml` հատկության արժեքով, վերարտադրված որպես չոր հասարակ HTML - տվյալների կապումները անտեսվում են։ Նշում որ դուք չեք կարող օգտագործել `v-html`֊ը որպեսզի գրել ձևանմուշի մասեր, որովհետև Vue֊ի տեքստով հենված ձևանմուշային գործիք չէ։ Փոխարենը, կոմպոնենտները նախտրելի են որպես հիմնական հիմք UI֊ի վերօգտագործման և կազմելու համար։

<p class="tip">Դինամիկորեն HTML render անելը ձեր կայքում կարող է լինել շատ վտանգավոր որովհետև այն հեշտորեն կարող է հանգեցնել [XSS խոցելիությունների](https://en.wikipedia.org/wiki/Cross-site_scripting)։ Միայն օգտագործեք HTML ինտերպոլացիաներ հավաստալի բովանդակություններում և **երբեք** չօգտագործեք այն օգտագործողի կողմից տրամադրվող բովանդակություններում։</p>

### Ատրիբուտներ

Բեղերը չեն կարող օգտագործվել HTML ատրիբուտների մեջ։ Փոխարենը, օգտագործեք [`v-bind` ուղղորդիչը](../api/#v-bind)․

``` html
<div v-bind:id="dynamicId"></div>
```

Boolean ատրիբուտների դեպքում, որտեղ նրանց գոյությունը վկայում է `true`, `v-bind`֊ը աշխատում է մի փոքր ուրիշ ձևերով այստեղ։ Այս օրինակում․

``` html
<button v-bind:disabled="isButtonDisabled">Կոճակ</button>
```

Եթե `isButtonDisabled` ունի `null`, `undefined`, կամ `false` ատրիբուտներից որևէ մեկը, `disabled` ատրիբուտը նույնիսկ չի ներառվի render եղած `<button>` էլեմենտում։

### JavaScript֊ի Արտահայտությունների Օգտագործումը

Մինչ այժմ մենք միայն կապելենք հասարակ հատկությունների բանալիներ մեր ձևանմուշում։ Բայց Vue.js֊ը իրականում համապատասխանում է JavaScript արտահայտությունների ամբողջ ուժին բոլոր տվյալների կապումների ներսում․

``` html
{{ number + 1 }}

{{ ok ? 'Այո' : 'Ոչ' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

Այս արտահայտությունները կվերահաշվարկվեն որպես JavaScript տվյալների scope֊ում տնօրինող Vue instance֊ի։ Մեկ սահմանափակումը դա երբ որ ամեն կապումը կարող է միայն պարունակի **մեկ արտահայտություն**, այնպես որ հետևյալը **ՉԻ** աշխատի․

``` html
<!-- սա հայտարարում է, այլ ոչ թե արտահայտություն -->
{{ var a = 1 }}

<!-- հոսքի կառավարումը նույնպես չի աշխատի, օգտագործեք երեքական արտահայտությունները -->
{{ if (ok) { return message } }}
```

<p class="tip">Ձևանմուշի արտահայտությունները սահմանափակված են և միայն ունեն հնարավորություն օգտագործելու [գլոբալների սպիտակ ցուցակ](https://github.com/vuejs/vue/blob/v2.6.10/src/core/instance/proxy.js#L9) ինչպիսին են `Math`֊ը և `Date`֊ը։ Դուք չպետք է փորձեք մուտք գործել դեպի օգտագործողի կողմից հայտարարված գլոբալներ ձեր ձևանմուշի արտահայտություններում։</p>

## Ուղղորդիչներ

Ուղղորդիչները հատուկ ատրիբուտներ են `v-` նախածանցով։ Ուղղորդիչ ատրիբուտները ակնկալվում է որպես **միայնակ JavaScript արտահայտություն** (բացառությամբ `v-for`֊ի, որը մենք կքնարկենք հետո)։ Ուղղորդիչի գործը դա այն է որ ռեակտիվորեն կիրառի կողմնակի ազդեցությունները DOM֊ին երբ արտահայտության արժեքը փոփոխվում է։ Եկեք վերանայենք օրինակը որը մենք տեսել եինք ներածությունում․

``` html
<p v-if="seen">Հիմա դու ինձ տեսնում էս</p>
```

Այստեղ, `v-if` ուղղորդիչը կջնջի/տեղադրի `<p>` էլեմենտը կախված `seen` արտահայտության արժեքի ճշմարիտ լինելուց։

### Արգումենտներ

Որոշ ուղղորդիչներ կարող են ստանալ «արգումենտ», որը նշվում է վերջակետով ուղղորդիչի անունից հետո։ Օրինակի համար, `v-bind` ուղղորդիչը օգտագործվում է որպեսզի ռեակտիվորեն թարմացնի HTML ատրիբուտը․

``` html
<a v-bind:href="url"> ... </a>
```

Այստեղ `href`֊ը արգումենտն է, որը տեղյակ է պահում `v-bind` ուղղորդիչին որպեսզի կապի էլեմենտի `href` ատրիբուտը `url` արտահայտության արժեքի հետ։

Մեկ այլ օրինակ դա `v-on` ուղղորդիչն է, որը լսում է DOM event֊ներին․

``` html
<a v-on:click="doSomething"> ... </a>
```

Այստեղ արգումենտը event֊ի անունն է որին պետք է լսել։ Մենկ կքնարկենք event֊ի գործարկման մասին ավելի մանրամասն նույնպես։

### Դինամիկ Արգումենտներ

> Նոր 2.6.0+֊ի մեջ

Սկսած 2.6.0 տարբերակից, նաև հնարավոր է օգտագործել JavaScript արտահայտություն ուղղորդիչի արգումենտում փաթաթելով այն քառակուսի փակագծերով․

``` html
<!--
Նշում որ կան որոշ կախվածություններ արգումենտի արտահայտությանը, ինչպես բացատրվում է "Դինամիկ Արգումենտի Արտահայտության Կախվածությունները" ներքևի բաժնում։
-->
<a v-bind:[attributeName]="url"> ... </a>
```

Այստեղ `attributeName`֊ը դինամիկորեն կհաշվարկվի որպես JavaScript արտահայտություն, և նրա հաշվարկված արժեքը կօգտագործվի որպես վերջնական արժեք արգումենտի համար։ Օրինակի համար, եթե ձեր Vue instance֊ը ունի տվյալների հատկություն, `attributeName`, որի արժեքը `"href"` է, այնուհետև այս կապումը հավասար կլինի `v-bind:href`֊ի։

Similarly, you can use dynamic arguments to bind a handler to a dynamic event name:

``` html
<a v-on:[eventName]="doSomething"> ... </a>
```

In this example, when `eventName`'s value is `"focus"`, `v-on:[eventName]` will be equivalent to `v-on:focus`.

#### Dynamic Argument Value Constraints

Dynamic arguments are expected to evaluate to a string, with the exception of `null`. The special value `null` can be used to explicitly remove the binding. Any other non-string value will trigger a warning.

#### Dynamic Argument Expression Constraints

Dynamic argument expressions have some syntax constraints because certain characters, such as spaces and quotes, are invalid inside HTML attribute names. For example, the following is invalid:

``` html
<!-- This will trigger a compiler warning. -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

The workaround is to either use expressions without spaces or quotes, or replace the complex expression with a computed property.

When using in-DOM templates (i.e., templates written directly in an HTML file), you should also avoid naming keys with uppercase characters, as browsers will coerce attribute names into lowercase:

``` html
<!--
This will be converted to v-bind:[someattr] in in-DOM templates.
Unless you have a "someattr" property in your instance, your code won't work.
-->
<a v-bind:[someAttr]="value"> ... </a>
```

### Modifiers

Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. For example, the `.prevent` modifier tells the `v-on` directive to call `event.preventDefault()` on the triggered event:

``` html
<form v-on:submit.prevent="onSubmit"> ... </form>
```

You'll see other examples of modifiers later, [for `v-on`](events.html#Event-Modifiers) and [for `v-model`](forms.html#Modifiers), when we explore those features.

## Shorthands

The `v-` prefix serves as a visual cue for identifying Vue-specific attributes in your templates. This is useful when you are using Vue.js to apply dynamic behavior to some existing markup, but can feel verbose for some frequently used directives. At the same time, the need for the `v-` prefix becomes less important when you are building a [SPA](https://en.wikipedia.org/wiki/Single-page_application), where Vue manages every template. Therefore, Vue provides special shorthands for two of the most often used directives, `v-bind` and `v-on`:

### `v-bind` Shorthand

``` html
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a :[key]="url"> ... </a>
```

### `v-on` Shorthand

``` html
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

They may look a bit different from normal HTML, but `:` and `@` are valid characters for attribute names and all Vue-supported browsers can parse it correctly. In addition, they do not appear in the final rendered markup. The shorthand syntax is totally optional, but you will likely appreciate it when you learn more about its usage later.
