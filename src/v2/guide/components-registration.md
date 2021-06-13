---
title: Կոմպոնենտի Գրանցումը
type: ուղեցույց
order: 101
---

> Այս էջը ենթադրում է որ դուք արդեն կարդացել եք [Կոմպոնենտների Հիմունքները](components.html)։ Կարդացեք այն եթե դուք նոր եք ծանոթանում կոմպոնենտներին։

<div class="vueschool"><a href="https://vueschool.io/lessons/global-vs-local-components?friend=vuejs" target="_blank" rel="sponsored noopener" title="Անվճար Vue.js-ի Կոմպոնենտի Գրանցման Դասընթաց">Դիտեք անվճար դաս Vue School-ում</a></div>

## Կոմպոնենտի Անունները

Երբ գրանցում ենք կոմպոնենտ, նրան միշտ կտրվի անուն։ Օրինակ, գլոբալ գրացման մեջ մենք տեսել ենք։

```js
Vue.component('my-component-name', { /* ... */ })
```

Կոմպոնենտի անունը առաջին արգումենտն է `Vue.component`-ում։

Անունը որը դուք տվել եք կոմպոնենտին կարող է կախված լինել թե որտեղ եք պատրաստվում այն օգտագործել։ Երբ օգտագործում եք կոմպոնենտը ուղիղ DOM-ի մեջ (որը հակասված է string ձևանմուշին կամ [մեկ ֆայլ կոմպոնենտին](single-file-components.html)), մենք խորհուրդ ենք տալիս հետևել [W3C օրենքներին](https://html.spec.whatwg.org/multipage/custom-elements.html#valid-custom-element-name) custom tag-երին անունների համար (բոլորը փոքրատառ, պետք է պարունակի միության գծիկ)։ Սա օգնում է ձեզ խուսափել կոնֆլիկտներից ընթացիկ և ապագա HTML տարրերի մեջ։

Դուք կարող եք նաև նայել առաջարկությունները կոմպոնենտի անունների համար [Ոճի Ուղեցույցում](../style-guide/#Base-component-names-strongly-recommended)։

### Անվան Casing-ը

Դուք ունեք երկու տարբերակ երբ նկարագրում եք կոմպոնենտի անունները։

#### Kebab-case-ով

```js
Vue.component('my-component-name', { /* ... */ })
```

Երբ նկարագրում եք կոմպոնենտը kebab-case-ով, դուք պետք է օգտագործեք kebab-case երբ դիմում եք այդ custom էլեմենտին, ինպես արված է `<my-component-name>`։

#### PascalCase-ով

```js
Vue.component('MyComponentName', { /* ... */ })
```

Երբ նկարագրում եք կոմպոնենտը PascalCase-ով, դուք կարող եք օգտագործել երկուսնել երբ դիմում եք custom էլեմենտին։ Սա նշանակում է որ `<my-component-name>` և `<MyComponentName>`-ը ընդունելի են։ Նշում, սակայն, միայն kebab-case-ով գրված անուններնեն ճիշտ ուղիղ DOM-ի մեջ (օրինակ՝ ոչ string ձևանմուշները)։

## Գլոբալ Գրանցում

Մինչ դեռ, մենք միայն ստեղծել ենք կոմպոնենտներ օգտագործելով `Vue.component`-ը․

```js
Vue.component('my-component-name', {
  // ... ընտրանքները ...
})
```

Այս կոմպոնենտները **գլոբալ գրանցված** են։ Սա նշանակում է որ նրանք կարող են օգտագործվել արմատային Vue instance-ի (`new Vue`) ցանկացած ձևանմուշում որը ստեղծվում է գրանցումից հետո։ Օրինակ՝

```js
Vue.component('component-a', { /* ... */ })
Vue.component('component-b', { /* ... */ })
Vue.component('component-c', { /* ... */ })

new Vue({ el: '#app' })
```

```html
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
  <component-c></component-c>
</div>
```

Սա նաև կիրառվում է բոլոր սուբկոմպոնենտներին, որը նշանակում է որ բոլոր երեքնել այս կոմպոնենտներից հասանելի կլինեն _մեկը մյուսի մեջ_։

## Լոկալ Գրանցում

Գլոբալ գրանցումը հաճախ իդեալական չէ։ Օրինակ, եթե դուք օգտագործում եք կառուցման համակարգեր ինչպիսին է Webpack—ը, գլոբալ գրանցելով բոլոր կոմպոնենտները նշանակում է եթե դուք չօգտագործեք որևէ կոմպոնենտ, այն դեռ կներառվի ձեր վերջին build-ում։ Սա անհարկի մեծացնում է չափսը JavaScript-ի որը ձեր օգտագործողները պետք է ներբեռնեն։

Այս դեպքերում, դուք կարող եք նկարագրել ձեր կոմպոնենտը որպես չոր JavaScript-ի օբյեկտներ։

```js
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```

Հետո հայտարարեք այն կոմպոնենտները որոնք դուք ցանկանում եք օգտագործել `components` ընտրանքի մեջ․

```js
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

Ամեն հատկանիշի համար որը գտնվում է `components` օբյեկտում, key-ն կլինի նույն անունը custom էլեմենտի, և արժեքը կպարունակի ընտրանքների օբյեկտը կոմպոնենտի համար։

Նշում որ **լոկալ գրանցված կոմպոնենտները հասանելի _չեն_ լինի սուբկոմպոնենտներում**։ Օրինակ, եթե դուք ցանկանում եք որպեսզի `ComponentA`-ն լինի հասանելի `ComponentB`-ում, դուք պետք է օգտագործեք․

```js
var ComponentA = { /* ... */ }

var ComponentB = {
  components: {
    'component-a': ComponentA
  },
  // ...
}
```

Կամ եթե դուք օգտագործում եք ES2015 մոդուլներ, ինչպիսիք են Babel-ը և Webpack-ը, այն ավելի նման կլինի հետևյալին։

```js
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  },
  // ...
}
```

Նշում որ ES2015+, տեղադրելով փոփոխականի անուն ինչպիսին `ComponentA`-ն է օբյեկտի ներսում կարճ կլինի `ComponentA: ComponentA`, որը նշանակում է որ փոփոխականի անունը․

- custom էլեմենտի անունն է որպեսզի օգտագործվի ձևանմուշում, և
- փոփոխականի անունն է որը պարունակում է կոմպոնենտնի ընտրանքները

## Մոդուլի Համակարգեր

Եթե դուք չեք օգտագործում մոդուլի համակարգեր `import`/`require`-ի հետ հանդերձ, դուք կարող եք բաց թողնել այս բաջինը հիմա։ Եթե դուք օգտագործում եք, մենք ունենք հատուկ ցուցումներ և խորհուրդներ հենց ձեզ համար։

### Լոկալ Գրանցում Մոդուլի Համակարգում

Եթե դուք դեռևս այստեղ եք, ուրեմն հավանական է որ դուք օգտագործում եք մոդուլի համակարգ, ինչպիսին են Babel և Webpack-ը համատեղ։ Այս դեպքերում, մենք խորհուրդ ենք տալիս ստեղծել `component` պանակ, որը կպարունակի ամեն կոմպոնենտը իր ֆայլի մեջ։

Եթե ձեզ պետք է import անել ամեն կոմպոնենտը որը դուք ցանկանում եք օգտագործել, նախքան լոկալ գրանցելը դրանք։ Օրինակ, կարող եք պահել որպես հիպոթետիկ `ComponentB.js` կամ `ComponentB.vue` ֆայլ։

```js
import ComponentA from './ComponentA'
import ComponentC from './ComponentC'

export default {
  components: {
    ComponentA,
    ComponentC
  },
  // ...
}
```

Հիմա `ComponentA` և `ComponentC` կարող են օգտագործվել `ComponentB`-ի ձևանմուշում։

### Ավտոմատ Գլոբալ Գրանցում Հիմնական Կոմպոնենտների Համար

Շատերը ձեր կոմպոնենտներից հարաբերականորեն ընդհանուր են, հնարավորություն ունենալով փաթաթվելու էլեմենտում ինչպիսին է input-ը կամ button-ը։ Մենք երբեմն դիմում ենք նրանց որպես [հիմնական կոմպոնենտներ](../style-guide/#Base-component-names-strongly-recommended) և նրանք նախատեսված են որպեսզի հաճախ օգտագործվեն ձեր կոմպոնենտների մեջ։

Արդյունքում շատ կոմպոնենտները կարող ենք ընդգրկել երկար ցուցած հիմնական կոմպոնենտներում․

```js
import BaseButton from './BaseButton.vue'
import BaseIcon from './BaseIcon.vue'
import BaseInput from './BaseInput.vue'

export default {
  components: {
    BaseButton,
    BaseIcon,
    BaseInput
  }
}
```

Պարզապես աջակցելու համար, որ համեմատաբար քիչ նշում կատարվի ձևանմուշում․

```html
<BaseInput
  v-model="searchText"
  @keydown.enter="search"
/>
<BaseButton @click="search">
  <BaseIcon name="search"/>
</BaseButton>
```

Բարեբախտորեն, եթե դուք օգտագործում եք Webpack (կամ [Vue CLI 3+](https://github.com/vuejs/vue-cli), որը ներքուստ օգտագործում է Webpack), դուք կարող եք օգտագործել `require.context` որպեսզի գլոբալ գրանցեք միայն շատ հաճախ օգտագործվող հիմնական կոմպոնենտները։ Ահա մի կոդի օրինակ որը դուք կարող եք օգտագործել գլոբալ կերպով հիմնական կոմպոնենտները import անելու ձեր ծրագրի մուտքային ֆայլի մեջ (օրինակ՝ `src/main.js`-ում)։

```js
import Vue from 'vue'
import upperFirst from 'lodash/upperFirst'
import camelCase from 'lodash/camelCase'

const requireComponent = require.context(
  // Հարաբերական կոմպոնենտների պանակի ճանապարհը
  './components',
  // Նայել նաև ենթապանականերում թե ոչ օգտագործելով boolean
  false,
  // Այս regular expression-ը օգտագործված է որ համապատասխանի հիմանական կոմպոնենտների ֆայլերի անուններին
  /Base[A-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // Ստանալ կոմպոնենտնի կոնֆիգուրացիան
  const componentConfig = requireComponent(fileName)

  // Ստանալ PascalCase անունը կոմպոնենտնի
  const componentName = upperFirst(
    camelCase(
      // Ստանում է ֆայլի անունը անկախ պանակի խորությունից
      fileName
        .split('/')
        .pop()
        .replace(/\.\w+$/, '')
    )
  )


  // Գրանցել կոմպոնենտ գլոբլալ կերպով
  Vue.component(
    componentName,
    // Նայեք կոմպոնենտի ընտրանքները `.default`-ում, որը
    // գոյություն կունենա եթե կոմպոնենտը export է արված `export default`-ով,
    // հակառակ դեպքում հետ կընկնի դեպի մոդուլի արմատ։
    componentConfig.default || componentConfig
  )
})
```

Հիշեք որ **գլոբալ գրանցումը պետք է կատարվի նախքան արմատային Vue instance-ի ստեղծելը (`new Vue`-ի հետ)**։ [Այստեղ է օրինակը](https://github.com/chrisvfritz/vue-enterprise-boilerplate/blob/master/src/components/_globals.js) այս pattern-ի իրական նախագծի կոնտեքստում։
