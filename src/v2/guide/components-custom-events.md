---
title: Custom Event-ներ
type: ուղեցույց
order: 103
---

> Այս էջը ենթադրում է որ դուք արդեն կարդացելեք [Կոմպոնենտների Հիմունքները](components.html)։ Կարդացեք այն եթե դուք նոր եք ծանոթանում կոմպոնենտներին։

<div class="vueschool"><a href="https://vueschool.io/lessons/communication-between-components?friend=vuejs" target="_blank" rel="sponsored noopener" title="Սովորեք թե ինչպես աշխատել custom event-ների հետ Vue School-ում">Սովորեք թե ինչպես աշխատել custom event-ների հետ անվճար Vue School-ի դասում</a></div>

## Event-ի Անուններ

Ի տարբերություն կոմպոնենտների և հատկությունների, event-ի անունները չեն տրամադրում ավտոմատ case-ի վերափոխում։ Փոխարենը, անունը արձակված event-ի պետք է համապատասխանի այն անունին որով պետք է լսել այդ event-ին։ Օրինակ՝ եթե արձակեք camelCased event-ի անուն։

```js
this.$emit('myEvent')
```

Լսելով kebab-cased տարբերակին ոչ մի էֆեկտ չի ունենա։

```html
<!-- Չի աշխատի -->
<my-component v-on:my-event="doSomething"></my-component>
```

Ի տարբերություն կոմպոնենտների և հատկությունների, event-ի անունները երբեք չեն օգտագործվի որպես փոփոխական կամ հատկության անուն JavaScript-ի մեջ, որի հետևանքով պետք չէ օգտագործել camelCase կամ PascalCase: Ի հավելումն, `v-on` event-ին լսողները DOM-ի ձևանմուշների ներսում ավտոմատ կերպով կվերափոխվեն փոքրատառ (HTML-ի case-insensitivity-ի պատճառով), այս դեպքում `v-on:myEvent` կդառնա `v-on:myevent` -- `myEvent`-ը լինելով անհնարին լսելու համար։

Այս պատճառներով, մեք խորհուրդ ենք տալիս ձեզ **միշտ օգտագործել kebab-case event-ի անունների համար**։

## Կոմպոնենտի `v-model`-ի Ձևափոխումները

> Նոր 2.2.0+-ի մեջ

Հիմնականում, `v-model`-ը կոմպոնենտի վրա օգտագործում է `value` որպես հատկություն և `input` որպես event, բայց որոշ input-ի տիպեր ինչպիսիք են checkbox-երը և radio button-ները կարող են օգտագործել `value` ատրիբուտը [ուրիշ նպատակներով](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox#Value)։ Օգտագործելով `model` ընտրանքը կարող է խուսփել կոնֆլիկտ այս դեպքերում։

```js
Vue.component('base-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
})
```
Հիմա երբ օգտագործում ենք `v-model` այս կոմպոնենտի վրա։

```html
<base-checkbox v-model="lovingVue"></base-checkbox>
```

`lovingVue`-ի արժեքը կփոխանցվի `checked` հատկությանը։ `lovingVue` հատկությունը պետք է թարմեցվի երբ `<base-checkbox>`-ը արձակի `change` event նոր արժեքի հետ հանդերձ։

<p class="tip">Նշում․ դուք միայն պետք է հայտարարեք <code>checked</code> հատկությունը կոմպոնենտի <code>props</code> ընտրանքում։</p>

## Native Event-ների Կապումը Կոմպոնենտներին

Շատ դեպքեր կլինեն երբ որ դուք պետք է ուղիղ լսեք native event-ին կոմպոնետնի արմատային էլեմենտի վրա։ Այս դեպքերում, դուք պետք է օգտագործեք `.native` փոփոխիչը `v-on`-ի համար։

```html
<base-input v-on:focus.native="onFocus"></base-input>
```

Սա կարող է լինել օգտակար երբեմն, բայց այն լավ գաղափար չէ երբ որ դուք փորձում էք լսել շատ կոնկրետ էլեմենտի, `<input>`-ին նման։ Օրինակ՝ `<base-input>` կոմպոնենտը վերևում նշված կարող է ռեֆակտոր լինել որ արմատային էլեմենտը իրականում `<label>` էլեմենտ է։

```html
<label>
  {{ label }}
  <input
    v-bind="$attrs"
    v-bind:value="value"
    v-on:input="$emit('input', $event.target.value)"
  >
</label>
```

Այդ դեպքում, `.native` լսողը ծնողի մեջ ուղակի լուռ կերպով կկոտրվի։ Սխալներ չեն գրանցվի, բայց `onFocus` handler-ը չի արձակվի երբ որ մենք սպասում ենք դրան։

Որպեսզի լուծենք այս հարցը, Vue—ն տրամադրում է `$listeners` հատկություն պարունակող լսողների օբյեկտ որը օգտագործվում է կոմպոնենտում։ Օրինակ՝

```js
{
  focus: function (event) { /* ... */ }
  input: function (value) { /* ... */ },
}
```

Օգտագործելով `$listener` հատկությունը, դուք կարող էք ուղարկել բոլոր event-ի լսողներին այդ կոմպոնենտում կոնկրետ ժառանքող էլեմենտին `v-on"$listeners"`-ի շնորհիվ։ Օրինակ՝ էլեմենտները ինչպիսիք են `<input>`, դուք հնարավոր է ուզենաք աշխատացել `v-model`-ի հետ, դա հազվադեպ օգտագործվում է որպեսզի ստեղծել նոր հաշվարկված հատկություն լսողների համար, ինչպես `inputListeners` է նշված ներքևում։

```js
Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  computed: {
    inputListeners: function () {
      var vm = this
      // `Object.assign`-ի ձուլում է օբյեկտները միասին որպեսզի կազմվի նոր օբյեկտ
      return Object.assign({},
        // Մենք ավելացնում ենք բոլոր լսողներին ծնողից
        this.$listeners,
        // Հետո մենք կարող ենք ավելացնել custom լսողներ կամ ovveride անել 
        // behavior-ը որոշ լսողների։
        {
          // Սա համոզվում է որ կոմպոնենտը աշխատում է v-model—ի հետ
          input: function (event) {
            vm.$emit('input', event.target.value)
          }
        }
      )
    }
  },
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on="inputListeners"
      >
    </label>
  `
})
```

Հիմա `<base-input>` կոմպոնենտը **ամբողջովին թափանցիկ wrapper** է, որը նշանակում է որ այն կարող է օգտագործվել նույն ձև ինչպես հասարակ `<input>` էլեմենտն է։ Բոլոր նույն ատրիբուտները և լսողները կաշխատեն, առանց `.native` փոփոխիչի։

## `.sync` Փոփոխիչ

> Նոր 2.3.0+-ի մեջ

Որոշ դեպքերում, մենք հնարավոր է "երկուղի կապման" կարիքը ունենանք հատկության համար։ Ցավոք սրտի, իրական երկուղի կապումը կարող է ստեղծել maintance-ի հարցեր, որովհետև ժառանգող կոմպոնենտները կարող են մուտացնել ծնողին առանց source-ի այդ մուտացիայի լինելով ակնհայտ ծնողում և ժառանգողում։

Այդ պաճառով դրա փոխարեն, մենք խորհուրդ ենք տալիս արձակել event-ներ նման կերպով `update:myPropName`։ Օրինակ՝ հիպոթետիկ կոմպոնենտի մեջ `title` հատկությունով, մենք կարող էինք հաղորդակցվել նոր արժեք նշանակելու մտադրության հետ։

```js
this.$emit('update:title', newTitle)
```

Հետո ծնողը կարող է լսել այդ event-ին և թարմացնել լոկալ տվյալների հատկությունը, եթե անհրաժեշտ է։ Օրինակ՝

```html
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>
```

Հարմարության համար, մենք առաջարկում ենք կարճ գրելաձև այս ձևի համար օգտագործելով `.sync` փոփոխիչը։

```html
<text-document v-bind:title.sync="doc.title"></text-document>
```

<p class="tip">Նշում որ <code>v-bind-ը</code> ի հանդերձ <code>.sync</code> փոփոխիչի հետ <strong>չի</strong> աշխպատում արտահայտությունների հետ (օրինակ՝ <code>v-bind:title.sync="doc.title + '!'"</code> սխալ է)։ Փոխարենը, դուք միայն պետք է տրամադրեք այն հատկության անունը որը դուք ցանկանում էք կապել, որը նման է <code>v-model-ին</code>։</p>

`.sync` փոփոխիչը կարող է օգտագործվել `v-bind`-ի հետ երբ օգտագործում էք օբյեկտ որպեսզի նշանակեք բազմաթիվ հատկություններ։

```html
<text-document v-bind.sync="doc"></text-document>
```

Սա փոխանցում է ամեն հատկանիշը `doc` օբյեկտին (օրինակ `title`) որպես անհատ հատկանիշ, այնուհետև ավելացնում է `v-on` թարմեցման լսողները ամեն մեկի համար։

<p class="tip">Օգտագործելով <code>v-bind.sync-ը</code> բառացի օբյեկտի հետ, ինչպիսին է <code>v-bind.sync="{ title: doc.title }"</code>, այն չի աշխատի, որովհետև այսպիսի բարդ արտահայտություն վերլուծելիս չափազանց շատ եզրեր կան։</p>
