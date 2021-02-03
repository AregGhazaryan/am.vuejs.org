---
title: Տեղադրում
type: ուղեցույց
order: 1
vue_version: 2.5.16
gz_size: "30.90"
---

### Համապատասխանության Նշում

Vue-ն **չի** աշխատում IE8-ում և ավելի հին տարբերակներում, որովհետև Vue-ն օգտագործում է ECMAScript 5 հատկանիշներ որոնք բացակայում են IE8-ի մեջ: Այստեղ կարող եք տեսնել բոլոր այն [բրաուզերները որոնք համապատասխանում են ECMAScript 5-ին](https://caniuse.com/#feat=es5)։

### Սեմանտիկ Տարբերակում

Vue-ն հետևում է [Սեմանտիկ Տարբերակման](https://semver.org/)-ը իր բոլոր պաշտոնական նախագծերի դոկումենտացիայի և աշխատաձևի համար։ Չ՛փաստաթղթավորված աշխատաձի կամ այլ բաց ներքին փոփոխությունները, նկարագրված են [թողարկման նշումներում](https://github.com/vuejs/vue/releases)։

### Թողարկման նշումներ

Վերջին կայուն տարբերակը։ {{vue_version}}

Մանրամասն թողարկման նշումները ամեն տարբերակի համար հասանելի են այստեղ [GitHub](https://github.com/vuejs/vue/releases):

## Vue Devtools

Երբ օգտագործում ենք Vue, մենք խորհուրդ ենք տալիս տեղադրել [Vue Devtools-ը](https://github.com/vuejs/vue-devtools#vue-devtools) ձեր բրաուզերում, որը ձեզ թույլ է տալիս inspect և debug անել ձեր Vue ծրագրերը ավելի հարմար interface-ում:

## Ուղիղ `<script>`  Ներառում

Ուղղակի ներբեռնեք և ներառեք script tag-ով։ `Vue`-ն կգրանցվի որպես գլոբալ փոփոխական։

<p class="tip">Զարգացման ժամանակ մի օգտագործեք minified տարբերակը: Այդ դեպքում ձեր մոտ կբացակայի նախազգուշացումները (warnings) հասարակ սխալների համար՜</p>

<div id="downloads">
  <a class="button" href="/js/vue.js" download>Զարգացման Տարբերակ</a><span class="light info">Բոլոր նախազգուշացումներով և debug ռեժիմով</span>

  <a class="button" href="/js/vue.min.js" download>Արտադրության Տարբերակ</a><span class="light info">Առանց նապազգուշացումներով, {{gz_size}}KB min+gzip</span>
</div>

### CDN

Prototyping-ի կամ սովորելու համար կարող եք նաև օգտագործել ամենանոր տարբերակը հետևյալ ձևով։

``` html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

Արտադրության ժամանակ, մենք խորհուրդ ենք տալիս օգտագործել կոնկրետ տարբերակ որպեսզի խուսափեք անսպասելի խափանումներից ավելի նոր տարբերակներում։

``` html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.0"></script>
```

Եթե դուք օգտագործում եք native ES մոդուլները, մենք ունենք նաև ES Modules-ին համապատասխան տարբերակ:

``` html
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.0/dist/vue.esm.browser.js'
</script>
```

Դուք կարող եք իմանալ ավելին NPM-ի package-ի մասին օգտագործելով հետևյալ հղումը [cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue/).

Vue-ն նաև հասանելի է [unpkg-ում](https://unpkg.com/vue@{{vue_version}}/dist/vue.js) և [cdnjs-ում](https://cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.js) (cdnjs կարող է որոշ ժամանակ տանել որպեսզի համաժամեցնել, այնպես որ ամենավերջին տարբերակը հնարավոր է որ հասանելի չ՛լինի։

Համոզվեք որ դուք կարդացել եք [Vue-ի տարբերակների](#Explanation-of-Different-Builds) մասին, և օգտագործեք **արտադրության տարբերակը** ձեր հրատարակած կայքում, փոխելով `vue.js`-ը `vue.min.js`-ի հետ։ Այս տարբերակը ավելի փոքր է և օպտիմիզացված է արագության համար, զարգացման փորձառության փոխարեն.

## NPM

NPM-ով խորհուրդ է տրվում տեղադրել երբ որ կառուցում եք մեծածավալ ծրագրեր Vue-ով։ Այն լավ զուգակցվում է մոդուլների bundler-ների հետ ինչպիսիք են [Webpack-ը](https://webpack.js.org/) կամ [Browserify-ը](http://browserify.org/)։ Vue-ն նաև տրամադրում է գործիքներ հեղինակելու համար [Մեկ ֆայլ կոմպոնենտներ](single-file-components.html)։

``` bash
# վերջին կայուն տարբերակ
$ npm install vue
```

## CLI

Vue-ն տրամադրում է [պաշտոնական CLI](https://github.com/vuejs/vue-cli) որ ավելի արագ կերպով ձեզ համար կտեղադրի մեկ էջ ծրագրի հենք (scaffolding)։ Այն ներառում է մարտկոցներով ներկառուցված տեղադրում ավելի ժամանակակից frontend աշխատընթացների համար։ Այն խլում է միայն մի քանի րոպե որպեսզի աշխատի և ապահովի ծրագիրը hot-reload-ով, lint-on-save, և production-ready build-ներով: Կարդացեք [Vue CLI-ի դոկումենտացիան](https://cli.vuejs.org) ավելի մանրամասների համար:

<p class="tip">CLI—ը ենթադրում որ դուք գիտեք թէ ինչ է Node.js-ը և նրա հետ կապված կառուցման գործիքները: Եթե դուք նոր եք ծանոթանում Vue-ի հետ կամ front-end կառուցման գործիքների հետ, մենք խորհուրդ ենք տալիս անցնել <a href="./"> այս ուղեցույցը </a> առանց կառուցման գործիքների նախքան CLI օգտագործելը։</p>

<div class="vue-mastery"><a href="https://www.vuemastery.com/courses/real-world-vue-js/vue-cli" target="_blank" rel="sponsored noopener" title="Vue CLI">Դիտել բացատրական հոլովակ Vue Mastery-ում</a></div>

## Տարբեր Build-ների բացատրությունը

[`dist/` բաժնում NPM package-ի մեջ](https://cdn.jsdelivr.net/npm/vue/dist/) դուք կարող եք փնտրել տարբեր build-ներ Vue.js-ի։ Ահա ցուցակ որը ցույց է տալիս տարբերությունները:

| | UMD | CommonJS | ES Մոդուլ (bundler-ների համար) | ES Մոդուլ (բրաուզերների համար) |
| --- | --- | --- | --- | --- |
| **Ամբողջովին** | vue.js | vue.common.js | vue.esm.js | vue.esm.browser.js |
| **Միայն Runtime** | vue.runtime.js | vue.runtime.common.js | vue.runtime.esm.js | - |
| **Ամբողջովին (արտադրություն)** | vue.min.js | - | - | vue.esm.browser.min.js |
| **Միայն Runtime (արտադրություն)** | vue.runtime.min.js | - | - | - |

### Պայմաններ

- **Ամբողջովին**: build որը պարունակում է compiler-ը և runtime-ը.

- **Compiler**: կոդ որը պատասխանատու է compile անելու ձևանմուշի string-ները դեպի JavaScript տպելու ֆունկցիաներ։

- **Runtime**: կոդ որը պատասխանատու է ստեղծել Vue-ի օբյեկտ, տպելու և միացնելու virtual DOM-ը, և այլն։ Պարզապես ունի ամեն ինչ բացի compiler-ից։

- **[UMD](https://github.com/umdjs/umd)**: UMD build-ը կարելի է օգտագործել ուղիղ բրաուզերի մեջ օգտագործելով `<script>` tag։ Հիմնական ֆայլը jsDelivr CDN-ից է [https://cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue) այն պարունակում է Runtime + Compiler UMD build-ը (`vue.js`)։

- **[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)**: CommonJS build-ները նախատեսված են այլ bundler-ների համար ինչպիսիք են [browserify-ը](http://browserify.org/) կամ [webpack 1-ը](https://webpack.github.io)։ Հիմնական ֆայլը այս bundler-ների համար  (`pkg.main`) է որը միայն Runtime CommonJS build է (`vue.runtime.common.js`)։

- **[ES Մոդուլ](http://exploringjs.com/es6/ch_modules.html)**: Vue-ի 2․6-ից տրամադրվում է երկու ES Մոդուլ (ESM) build-ներ:

  - ESM-ի bundler-ները: նախատեսված է ժամանակակից bundler-ների համար ինչպիսիք են [webpack 2-ը](https://webpack.js.org) կամ [Rollup-ը](https://rollupjs.org/)։ ESM format-ը դիզայանավորված է որպեսզի ստատիկորեն վերլուծվի և bundler-ները կարողանան կատարեն "tree-shaking" և դուրս հանել չ՛օգտագործած կոդը ձեր վերջին bundle-ում: Հիմնական ֆայլը այս bundler-ների համար (`pkg.module`)-ն է որը միայն Runtime է ES Մոդուլ build-ի (`vue.runtime.esm.js`)։

  - ESM-ը բրաուզերների համար (2.6+ միայն)․ նախատեսված է ուղիղ ներմուծելու ժամանակակից բրաուզերներում օգտագործելով `<script type="module">`.

### Runtime + Compiler ընդեմ Runtime-only

Եթե դուք ցանկանում եք compile անեք ձևանմուշներ օգտագործողի մոտ (օրինակ՝ փոխանցել string `template` տարբերակում, կամ mount անել էլեմենտին օգտագործելով միայն իր DOM HTML-ը որպես ձևանմուշ), ձեզ անհրաժեշտ կլինի compiler-ը և վերջին build-ը։

``` js
// սա պահանջում է compiler-ը
new Vue({
  template: '<div>{{ բարև }}</div>'
})

// սա ոչ
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```

Երբ օգտածգործում ենք `vue-loader` կամ `vueify`, ձևանմուշներ `*.vue`-ի մեջ, ֆայլերը pre-compile եղած են լինում JavaScript-ի մեջ build-ի ժամանակ։ Ձեզ պետք չի compiler վերջին bundle-ում, և դուք կարող եք օգտագործել միայն runtime build-ը։

Ի վեր միայն runtime build-ները համարյա 30%-ով ավելի թեթև են քան ամբողջական build-ները, դուք պետք է օգտագործեք այն երբ կարող եք։ Եթե դուք ցանկանում եք օգտագործել ամբողջական build-ը նրա փոխարեն, դուք պետք է alias տրամադրեք ձեր bundler-ում։

#### Webpack

``` js
module.exports = {
  // ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' webpack 1-ի համար
    }
  }
}
```

#### Rollup

``` js
const alias = require('rollup-plugin-alias')

rollup({
  // ...
  plugins: [
    alias({
      'vue': require.resolve('vue/dist/vue.esm.js')
    })
  ]
})
```

#### Browserify

Ավելացրեք ձեր նախագիծը `package.json`-ում:

``` js
{
  // ...
  "browser": {
    "vue": "vue/dist/vue.common.js"
  }
}
```

#### Parcel

Ավելցարեք ձեր նախագիծը `package.json`-ում:

``` js
{
  // ...
  "alias": {
    "vue" : "./node_modules/vue/dist/vue.common.js"
  }
}
```

### Զարգացման Տեսակը ընդեմ Արտադրության Տեսակի

Զարգացման/արտադրության տեսակները hard-code են արած UMD build-ների համար։ un-minified ֆայլերը զարգացման համար են, և minified ֆայլերը արտադրության։

CommonJS և ES Մոդուլ build-ները նախատեսված են bundler-ների համար, որի հետևանքով մենք չենք տրամադրում minified տարբերակներ նրանց համար։ Դուք պատասխանատու կլինեք minify անելու ձեր վերջին bundle-ը։

CommonJS և ES Մոդուլ build-ները նաև տրամադրում raw ստուգումներ `process.env.NODE_ENV`-ի համար որպեսզի որոշվի այն տեսակը որով պետք է այն աշխատի։ Դուք պետք է օգտագործեք համապատասխան bundler-ի configuration-ը որպեսզի փոփոխեք enviroment փոփոխականները և կառավարեք թէ ինչ տեսակով Vue-ն պետք է աշխատի։ Փոփոխելով `process.env.NODE_ENV`-ին string literal-ների հետ, նաև թույլ է տալիս minifier-ները ինչպիսիք են UglifyJS որպեսզի ամբողջովին վերացնի զարգացման համար նախատեսված կոդի բլոկները, նվազեցնելով վերջին ֆայլի չափսը։

#### Webpack

Webpack 4+-ի մեջ դուք կարող եք օգտագործել `mode` option-ը:

``` js
module.exports = {
  mode: 'production'
}
```

Բայց Webpack 3 և ավելի հին տարբերակներում, դուք պետք է օգտագործեք [DefinePlugin](https://webpack.js.org/plugins/define-plugin/):

``` js
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify('production')
      }
    })
  ]
}
```

#### Rollup

Օգտագործեք [rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace):

``` js
const replace = require('rollup-plugin-replace')

rollup({
  // ...
  plugins: [
    replace({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
}).then(...)
```

#### Browserify

Տվեք գլոբալ [envify](https://github.com/hughsk/envify) transform ձեր bundle-ին.

``` bash
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

Նաև նայեք [Արտադրության Տեղակայման Հուշումներ](deployment.html).

### CSP environment-ներ

Որոշ environment-ներ, ինչպիսիք են Google Chrome App-ը, պաշտպանված են Բովանդակության Անվտանգության Քաղաքականությունով (CSP), որը արգելում է `new Function()`-ի օգտագործումը։ Ամբողջ build-ը կախված է այս հատկության վրա որպեսզի compile անի ձևանմուշները, և օգտագործելի լինի այն այս environment-ներում։

Մյուս դեպքում, միայն runtime build-ը ամբողջովին համապատասխանում է CSP-ին։ Երբ օգտագործում ենք միայն runtime build-ը [Webpack + vue-loader-ի](https://github.com/vuejs-templates/webpack-simple) կամ [Browserify + vueify-ի](https://github.com/vuejs-templates/browserify-simple) հետ, ձեր ձևանմուշները precompile կլինեն դեպի `render` ֆունկցիա որը հիանալի է աշխատում CSP environment-ներում։

## Զարգացման Build

**Կարևոր**: կառուցված ֆայլերը GitHub-ի `/dist` պանակում միայն ստուգվում են թողարկումների ժամանակ։ Որպեսզի օգտագործեք Vue-ն վերջին կոդից որը որ գտնվում է GitHub-ում, դուք պետք է build անեք ինքներտ՜

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

Միայն UMD build-ներն են հասանելի Bower-ից

``` bash
# latest stable
$ bower install vue
```

## AMD Մոդուլի Բեռնողներ

Բոլոր UMD build-ները կարող են օգտագործվել ուղիղ ինչպես AMD Մոդուլ։
