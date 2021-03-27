---
title: Պայմանական Rendering
type: ուղեցույց
order: 7
---

<div class="vueschool"><a href="https://vueschool.io/lessons/vuejs-conditionals?friend=vuejs" target="_blank" rel="sponsored noopener" title="Սովորեք թե ինչպես է պայմանական rendering-ը աշխատում Vue School-ի հետ">Սովորեք անվճար Vue School-ի դասում թե ինչպես է պայմանական rendering-ը աշխատում</a></div>

## `v-if`

`v-if` ուղղորդիչը օգտագործվում է որպեսզի պայմանականորեն render-անի մի բլոկ։ Բլոկը միայն render կլինի այն ժամանակ երբ ուղղորդիչի արտահայտությունը վերադարձնում է true արժեք։

``` html
<h1 v-if="awesome">Vue-ն հիանալի է!</h1>
```

Նաև հնարավոր է ավելացնել «else բլոկ» օգտագործելով `v-else`-ի հետ։

``` html
<h1 v-if="awesome">Vue-ն հիանալի է!</h1>
<h1 v-else>Օ ոչ 😢</h1>
```

### Պայմանական Խմբեր `v-if`-ի Հետ `<template>`-ում

Քանի որ `v-if`-ը ուղղորդիչ է, այն պետք է միացված լինի մեկ էլեմենտի։ Բայց ի՞նչ կլինի եթե մենք ցանկանում ենք միացնել մի քանի էլեմենտներ, այս դեպքում մենք կարող ենք օգտագործել `v-if`-ը `<template>` էլեմենտի վրա, որը ծառայում է որպես անտեսանելի wrapper։ Վերջին render եղած արդյունքում դուք չեք ներառի `<template>` էլեմենտը։

``` html
<template v-if="ok">
  <h1>Վերնագիր</h1>
  <p>Պարբերություն 1</p>
  <p>Պարբերություն 2</p>
</template>
```

### `v-else`

Դուք կարող եք օգտագործել `v-if` ուղղորդիչը որպեսզի նշեք «else բլոկ» `v-if`-ի համար․

``` html
<div v-if="Math.random() > 0.5">
  Հիմա դուք տեսնում եք ինձ
</div>
<div v-else>
  Հիմա ոչ
</div>
```

`v-else` Էլեմենտը միանգամից պետք է լինի `v-if`-ով կամ `v-else-if`-ով էլեմենտից հետո - հակառակ դեպքում այն չի ճանաչվի։

### `v-else-if`

> Նոր 2.1.0+-ի մեջ

`v-else-if`-ը, ինչպես անունը առաջարկում է, ծառայում է որպես «else if բլոկ» `v-if`-ի համար։ Այն նաև կարող է շղթայված լինել բազմաթիվ անգամ․

```html
<div v-if="type === 'A'">
  Ա
</div>
<div v-else-if="type === 'B'">
  Բ
</div>
<div v-else-if="type === 'C'">
  Գ
</div>
<div v-else>
  Ոչ Ա/Բ/Գ
</div>
```

Նման լինելով `v-else`-ին, `v-else-if` էլեմենտը պետք է անմիջապես հետևի `v-if`-ով կամ `v-else-if`-ով էլեմենտի։

### Վերօգտագործվող Էլեմենտների Կառավարումը `key`-ով

Vue-ն փորձում է render անել էլեմենտները որքան հնարավոր է արդյունավետ, հաճախ վերօգտագործելով նրանց ի փոխարեն կրկին render անելուց։ Սա ոչ միայն օգնում է Vue-ն դարձնել շատ արագ, նաև ունի մի քանի առավելություն։ Օրինակի համար, եթե դուք թույլ տաք օգտագործողներին որպեսզի փոփոխեն լոգինների տիպերը․

``` html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Մուտքագրեք ձեր օգտանունը">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

Այնուհետև փոփոխելով `loginType`-ը վերևում նշված կոդում չի ջնջի թե ինչ է օգտատերը գրել։ Քանի որ ձևանմուշները օգտագործում են նույն էլեմենտները, `<input>`-ը չի փոխարինվի - այլ միայն իր `placholder`-ը։

Փորձեք մուտքագրել որևէ տեքստ դաշտի մեջ, այուհետև սեղմեք փոխել․

{% raw %}
<div id="no-key-example" class="demo">
  <div>
    <template v-if="loginType === 'username'">
      <label>Օգտանուն</label>
      <input placeholder="Մուտքագրեք ձեր օգտանունը">
    </template>
    <template v-else>
      <label>Էլ․Փոստ</label>
      <input placeholder="Մուտքագրեք ձեր օգտանուն էլ․փոստը">
    </template>
  </div>
  <button @click="toggleLoginType">Փոխել Լոգինի Տիպը</button>
</div>
<script>
new Vue({
  el: '#no-key-example',
  data: {
    loginType: 'username'
  },
  methods: {
    toggleLoginType: function () {
      return this.loginType = this.loginType === 'username' ? 'email' : 'username'
    }
  }
})
</script>
{% endraw %}

Ամեն դեպքում սա միշտ ցանկալի չէ, այնպես որ Vue-ն առաջարկում է ձեզ ճանապարհ ասելու, «Այս երկու էլեմենտները լիովին առանձին են - չվերօգտագործես դրանք»։ Ավելացնելով `key` ատրիբուտ հատուկ արժեքներով․

``` html
<template v-if="loginType === 'username'">
  <label>Օգտանուն</label>
  <input placeholder="Մուտքագրեք ձեր օգտանունը" key="username-input">
</template>
<template v-else>
  <label>Էլ․Փոստ</label>
  <input placeholder="Մուտքագրեք ձեր օգտանուն էլ․փոստը" key="email-input">
</template>
```

Հիմա այդ դաշտերը render կլինեն զրոյից ամեն անգամ փոխելու ժամանակ։ Տեսեք ինքներտ․

{% raw %}
<div id="key-example" class="demo">
  <div>
    <template v-if="loginType === 'username'">
      <label>Username</label>
      <input placeholder="Մուտքագրեք ձեր օգտանունը" key="username-input">
    </template>
    <template v-else>
      <label>Email</label>
      <input placeholder="Մուտքագրեք ձեր օգտանուն էլ․փոստը" key="email-input">
    </template>
  </div>
  <button @click="toggleLoginType">Փոխել Լոգինի Տիպը</button>
</div>
<script>
new Vue({
  el: '#key-example',
  data: {
    loginType: 'username'
  },
  methods: {
    toggleLoginType: function () {
      return this.loginType = this.loginType === 'username' ? 'email' : 'username'
    }
  }
})
</script>
{% endraw %}

Նշում որ `<label>` էլեմենտները արդյունավետորեն վերօգտագործվել են, որովհետև նրանք չունեն `key` ատրիբուտներ։

## `v-show`

Մեկ այլ ճանապարհ պայմանականորեն ցույց տալու էլեմենտը դա `v-show` ուղղորդիչն է։ Օգտագործումը հիմնականում նույնն է․

``` html
<h1 v-show="ok">Բարև՜</h1>
```

Տարբերությունը որ էլեմենտը `v-show`-ի հետ միշտ render կլինի և կմնա DOM-ում; `v-show`-ը միայն փոփոխում է `display` CSS հատկությունը էլեմենտի։

<p class="tip">Նշում որ `v-show`-ը չի համապատասխանում `<template>` էլեմենտի հետ, և չի աշխատում `v-else`-ի հետ։</p>

## `v-if` ընդեմ `v-show`

`v-if`-ը «իրական» պայմանական rendering է որովհետև այն համոզվում է որ event listener-ները և ժառանգող կոմպոնենտները պայմանական բլոկներում համապատասխանորեն ոչնչացված և վերստեղծված են փոփոխությունների ժամանակ։

`v-if`-ը նաև **ծույլ է**․ եթե պայմանը false-է սկզբնական render-ում, այն ոչինչ չի անի - պայմանական բլոկը render չի լինի մինչև պայմանը կդառնա true առաջին անգամ։

համեմատելով, `v-show`-ը ավելի պարզ է - էլեմենտը միշտ render կլինի անկախ սկզբնական պայմանից, CSS-ով հիմք ունեցող փոփոխությամբ։

Հիմնականում, `v-if`-ունի ավելի շատ փոփոխությունների ծախս իսկ `v-show`-ը ունի ավելի շատ սկզբնական render—ի ծախս։ Այնպես որ նախընտրեք `v-show`-ը եթե դուք պետք է փոփոխեք ինչոր մի բան շատ հաճախ, և `v-if`-ը եթե պայմանը անհավանական է որ կփոփոխվի runtime-ի ժամանակ։

## `v-if`-ը `v-for`-ի հետ

<p class="tip">Օգտագործելով `v-if` և `v-for` միասին **խորհուրդ չի տրվում**։ Նայեք [ոճի ուղեցույցը](/v2/style-guide/#Avoid-v-if-with-v-for-essential) հավելյալ ինֆորմացիայի համար։</p>

Երբ օգտագործված է `v-if`-ի հետ, `v-for`-ը ունի ավելի ավելի բարձր գերակայություն քան `v-if`-ը։ Նայեք <a href="../guide/list.html#v-for-with-v-if">ցուցակի rendering-ի ուղեցույցը</a> ավելի մանրամասների համար։
