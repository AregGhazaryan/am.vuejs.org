---
title: Սլոտներ
type: guide
order: 104
---

> Այս էջը ենթադրում է որ դուք արդեն կարդացել եք [Կոմպոնենտների Հիմունքները](components.html)։ Կարդացեք այն եթե դուք նոր եք ծանոթանում կոմպոնենտներին։

> 2.6.0-ի մեջ, մենք ներկայացրել ենք նոր միակցված գրելաձև (`v-slot` ուղղորդիչը) անվանված և scope-ված սլոտների համար։ Այն փոփոխում է `slot` և `slot-scope` ատրիբուտները, որոնք այժմ depricated են, բայց նրանք _չեն_ ջնջվել և մինջ դեռ փաստաթղթված են [այստեղ](#Deprecated-Syntax)։ Հիմանվորումը նոր գրելաձևի ներկայացման համար նկարագրված է այս [RFC-ում](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0001-new-slot-syntax.md)։

## Սլոտի Բովանդակություն

Vue-ն պարունակում է բովանդակության բաշխման API որը ոգեշնչված [Web Կոմպոնենտների նշանակությունների նմուշից](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md), օգտագործելով `<slot>` էլեմենտը որպեսզի մատուցել որպես բաշխման ելքագրում բովանդակության համար։

Սա ձեզ թույլ է տալիս ստեղծել կոմպոնենտներ այսպես․

``` html
<navigation-link url="/profile">
  Ձեր Էջը
</navigation-link>
```

Այնուհետև այս ձևանմուշում `<navigation-link>`—ի համար, դուք կարող եք ունենալ․

``` html
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
```

Երբ կոմպոնենտը render է լինում, `<slot></slot>`-ը կփոփոխվի «Ձեր Էջը» տեքստով։ Սլոտները կարող են պարունակել ցանկացած ձևանմուշի կոդ, ներառյալ HTML:

``` html
<navigation-link url="/profile">
  <!-- Ավելացնել Font Awesome icon -->
  <span class="fa fa-user"></span>
  Ձեր Էջը
</navigation-link>
```

Կամ նույնիսկ ուրիշ կոմպոնենտներ․

``` html
<navigation-link url="/profile">
  <!-- Օգտագործել կոմպոնենտ որպեսզի ավելացնել icon -->
  <font-awesome-icon name="user"></font-awesome-icon>
  Ձեր Էջը
</navigation-link>
```

Եթե `<navigtaion-link>`-ի ձևանմուշը **չի** պարունակում `<slot>` էլեմենտ, ցանկացած բովանդակություն որը տեղադրված է իր բացվող և փակվող tag-ի մեջտեղում կհեռացվի։

## Կազմային Scope

Երբ որ դուք ուզում եք օգտագործել տվյալներ սլոտի մեջ, ինչպիսին է հետևյալը․

``` html
<navigation-link url="/profile">
  Մուտք է գործված որպես {{ user.name }}
</navigation-link>
```

Այդ սլոտը ունի մուտք դեպի նույն instance-ի հատկանիշներ (օրինակ նույն «scope»-ը) որպես մնացած մասը ձևանմուշի։ Սլոտը **չունի** մուտք դեպի `<navigation-link>`-ի scope: Օրինակի համար, փորձելով մտնել դեպի `url` պարզապես չի աշխատի․

``` html
<navigation-link url="/profile">
  Սեղմելով այստեղ դուք կգնաք դեպի: {{ url }}
  <!--
  `url`-ը կլինի undefined, որովհետև այս բովանդակություն է
  տրված <navigation-link>, ի փոխարեն հայտարարված <navigation-link>
  կոմպոնենտի մեջ։
  -->
</navigation-link>
```

Որպես կանոն, հիշեք որ․

> Ամեն ինչ ծնողի ձևանմուշում compile է եղած ծնողի scope-ում; ամեն ինչ ժառանգողի ձևանմուշում compile է եղած ժառանգողի scope-ում։

## Ընկման Բովանդակություն

Կան դեպքեր երբ հարկավոր է հատկացնել ընկման (օրինակ՝ հիմնական) բովանդակություն սլոտի համար, որը պետք է render լինի երբ բովանդակությունը չի տրամադրված։ Օրինակ՝ `<submit-button>` կոմպոնենտը։

```html
<button type="submit">
  <slot></slot>
</button>
```

Մենք հնարավոր է որ կցանականանք «Submit» տեքստը render լինի `<button>`-ի մեջ հիմնականում։ Որպեսզի նշանակել «Submit» որպես ընկման բովանդակություն, մենք կարող ենք տեղադրել այն `<slot>` tag-երի միջև։

```html
<button type="submit">
  <slot>Submit</slot>
</button>
```

Հիմա երբ որ օգտագործենք `<submit-button>`-ը ծնող կոմպոնենտում, առանց տրամադրելու բովանդակություն սլոտի համար։

```html
<submit-button></submit-button>
```

Ապա այն render կանի ընկման բովանդակությունը, «Submit»․

```html
<button type="submit">
  Submit
</button>
```

Բայց եթե մենք տրամադրենք բովանդակություն․

```html
<submit-button>
  Save
</submit-button>
```

Ապա տրամադրված բովանդակությունը render կլինի փոխարենը․

```html
<button type="submit">
  Save
</button>
```

## Անվանված Սլոտներ

> Թարմեցված 2.6.0+-ի մեջ։ [Նայեք այստեղ](#Deprecated-Syntax) հին գրելաձևի համար օգտագործելով `slot` ատրիբուտը։

Կան նաև դեպքեր երբ օգտակար է ունենալ մի քանի սլոտներ։ Օրինակի համար, `<base-layout>` կոմպոնենտում հետևյալ ձևանմուշի հետ հանդերձ։

``` html
<div class="container">
  <header>
    <!-- Մենք ուզում ենք header-ի բովանդակությունը այստեղ -->
  </header>
  <main>
    <!-- Մենք ուզում ենք main-ի բովանդակությունը այստեղ -->
  </main>
  <footer>
    <!-- Մենք ուզում ենք footer-ի բովանդակությունը այստեղ -->
  </footer>
</div>
```

Այս դեպքերում, `<slot>` էլեմենտը ունի հատուկ ատրիբուտ, `name`, որը կարող է օգտագործվել որպեսզի հայտարարել հավելյալ սլոտներ։

``` html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

`<slot>` ելքագրիչը առանց `name`-ի կստանա «default» անունը։

Որպեսզի տրամադրենք բովանդակություն անվանված սլոտներին, մենք կարող ենք օգտագործել `v-slot` ուղղորդիչը `<template>`-ի վրա, տրամադրելով անունը սլոտի ինչպես `v-slot`-ի արգումենտում է նշված։

```html
<base-layout>
  <template v-slot:header>
    <h1>Այստեղ կարող է լինել էջի վերնագիրը</h1>
  </template>

  <p>Պարբերություն հիմնական բովանդակության համար։</p>
  <p>Հավելյալ պարբերություն։</p>

  <template v-slot:footer>
    <p>Այստեղ կարող է լինել կոնտակտային ինֆորմացիա</p>
  </template>
</base-layout>
```

Հիմա ամեն ինչ `<template>` էլեմենտների մեջ կփոխանցվի համապատասխան սլոտների։ Ցանկացած բովանդակություն որը փաթաթված չէ `<template>`-ի մեջ օգտագործելով `v-slot`-ը ենթադրվում է որ այն նախատեսված է հիմնական սլոտի համար։

Սակայն, դուք դեռ կարող եք փաթաթել հիմնական սլոտի բովանդակությունը `<template>`-ի մեջ եթե ցանկանում եք որպեսզի այն լինի ավելի կոնկրետ։

```html
<base-layout>
  <template v-slot:header>
    <h1>Այստեղ կարող է լինել էջի վերնագիրը</h1>
  </template>

  <template v-slot:default>
    <p>Պարբերություն հիմնական բովանդակության համար։</p>
    <p>Հավելյալ պարբերություն։</p>
  </template>

  <template v-slot:footer>
    <p>Այստեղ կարող է լինել կոնտակտային ինֆորմացիա</p>
  </template>
</base-layout>
```

Երկու դեպքում, render եղած HTML-ը կլինի այսպիսին․

``` html
<div class="container">
  <header>
    <h1>Այստեղ կարող է լինել էջի վերնագիրը</h1>
  </header>
  <main>
    <p>Պարբերություն հիմնական բովանդակության համար։</p>
    <p>Հավելյալ պարբերություն։</p>
  </main>
  <footer>
    <p>Այստեղ կարող է լինել կոնտակտային ինֆորմացիա</p>
  </footer>
</div>
```

Նշում որ **`v-slot`-ը կարող է միայն ավելացվել `<template>`-ին** (մեկ [բացառությամբ](#Abbreviated-Syntax-for-Lone-Default-Slots)), ի տարբերություն հին [`slot` ատրիբուտի](#Deprecated-Syntax)։

## Scope-ված Սլոտներ

> Թարմացված 2.6.0+-ի մեջ. [Նայեք այստեղ](#Deprecated-Syntax) հին գրելաձևի համար օգտագործելով `slot-scope` ատրիբուտը։

Երբեմն, օգտակար է սլոտի բովանդակության համար ունենալ մուտք դեպի տվյալներ որոնք միայն հասանելի են ժառանգող կոմպոնենտում։ Օրինակ, պատկերացրեք `<current-user>` կոմպոնենտ հետևյալ ձևանմուշով․

```html
<span>
  <slot>{{ user.lastName }}</slot>
</span>
```

Մենք հնարավոր է ցանկանանք փոփոխել այս ընկման բովանդակությունը որպեսզի ցույց տալ օգտագործողի անունը, ազգանունի փոխարեն, այսպես․

``` html
<current-user>
  {{ user.firstName }}
</current-user>
```

Սա չի աշխատի, սակայն, որովհետև միայն `<current-user>` կոմպոնենտը կարող է մուտք գործել դեպի `user` և մեր տրամադրած բովանդակությունը render է եղած ծնողի մեջ։

Որպեսզի `user`-ը հասանելի դարձնել սլոտի բովանդակության մեջ որը գտնվում է ծնողում, մենք կարող ենք կապել `user`-ը որպես `<slot>` էլեմենտի ատրիբուտ։

``` html
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```

Ատրիբուտները կապված `<slot>` էլեմենտին կոչվում են **սլոտի prop—ներ**։ Հիմա, ծնողի scope-ում, մենք կարող ենք օգտագործել `v-slot`-ը արժեքով հանդերձ որպեսզի հայտարարենք անուն սլոտ prop-ի համար որը մենք տրամադրել ենք։

``` html
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```

Այս օրինակում, մենք ընտրել ենք որ անվանենք օբյեկտը որը պարունակում է մեր բոլոր սլոտի prop-ները `slotProps`, բայց դուք կարող եք օգտագործել ցանկացած անուն։

### Կրճատված Գրելաձև Միայնակ Հիմնական Սլոտների Համար

Վերևում նշված դեպքերում, երբ _միայն_ հիմնական սլոտն է տրամադրած բովանդակությունը, կոմպոնենտի tag-երը կարող են օգտագործվել որպես սլոտի ձևանմուշներ։ Սա թույլ է տալիս մեզ օգտագործել `v-slot`-ը ուղիղ կոմպոնենտի վրա։

``` html
<current-user v-slot:default="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

Սա կարող է կրճատվել ավելին։ Ուղակի չնշված բովանդակությունը համարվում է որպես հիմնական սլոտի համար, `v-slot`-ը արանց արգումենտի համարվում է դիմում հիմնական սլոտին։

``` html
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

Նշում որ կրճատված գրելաձևը հիմնական սլոտի համար **չի** կարող խառնվել անվանված սլոտների հետ, որովհետև այն կառաջացնի scope-ի անորոշություն։

``` html
<!-- ՍԽԱԼ, կառաջացնի նախազգուշացում -->
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
  <template v-slot:other="otherSlotProps">
    slotProps-ը հասանելի չէ այստեղ
  </template>
</current-user>
```

Երբ կան նաև բազմաթիվ սլոտները, օգտագործել ամբողջ `<template>` գրելաձևը _բոլոր_ սլոտների համար։

``` html
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</current-user>
```

### Սլոտ Prop-ների Ապակառուցումը

Ներքինում, scope-ված սլոտները աշխատում են փաթաթվելով ձեր սլոտի բովանդակությանը ֆունկցիայում փոխանցված մեկ արգումենտով։

```js
function (slotProps) {
  // ... սլոտի բովանդակություն ...
}
```

Սա նշանակում է `v-slot`-ի արժեքը իրականում կարող է ընդհունել ցանկցած ճիշտ JavaScript արտահայտություն որը կարող է հայտնվել ֆունկցիայի հայտարարման արգումենտում։ Այնպես որ համապատասխանող enviroment-ներում ([մեկ ֆայլ կոմպոնենտներում](single-file-components.html) կամ [ժամանակակից բրաուզերներում](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Browser_compatibility)), դուք նաև կարող եք օգտագործել [ES2015 ապակառուցում](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Object_destructuring) որպեսզի դուրս հանեք կոնկրետ սլոտի prop-ներ, ինչպես այստեղ․

``` html
<current-user v-slot="{ user }">
  {{ user.firstName }}
</current-user>
```

Սա կարող է դարձնել ձևանմուշը ավելի մաքուր, հատկապես երբ սլոտը տրամադրում է բազմաթիվ prop-ներ։ Սա կարող է բացել նաև այլ հնարավորություններ, ինչպիսիք են prop-ների վերանվանումը օրինակ՝ `user`-ը դեպի `person`։

``` html
<current-user v-slot="{ user: person }">
  {{ person.firstName }}
</current-user>
```

Դուք կարող եք նաև հայտարարել ընկումներ, որ օգտագործվի այն ժամանակ երբ սլոտի prop-ը undefined է։

``` html
<current-user v-slot="{ user = { firstName: 'Հյուր' } }">
  {{ user.firstName }}
</current-user>
```

## Դինամիկ Սլոտի Անուններ

> Նոր 2.6.0+-ի մեջ

[Դինամիկ ուղղորդիչ արգումենտները](syntax.html#Dynamic-Arguments) նաև աշխատում է `v-slot`—ի վրա, թույլ տալով դինամիկ սլոտի անունների հայտարարումը․

``` html
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

## Անվանված Սլոտներ Կարճ

> Նոր 2.6.0+-ի մեջ

Նման `v-on`-ին և `v-bind`-ին, `v-slot` նաև ունի կարճ գրելաձև, փոփոխելով ամեն ինչ արգումենտից առաջ (`v-slot:`) հատուկ նշանի հետ հադերձ `#`։ Օրինակ, `v-slot:header`-ը կարող է գրվել որպես `#header`։

```html
<base-layout>
  <template #header>
    <h1>Այստեղ կարող է լինել էջի վերնագիրը</h1>
  </template>

  <p>Պարբերություն հիմնական բովանդակության համար։</p>
  <p>Հավելյալ պարբերություն։</p>

  <template #footer>
    <p>Այստեղ կարող է լինել կոնտակտային ինֆորմացիա</p>
  </template>
</base-layout>
```

Սակայն, ինչպես այլ ուղղորդիչներ, կարճ գրելաձևը հասանելի է միայն երբ արգումենտը տրամադրված է։ Սա նշանակում է որ հետևյալ գրելաձևը սխալ է․

``` html
<!-- Սա կստեղծի նախազգուշացում -->
<current-user #="{ user }">
  {{ user.firstName }}
</current-user>
```

Փոխարենը, դուք միշտ պետք է նշեք անունը սլոտի եթե ցանկանում եք օգտագործել կարճ գրելաձև։

``` html
<current-user #default="{ user }">
  {{ user.firstName }}
</current-user>
```

## Այլ Օրինակներ

**Սլոտի prop-ները թույլ են տալիս մեզ դարձնել սլոտները դեպի վերօգտագործվող ձևանմուշների որոնք կարող են render անել տարբեր բովանդակություն կախված մուտքագրված prop-ներից։** Սա կարող է շատ օգտակար լինել երբ դուք ստեղծում եք վերօգտագործվող կոմպոնենտ որը կապսուլացնում է տվյալների տրամաբանությունը միաժամանակ թույլ տալով ծնող կոմպոնենտի սպառումը որպեսզի փոփոխի իր layout-ի մի մաս։

Օրինակ, մենք ունենք `<todo-list>` կոմպոնենտ որը պարունակում է layout և ֆիլտրացման տրամաբանություն ցանկի համար։

```html
<ul>
  <li
    v-for="todo in filteredTodos"
    v-bind:key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

Ի փոխարեն hard-coding անելու ամեն todo-ի բովանդակությունը, մենք կարող ենք թույլ տալ ծնող կոմպոնենտին վերցնել կառավարումը դարձնելով ամեն todo-ն սլոտ, այնուհետև կապելով `todo`-ն որպես սլոտի prop։

```html
<ul>
  <li
    v-for="todo in filteredTodos"
    v-bind:key="todo.id"
  >
    <!--
    Մենք ունենք սլոտ ամեն todo-ի համար, փոխանցելով դա
    `todo` օբյեկտին որպես սլոտ-ի prop:
    -->
    <slot name="todo" v-bind:todo="todo">
      <!-- Ընկման բովանդակություն -->
      {{ todo.text }}
    </slot>
  </li>
</ul>
```

Հիմա երբ մենք օգտագործենք `<todo-list>` կոմպոնենտ, մենք կարող ենք ըստ ցանկության հայտարարել ալտերնատիվ `<template>` todo items-ի համար, բայց տվյալների մուտք ապահովելով ժառանգողից։

```html
<todo-list v-bind:todos="todos">
  <template v-slot:todo="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>
```

Սակայն, սա շատ քիչ է ցույց տալիս թե ինչի են ընդհունակ scope-ված սլոտները։ Իրական կյանքում, ուժեղ օրինակները scope-ված սլոտների օգտագործման, մենք խորհուրդ ենք տալիս զննել գրադարանները ինչպիսին են [Vue Virtual Scroller](https://github.com/Akryum/vue-virtual-scroller), [Vue Promised](https://github.com/posva/vue-promised), և [Portal Vue](https://github.com/LinusBorg/portal-vue)։

## Հնացած Գրելաձև

> `v-slot` ուղղորդիչը ներկայացվել էր Vue 2.6.0-ում, տրամադրելով կատարելագործված, ալտերնատիվ API դեռ համապատասխանող `slot` և `slot-scope` ատրիբուտներին։ Ներդրման ամբողջական հիմնավորումը `v-slot`-ի համար նկարագրված է այս [RFC-ում](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0001-new-slot-syntax.md)։ `slot` և `slot-scope` ատրիբուտները կշարունակեն համապատասխանել բոլոր ապագայի 2.x թողարկումներում, բայց պաշտոնապես հնացած են և ի վերջո կհեռացվեն Vue 3-ում։

### Անվանված Սլոտները `slot` Ատրիբուտի Հետ

> <abbr title="Դեռ համապատասխանում է բոլոր 2.x տարբերակներում Vue-ի, բայց այլևս խորհուրդ չի տրվում օգտագործել։">Հնացված է</abbr> 2.6.0+-ի մեջ։ Նայեք [այստեղ](#Named-Slots) նոր, առաջարկված գրելաձևը։

Որպեսզի փոխանցել բովանդակություն անվանված սլոտներին ծնողից, օգտագործեք այս հատուկ `slot` ատրիբուտը `<template>`-ի վրա (օգտագործելով `<base-layout>` կոմպոնենտը նկարագրված [այստեղ](#Named-Slots) որպես օրինակ։)

```html
<base-layout>
  <template slot="header">
    <h1>Այստեղ կարող է լինել էջի վերնագիրը</h1>
  </template>

  <p>Պարբերություն հիմնական բովանդակության համար։</p>
  <p>Հավելյալ պարբերություն։</p>

  <template slot="footer">
    <p>Այստեղ կարող է լինել կոնտակտային ինֆորմացիա</p>
  </template>
</base-layout>
```

Or, the `slot` attribute can also be used directly on a normal element:

``` html
<base-layout>
  <h1 slot="header">Այստեղ կարող է լինել էջի վերնագիրը</h1>

  <p>Պարբերություն հիմնական բովանդակության համար։</p>
  <p>Հավելյալ պարբերություն։</p>

  <p slot="footer">Այստեղ կարող է լինել կոնտակտային ինֆորմացիա</p>
</base-layout>
```

Այստեղ դեռ կարող է լինել մեկ չանվանված սլոտ, որը **հիմնական սլոտն** է, որը ծառայում է որպես ընկում չհամապատասխանող բովանդակությունների համար։ Երկու օրինակներում նշված վերևում, render եղած HTML-ը կլինի․

``` html
<div class="container">
  <header>
    <h1>Այստեղ կարող է լինել էջի վերնագիրը</h1>
  </header>
  <main>
    <p>Պարբերություն հիմնական բովանդակության համար։</p>
    <p>Հավելյալ պարբերություն։</p>
  </main>
  <footer>
    <p>Այստեղ կարող է լինել կոնտակտային ինֆորմացիա</p>
  </footer>
</div>
```

### Scope-ված Սլոտները `slot-scope` Ատրիբուտի Հետ

> <abbr title="Դեռ համապատասխանում է բոլոր 2.x տարբերակներում Vue-ի, բայց այլևս խորհուրդ չի տրվում օգտագործել։">Հնացած է</abbr> 2.6.0+-ի մեջ։ Նայեք [այստեղ](#Scoped-Slots) նոր, առաջարկված գրելաձևը։

Որպեսզի ստանալ prop-ները փոխանցված սլոտին, ծնող կոմպոնենտը կարող է օգտագործել `<template>` `slot-scope` ատրիբուտի հետ (օգտագործելով `<slot-example>`-ը նկարագրված [այստեղ](#Scoped-Slots) որպես օրինակ)․

``` html
<slot-example>
  <template slot="default" slot-scope="slotProps">
    {{ slotProps.msg }}
  </template>
</slot-example>
```

Այսեղ, `slot-scope`-ը հայտարարում է ստացած prop-ների օբյեկտը որպես `slotProps` փոփոխական, և դարձնում է այն հասանելի `<template>` scope-ի մեջ։ Դուք կարող եք անվանել `slotProps` ինչ ցանկանաք, JavaScript ֆունկցիայի արգումենտներին նման:

Այստեղ `slot="default"`-ը կարելի է բաց թողնել այնպես, ինչպես ենթադրվում է․

``` html
<slot-example>
  <template slot-scope="slotProps">
    {{ slotProps.msg }}
  </template>
</slot-example>
```

`slot-scope` ատրիբուտը կարող է օգտագործվել ուղիղ ոչ `<template>` էլեմենտի վրա (կոմպոնենտները ներառյալ)․

``` html
<slot-example>
  <span slot-scope="slotProps">
    {{ slotProps.msg }}
  </span>
</slot-example>
```

Սա նշանակում է `slot-scope`-ի արժեքը իրականում կարող է ընդհունել ցանկցած վալիդ JavaScript արտահայտություն որը կարող է հայտնվել ֆունկցիայի հայտարարման արգումենտում։ Այնպես որ համապատասխանող enviroment-ներում ([մեկ ֆայլ կոմպոնենտներում](single-file-components.html) կամ [ժամանակակից բրաուզերներում](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Browser_compatibility)), դուք նաև կարող եք օգտագործել [ES2015 ապակառուցում](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Object_destructuring) որպեսզի դուրս հանեք կոնկրետ սլոտի prop-ներ, ինչպես այստեղ․

``` html
<slot-example>
  <span slot-scope="{ msg }">
    {{ msg }}
  </span>
</slot-example>
```

Օգտագործելով `<todo-list>` նկարագրված [այստեղ](#Other-Examples) որպես օրինակ, ահա նման օգտագործման օրինակ օգտագործելով `slot-scope`-ը․

``` html
<todo-list v-bind:todos="todos">
  <template slot="todo" slot-scope="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>
```
