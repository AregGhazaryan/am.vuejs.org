---
title: Արտադրության Deployment
type: guide
order: 404
---

> Ստորև բերված խորհուրդների մեծամասնությունը միացված է հիմնականում, եթե օգտագործում եք [Vue CLI](https://cli.vuejs.org)։ Այս բաժինը տեղին է միայն այն դեպքում, եթե դուք օգտագործում եք custom build setup:

## Արտադրության Ռեժիմի Միացումը

Զարգացման ընթացքում, Vue-ն տրամադրում է շատ նախազգուշացումներ որպեսզի օգնի ձեզ հասարակ սխալներով և այլ խնդիրներով։ Սակայն, այդ նախագուշացման տողերը դառնում են անպետք արտադրության մեջ և մեծացնում են ձեր ծրագրի payload-ի չափսը։ Ի հավելումն, այս նախազգուշացումներից որոշները ունեն փոքր runtime-ի ծախս որը կարող ենք խուսափել արտադրության մեջ։

### Առանց Կառուցման Գործիքների

Եթե դուք օգտագործում էք full build, օրինակ․ ուղիղ ներառում եք Vue-ն script tag-ի շնորհիվ առանց կառուցման գործիքի, համոզվեք որ դուք օգտագործում եք minified տարբերակը (`vue.min.js`) արտադրության համար։ Երկու տարբերակներնել կարող են հանդիպել [Տեղադրման Ուղեցույցում](installation.html#Direct-lt-script-gt-Include)։

### Կառուցման Գործիքների Հետ

Երբ օգտագործում եք կառուցման գործիք ինչպիսին են Webpack կամ Browserify-ը, արտադրության ռեժիմը կախված կլինի `process.env.NODE_ENV`-ից որը գտնվում է Vue-ի source կոդի մեջ, և այն կօգտագործի զարգացման ռեժիմը հիմնականում։ Երկու կառուցման գործիքներն ել հնարավորթյուն են տալիս վերագրել այս փոփոխականը որպեսզի միացնել Vue-ի արտադրության ռեժիմը, և բոլոր նախազգուշացումները կջնջվեն minifier-ների կողմից կառուցման ժամանակ։ Բոլոր `vue-cli` ձևանմուշները ունեն նախնական կարգավորումներ, բայց օգտակար է իմանալ թե ինչպես են նրանք արվում։

#### Webpack

Webpack 4+-ի մեջ, դուք կարող եք օգտագործել `mode` ընտրանքը․

``` js
module.exports = {
  mode: 'production'
}
```

Բայց Webpack 3 և ավելի վաղ տարբերակներում, դուք պետք է օգտագործեք [DefinePlugin](https://webpack.js.org/plugins/define-plugin/)․

``` js
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
}
```

#### Browserify

- Աշխատացրեք ձեր bundling հրամանը իրական `NODE_ENV` environment փոփոխականը դրված `"production"`-ի վրա։ Սա տեղյակ կպահի `vueify` որպեսզի խուսափել ներառելուց hot-reload և զարգացման հետ կապված կոդը։

- Կիրառեք գլոբալ [envify](https://github.com/hughsk/envify) transform ձեր bundle-ին։ Սա թույլ է տալիս minifier-ին որպեսզի ջնջի բոլոր նախազգուշացումները Vue-ի source կոդից փաթաթված env փոփոխականի պայմանական բլոկներում։

  ``` bash
  NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
  ```

- Կամ, օգտագործելով [envify](https://github.com/hughsk/envify) Gulp-ի հետ.

  ``` js
  // Օգտագործեք envify custom մոդուլը որպեսզի նշեք environment փոփոխականները
  var envify = require('envify/custom')

  browserify(browserifyOptions)
    .transform(vueify)
    .transform(
      // Պահանջված է որպեսզի աշխատացնի node_modules-ի ֆայլերը
      { global: true },
      envify({ NODE_ENV: 'production' })
    )
    .bundle()
  ```

- Կամ, օգտագործելով [envify](https://github.com/hughsk/envify) Grunt-ի հետ և [grunt-browserify](https://github.com/jmreidy/grunt-browserify)․

  ``` js
  // Օգտագործեք envify custom մոդուլը որպեսզի նշեք environment փոփոխականները
  var envify = require('envify/custom')

  browserify: {
    dist: {
      options: {
        // Ֆունկցիա որպեսզի շեղվել grunt-browserify-ի հիմնական հերթականությունից
        configure: b => b
          .transform('vueify')
          .transform(
            // Պահանջված է որպեսզի աշխատացնի node_modules-ի ֆայլերը
            { global: true },
            envify({ NODE_ENV: 'production' })
          )
          .bundle()
      }
    }
  }
  ```

#### Rollup

Օգտագործեք [@rollup/plugin-replace](https://github.com/rollup/plugins/tree/master/packages/replace)․

``` js
const replace = require('@rollup/plugin-replace')

rollup({
  // ...
  plugins: [
    replace({
      'process.env.NODE_ENV': JSON.stringify( 'production' )
    })
  ]
}).then(...)
```

## Ձևանմուշների Pre-Compiling-ը

Երբ օգտագործում ենք DOM-ի մեջի ձևանմուշներ կամ JavaScript-ի մեջի ձևանմուշի string-ները, ձևանմուշի render-ի ֆունկցիաները կատարվում են օդի մեջ։ Սա հիմնական բավականին արագ է շատ դեպքերում, բայց լավագույն դեպքում պետք է խուսափել եթե ձեր ծրագիրը զգայուն է performance-ի հանդեպ։

Ամենահեշտ ճանափարհը pre-compile անելու ձևանմուշները դա [Մեկ Ֆայլ Կոմպոնենտների](single-file-components.html) օգտագործումն է - կապակցված կառուցման կարգավորումները ավտոմատ կերպով pre-compile են անուն ձեր համար, այնպես որ կառուցված կոդը պարունակում է արդեն compile եղած render ֆունկցիաները չփոփոխված ձևանմուշների string-ների փոխարեն։

Եթե դուք օգտագործում եք Webpack, և նախընտրում եք բաժանել JavaScript և ձևանմուշի ֆայլերը, դու կարող եք օգտագործել [vue-template-loader-ը](https://github.com/ktsn/vue-template-loader), որև նաև դառձնում է ձևանմուշի ֆայլերը JavaScript render ֆունկցիաներ կառուցման քայլում։ 

## Կոմպոնենտի CSS-ի Ստացումը

Երբ օգտագործում եք մեկ ֆայլ կոմպոնենտներ, կոմպոնենտների մեջի CSS-ը ներարկվում է դինամիկորեն որպես `<style>` tag-երով JavaScript-ի շնորհիվ։ Սա ունի փոքր runtime-ի ծախս, և եթե դուք օգտագործում եք server-side rendering այն կպատճառի «առանց ոճի բովանդակության ակընթարթ»։ Ստալաով CSS-ը բոլոր կոմպոնենտներից դեպի նույն ֆայլ կօգնի մեզ խուսափել այս խնդիրից, և արդյունքում կլավացնի CSS-ի minification-ը և caching-ը։ 

Դիմեք համապատասխան կառուցման գործիքների փաստաթղթերին որպեսզի տեսնեք թե ինչպես է դա արվում․

- [Webpack + vue-loader](https://vue-loader.vuejs.org/en/configurations/extract-css.html) (`vue-cli` webpack ձևանմուշը ունի սա նախնական կարգավորված)
- [Browserify + vueify](https://github.com/vuejs/vueify#css-extraction)
- [Rollup + rollup-plugin-vue](https://vuejs.github.io/rollup-plugin-vue/#/en/2.3/?id=custom-handler)

## Runtime Սխալների Հետևումը

Եթե runtime սխալ է հայտվում կամպոնենտի render-ի ժամանակ, այն կփոխանցվի գլոբալ `Vue.config.errorHandler`-ի կարգավորման ֆունկցիային եթե այն դրված չէ։ Կարող է լավ միտք լինել որպեսզի օգտագործել այս hook-ը error-tracking service-ի հետ համատեղ ինչպիսին է [Sentry](https://sentry.io), որը տրամադրում է [պաշտոնական տեղադրում](https://sentry.io/for/vue/) Vue-ի համար։