---
title: TypeScript-ի Համապտասխանեցում
type: ուղեցույց
order: 403
---

> [Vue CLI-ը](https://cli.vuejs.org) դրամադրում է արդեն պատրաստ TypeScript-ի գործիքներ։

## Պաշտոնական Հայտարարումը NPM Գրադարաններում

Ստատիկ տիպի համակարգը կարող է մեզ օգնել խուսափել բազմաթիվ runtime error-ներից, հատկապես երբ ծրագիրը աճում է։ Այս պատճառով Vue-ն ունի [պաշտոնական տիպի հայտարարումները](https://github.com/vuejs/vue/tree/dev/types) [TypeScript-ի համար](https://www.typescriptlang.org/) - ոչ միայն Vue-ի հիմնական գրադարանում, այլ նաև [vue-router-ում](https://github.com/vuejs/vue-router/tree/dev/types) և [vuex-ում](https://github.com/vuejs/vuex/tree/dev/types) նույնպես։

Մինչ նրանք [հրատարակված են NPM-ում](https://cdn.jsdelivr.net/npm/vue@2/types/), և վերջին TypeScript-ը գիտի թե ինչպես վերլուծի տիպերի հայտարարումը NPM գրադարաններում, սա նշանակում է երբ այն տեղադրված է NPM-ի շնորհիվ, ձեզ անհրաժեշտ չէ հավելյալ գործիքներ որպեսզի օգտագործել TypeScript-ը Vue-ի հետ։

## Առաջարկվող Կոնֆիգուրացիան

``` js
// tsconfig.json
{
  "compilerOptions": {
    // սա նշում է Vue-ի բրաուզերի համապատասխանեցումը
    "target": "es5",
    // սա տրամադրում է մեզ ավելի խիստ ինտերֆեյս տվյալների հատկությունների համար `this`-ում
    "strict": true,
    // եթե օգտագործում եք webpack 2+ կամ rollup, որպեսզի օգտագործել tree shaking․
    "module": "es2015",
    "moduleResolution": "node"
  }
}
```

Նշում որ դուք պետք է ներառեք `strict: true` (կամ գոնե `noImplicitThis: true` որը `strict` նշանի մի մասն է) որպեսզի օգտագործել տիպի ստուգում `this`-ի վրա կոմպոնենտի մեթոդներում, հակառակ դեպքում այն միշտ կվարվի որպես `any` տիպ։

Նայեք [TypeScript-ի compiler-ի ընտրանքների փաստաթղթերը](https://www.typescriptlang.org/docs/handbook/compiler-options.html) ավելի մանրամասների համար։

## Զարգացման Գործիքներ

### Նախագծի Ստեղծում

[Vue CLI 3](https://github.com/vuejs/vue-cli) կարող է ստեղծել նոր նախագծեր որոնք օգտագործում են TypeScript։ Որպեսզի սկսենք․

```bash
# 1. Տեղադրեք Vue CLI, եթե այն դեռ տեղադրված չե
npm install --global @vue/cli

# 2. Ստեղծեք նոր նախագիծ, այնուհետև ընտրեք "Manually select features" (Ձեռքով ընտրել նախընտրանքները) ընտրանքը
vue create my-project-name
```

### Խմբագրման Համապատասխանեցում

Որպեսզի զարգացնել Vue ծրագրեր TypeScript-ի հետ, մենք խորհուրդ ենք տալիս օգտագործել [Visual Studio Code-ը](https://code.visualstudio.com/), որը տրամադրում է հիանալի համապատասխանեցում TypeScript-ի համար։ Եթե դուք օգտագործում եք [մեկ-ֆայլ կոմպոնենտները](./single-file-components.html) (SFC-ներ), տեղադրեք հրաշալի [Vetur extension-ը](https://github.com/vuejs/vetur), որը տրամադրում է TypeScript-ի ինտերֆեյս SFC-ների ներքո և այլ շատ լավ հատկություններ։

[WebStorm](https://www.jetbrains.com/webstorm/) նաև տրամադրում է համապատասխանեցում TypeScript-ի և Vue-ի համար։

## Սովորական Օգտագործում

Որպեսզի թույլ տալ TypeScript-ի ճշտգրտորեն նշի տիպերը Vue կոմպենտնի ընտրանքներում, դուք պետք է հայտարարեք կոմպոնենտները `Vue.component` կամ `Vue.extend`-ով․

``` ts
import Vue from 'vue'

const Component = Vue.extend({
  // տիպերի ինտերֆեյսը միացված է
})

const Component = {
  // սա չի ունենա տիպի ինտերֆեյս,
  // որովհետև TypeScript-ը չի հասկանա որ սա ընտրանք է Vue կոմպոնենտի համար։
}
```

## Class-ի Ոճով Vue Կոմպոնենտներ

Եթե դուք նախընտրում եք class-ով հիմնված API երբ հայտարարում եք կոմպոնենտներ, դուք կարող եք օգտագործել պաշտոնական [vue-class-component](https://github.com/vuejs/vue-class-component) դեկորատորը․

``` ts
import Vue from 'vue'
import Component from 'vue-class-component'

// @Component դեկորատորը ցույց է տալիս որ class-ը Vue component է
@Component({
  // Բոլոր կոմպոնենտի ընտրանքրը թույլ են տրվում այստեղ
  template: '<button @click="onClick">Click!</button>'
})
export default class MyComponent extends Vue {
  // Սկզբնական տվյալները կարող են հայտարարվել որպես instance-ի հատկություններ
  message: string = 'Hello!'

  // Կոմպոնենտի մեթոդները կարող են հայտարարվել որպես instance-ի մեթոդներ
  onClick (): void {
    window.alert(this.message)
  }
}
```

## Տիպերի Աուգմենտացիան Plugin-ների Հետ Օգտագործման Համար

Plugin-ները կարող են ավելացնել global/instance հատկություններ և կոմպոնենտի ընտրանքներ Vue-ում։ Այս դեպքերում, տիպերի հայտարարումը պահանջվում է որպեսզի plugin-ները compile լինեն TypeScript-ում։ Բարեբախտորեն, գոյություն ունի TypeScript-ի հատկություն որպեսզի ագումենտացիայի ենթարկել գոյություն ունեցող տիպերը որը անվանվում է [մոդուլի աուգմենտացիա](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#module-augmentation)։

Օրինակի համար, որպեսզի հայտարարել instance-ի հատկություն `$myProperty` `string` տիպով․

``` ts
// 1. Համողվեք որ import 'vue'-ն ավելացված է նախքան աուգմենտացված տիպերի հայտարարումը
import Vue from 'vue'

// 2. Նշեք ֆայլը տիպերով հանդերձ որոնք որ դուք ցանկանում եք աուգմենտացնել
//    Vue-ն ունի կոնստրուկտոր տիպ types/vue.d.ts-ում
declare module 'vue/types/vue' {
  // 3. Հայտարարեք աուգմենտացիա Vue-ի համար
  interface Vue {
    $myProperty: string
  }
}
```

Վերևի կոդը ձեր նախագծում որպես հայտարարման ֆայլ ներառելուց հետո (ինչպես `my-property.d.ts`), դուք կարող եք օգտագործել `$myProperty` Vue instance-ում։

```ts
var vm = new Vue()
console.log(vm.$myProperty) // Սա պետք է հաջողությամբ compile լինի
```

Դուք կարող եք նաև հայտարարել հավելյալ գլոբալ հատկություններ և կոմպոնենտի ընտրանքներ․

```ts
import Vue from 'vue'

declare module 'vue/types/vue' {
  // Գլոբալ հատկությունները կարող են հայտարարվել
  // `VueConstructor` ինտերֆեյսում
  interface VueConstructor {
    $myGlobal: string
  }
}

// ComponentOptions-ը հայտարարված է types/options.d.ts
declare module 'vue/types/options' {
  interface ComponentOptions<V extends Vue> {
    myOption?: string
  }
}
```

Վերևում հայտարարները թույլ են տալիս որ հետևյալ կոդը compile լինի․

```ts
// Գլոբալ հատկություն
console.log(Vue.$myGlobal)

// Հավելյալ կոմպոնենտի ընտրանք
var vm = new Vue({
  myOption: 'Բարև'
})
```

## Վերադարձի Տիպերի Նշումները

Vue-ի հայտարարման ֆայլերի օղակային կազմվածքի պատճառով, TypeScript-ը կարող է ունենալ դժվարություններ եզրակացնելու տիպերը որոշ մեթոդների։ Այս պատճառով, դուք պետք է նշեք վերադարձի տիպը մեթոդներում ինչպիսիք են `render` և այն բոլորը որոնք `computed`-ում են։

```ts
import Vue, { VNode } from 'vue'

const Component = Vue.extend({
  data () {
    return {
      msg: 'Hello'
    }
  },
  methods: {
    // պետք է նշում որովհետև `this`-ն է վերադառնում տիպով
    greet (): string {
      return this.msg + ' world'
    }
  },
  computed: {
    // պետք է նշում
    greeting(): string {
      return this.greet() + '!'
    }
  },
  // `createElement`-ը եզրակացրած է, բայց `render`-ին պետք է վերադարձման տիպ
  render (createElement): VNode {
    return createElement('div', this.greeting)
  }
})
```

Եթե տիպերի եզրակացումը չի աշխատում, որոշ մեթոդների նշումը կարող է օգնել լուծել այս խնդիրները։ Օգտագործելով `--noImplicitAny` ընտրանքը կօգնի փնտրել նման շատ չնշված մեթոդներ։



## Հատկությունների Նշումները

```ts
import Vue, { PropType } from 'vue'

interface ComplexMessage {
  title: string,
  okMessage: string,
  cancelMessage: string
}
const Component = Vue.extend({
  props: {
    name: String,
    success: { type: String },
    callback: {
      type: Function as PropType<() => void>
    },
    message: {
      type: Object as PropType<ComplexMessage>,
      required: true,
      validator (message: ComplexMessage) {
        return !!message.title;
      }
    }
  }
})
```
Եթե ձեր վալիդատորը չի ստանում տիպերի ինտերֆեյսը, կամ այն չի աշխատում, նշելով արգումենտը սպասվող տիպով կարող է օգնել ձեզ լուծել այս խնդիրները։
