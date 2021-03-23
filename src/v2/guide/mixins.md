---
title: Mixin֊ներ
type: ուղեցույց
order: 301
---

## Հիմքեր

Mixin֊ները տրամադրում են ճկուն համակարգ որպեսզի բաշխել վերօգտագործելի ֆունկցիոնալությունները Vue կոմպոնենտների համար։ Mixin օբյեկտը կարող է պարունակել ցանկացած կոմպոնենտի ընտրանք։ Երբ կոմպոնենտը օգտագործում է mixin, բոլոր ընտրանքները mixin֊ում կխառնվեն կոմպոնենտի ընտրանքների հետ։

<div class="vue-mastery"><a href="https://www.vuemastery.com/courses/next-level-vue/mixins" target="_blank" rel="noopener" title="Mixins Tutorial">Դիտեք այս բացատրող տեսանյութը Vue Mastery֊ի ում</a></div>

Օրինակ․

``` js
// հայտարարեք mixin օբյեկտ
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('բարև mixin֊ից!')
    }
  }
}

// հայտարարեք կոմպոնենտ որը օգտագործում է այդ mixin֊ը
var Component = Vue.extend({
  mixins: [myMixin]
})

var component = new Component() // => "բարև mixin֊ից"
```

## Ընտրանքների Ձուլում

Երբ mixin և կոմպոնենտը պարունակում են վերահայտարարվող ընտրանքներ, նրանք «կձուլվեն» օգտագործելով համապատասխան ստրատեգիաները։

Օրինակի համար, տվյալների օբյեկտները անցնում են ռեկուրսիվ ձուլման միջով, և կոմպոնենտի տվյալները ստանում են ավելի բարձր կարևորություն կոնֆլիկտների դեպքում։

``` js
var mixin = {
  data: function () {
    return {
      message: 'բարև',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'հաջող',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "հաջող", foo: "abc", bar: "def" }
  }
})
```

Hook ֆունկցիաները նույն անունով ձուլվում են դեպի զանգված այնպես որ նրանք բոլորը կկանչվեն։ Mixin hook-երը կկանչվեն **նախքան** կոմպոնենտի hook֊երը։

``` js
var mixin = {
  created: function () {
    console.log('mixin֊ի hook֊ը կանչվել է')
  }
}

new Vue({
  mixins: [mixin],
  created: function () {
    console.log('կոմպոնենտի hook֊ը կանչվել է')
  }
})

// => «mixin֊ի hook֊ը կանչվել է
// => «կոմպոնենտի hook֊ը կանչվել է»
```

Ընտրանքները որոնք սպասում են օբյեկտի արժեքներ, օրինակի համար `methods`, `components` և `directives`, կձուլվեն դեպի նույն օբյեկտ։ Կոմպոնենտի ընտրանքները կստանան կարևորություն երբ կոնֆիկտ ունեցող բանալիներ կան օբյեկտներում․

``` js
var mixin = {
  methods: {
    foo: function () {
      console.log('foo')
    },
    conflicting: function () {
      console.log('mixin֊ից')
    }
  }
}

var vm = new Vue({
  mixins: [mixin],
  methods: {
    bar: function () {
      console.log('bar')
    },
    conflicting: function () {
      console.log('mixin֊ից դուրս')
    }
  }
})

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => «mixin-ից դուրս»
```

Նշում որ նույն ձուլման ստրատեգիաները օգտագործվում են `Vue.extend()`֊ում։

## Գլոբալ Mixin

Դուք նաև կարող եք կիրառել mixin գլոբալ կերպով։ Օգտագործեք այն զգուշուցյամբ՛ այն կազդի **ամեն** Vue instance֊ի վրա որոնք ստեղծվել են հետո։ Երբ ճիշտ է օգտագործվում, այն կարող է օգտագործվել որպեսզի ներարկել գործընթացի տրամաբանություն custom ընտրանքների համար․

``` js
// ներարկեք handler `myOption` custom ընտրանքի համար
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'բարև՛'
})
// => "բարև՛"
```

<p class="tip">Օգտագործեք գլոբալ mixin֊ները քիչ և զգուշությամբ, որովհետև այն ազդում է ամեն ստեղծված Vue instance֊ի վրա, ներառյալ երրորդ կողմի կոմպոնենտների։ Շատ դեպքերում, դուք միայն պետք է օգտագործեք այն custom ընտրանքի handling֊ի համար ներկայացված վերևի օրինակի մեջ։ Նաև լավ միտք է դարձնել նրանց [Plugin](plugins.html) որպեսզի խուսափել կրկնօրինակի կիրառումից։</p>

## Custom Ընտրանքի Ձուլման Ստրատեգիաներ

Երբ custom ընտրանքները ձուլված են, նրանք օգտագործում են հիմնական ստրատեգիան որը վերագրում է գոյություն ունեցող արժեքը։ Եթե դուք ցանկանում եք որ custom ընտրանքը ձուլվի օգտագործելով հատուկ տրամաբանություն, դուք պետք է միացնեք ֆունկցիա `Vue.config.optionMergeStrategies`֊ին․

``` js
Vue.config.optionMergeStrategies.myOption = function (toVal, fromVal) {
  // return mergedVal
}
```

Շատ օբյեկտով հիմնված ընտրանքներում, դուք կարող եք օգտագործել նույն ստրատեգիան որը օգտագործվում է `methods`֊ի կողմից․

``` js
var strategies = Vue.config.optionMergeStrategies
strategies.myOption = strategies.methods
```

Ավելի խորացված օրինակի համար կարող եք նայել [Vuex](https://github.com/vuejs/vuex)֊ի 1.x ձուլման ստրատեգիան․

``` js
const merge = Vue.config.optionMergeStrategies.computed
Vue.config.optionMergeStrategies.vuex = function (toVal, fromVal) {
  if (!toVal) return fromVal
  if (!fromVal) return toVal
  return {
    getters: merge(toVal.getters, fromVal.getters),
    state: merge(toVal.state, fromVal.state),
    actions: merge(toVal.actions, fromVal.actions)
  }
}
```
