---
title: Դինամիկ և Async Կոմպոնենտներ
type: ուղեցույց
order: 105
---

> Այս էջը ենթադրում է որ դուք արդեն կարդացել եք [Կոմպոնենտների Հիմունքները](components.html)։ Կարդացեք այն եթե դուք նոր եք ծանոթանում կոմպոնենտներին։

## `keep-alive`-ը դինամիկ կոմպոնենտների հետ

Ավելի վաղ, մենգ օգտագործել ենք `is` ատրիբուտը որպեսզի փոխենք կոմպոնենտները tabbed interface-ում։

{% codeblock lang:html %}
<component v-bind:is="currentTabComponent"></component>
{% endcodeblock %}

Երբ փոփոխում ենք այս կոմպոնենտները, հնարավոր է որ երբեմն դուք կցանկանաք կառավարել նրանց վիճակը կամ խուսափել re-rendering-ից performance-ի պատճառներով։ Օրինակ՝ երբ երկարացնում ենք մեր tabbed interface-ի մի փոքր։

{% raw %}
<div id="dynamic-component-demo" class="demo">
  <button
    v-for="tab in tabs"
    v-bind:key="tab"
    v-bind:class="['dynamic-component-demo-tab-button', { 'dynamic-component-demo-active': currentTab === tab }]"
    v-on:click="currentTab = tab"
  >{{ tab }}</button>
  <component
    v-bind:is="currentTabComponent"
    class="dynamic-component-demo-tab"
  ></component>
</div>
<script>
Vue.component('tab-posts', {
  data: function () {
    return {
      posts: [
        {
          id: 1,
          title: 'Cat Ipsum',
          content: '<p>Dont wait for the storm to pass, dance in the rain kick up litter decide to want nothing to do with my owner today demand to be let outside at once, and expect owner to wait for me as i think about it cat cat moo moo lick ears lick paws so make meme, make cute face but lick the other cats. Kitty poochy chase imaginary bugs, but stand in front of the computer screen. Sweet beast cat dog hate mouse eat string barf pillow no baths hate everything stare at guinea pigs. My left donut is missing, as is my right loved it, hated it, loved it, hated it scoot butt on the rug cat not kitten around</p>'
        },
        {
          id: 2,
          title: 'Hipster Ipsum',
          content: '<p>Bushwick blue bottle scenester helvetica ugh, meh four loko. Put a bird on it lumbersexual franzen shabby chic, street art knausgaard trust fund shaman scenester live-edge mixtape taxidermy viral yuccie succulents. Keytar poke bicycle rights, crucifix street art neutra air plant PBR&B hoodie plaid venmo. Tilde swag art party fanny pack vinyl letterpress venmo jean shorts offal mumblecore. Vice blog gentrify mlkshk tattooed occupy snackwave, hoodie craft beer next level migas 8-bit chartreuse. Trust fund food truck drinking vinegar gochujang.</p>'
        },
        {
          id: 3,
          title: 'Cupcake Ipsum',
          content: '<p>Icing dessert soufflé lollipop chocolate bar sweet tart cake chupa chups. Soufflé marzipan jelly beans croissant toffee marzipan cupcake icing fruitcake. Muffin cake pudding soufflé wafer jelly bear claw sesame snaps marshmallow. Marzipan soufflé croissant lemon drops gingerbread sugar plum lemon drops apple pie gummies. Sweet roll donut oat cake toffee cake. Liquorice candy macaroon toffee cookie marzipan.</p>'
        }
      ],
      selectedPost: null
    }
  },
  template: '\
    <div class="dynamic-component-demo-posts-tab">\
      <ul class="dynamic-component-demo-posts-sidebar">\
        <li\
          v-for="post in posts"\
          v-bind:key="post.id"\
          v-bind:class="{ \'dynamic-component-demo-active\': post === selectedPost }"\
          v-on:click="selectedPost = post"\
        >\
          {{ post.title }}\
        </li>\
      </ul>\
      <div class="dynamic-component-demo-post-container">\
        <div \
          v-if="selectedPost"\
          class="dynamic-component-demo-post"\
        >\
          <h3>{{ selectedPost.title }}</h3>\
          <div v-html="selectedPost.content"></div>\
        </div>\
        <strong v-else>\
          Սեղմեք բլոգի վերնագրի վրա ձախ կողմում որպեսզի դիտեք այն։\
        </strong>\
      </div>\
    </div>\
  '
})
Vue.component('tab-archive', {
  template: '<div>Արխիվ կոմպոնենտ</div>'
})
new Vue({
  el: '#dynamic-component-demo',
  data: {
    currentTab: 'Posts',
    tabs: ['Posts', 'Archive']
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
  overflow-anchor: none;
}
.dynamic-component-demo-tab-button:hover {
  background: #e0e0e0;
}
.dynamic-component-demo-tab-button.dynamic-component-demo-active {
  background: #e0e0e0;
}
.dynamic-component-demo-tab {
  border: 1px solid #ccc;
  padding: 10px;
}
.dynamic-component-demo-posts-tab {
  display: flex;
}
.dynamic-component-demo-posts-sidebar {
  max-width: 40vw;
  margin: 0 !important;
  padding: 0 10px 0 0 !important;
  list-style-type: none;
  border-right: 1px solid #ccc;
}
.dynamic-component-demo-posts-sidebar li {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  cursor: pointer;
}
.dynamic-component-demo-posts-sidebar li:hover {
  background: #eee;
}
.dynamic-component-demo-posts-sidebar li.dynamic-component-demo-active {
  background: lightblue;
}
.dynamic-component-demo-post-container {
  padding-left: 10px;
}
.dynamic-component-demo-post > :first-child {
  margin-top: 0 !important;
  padding-top: 0 !important;
}
</style>
{% endraw %}

Դուք կնկատեք որ եթե դուք ընտեք գրառում, հետո փոխելով դեպի _Archive_ tab, հետո վերադառնալով _Posts_ , դուք այլևս չեք տեսնի այն գրառումը որը որ դուք ընտրել էիք նախկինում։ Որովհետև ամեն անգամ երբ դուք փոխում եք դեպի նոր tab, Vue-ն ստեղծում է նոր `currentTabComponent`-ի instance։

Վերստեղծելով դինամիկ կոմպոնենտներ հիմնականում ճիշտ վարք է, բայց այս դեպքում, մենք պարզապես ուզում ենք որ այս tab-ի կոմպոնենտների instance-ները cache լինեն երբ որ նրանք ստեղծվում են առաջին անգամ։ Որպեսզի լուծենք այս հարցը, մենք պետք է փաթաթենք մեր դինամիկ կոմպոնենտը `<keep-alive>` էլեմենտով։

``` html
<!-- Ոչ ակտիվ կոմպոնտները cache կլինեն! -->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

Նայեք արդյունքը ներքևում։

{% raw %}
<div id="dynamic-component-keep-alive-demo" class="demo">
  <button
    v-for="tab in tabs"
    v-bind:key="tab"
    v-bind:class="['dynamic-component-demo-tab-button', { 'dynamic-component-demo-active': currentTab === tab }]"
    v-on:click="currentTab = tab"
  >{{ tab }}</button>
  <keep-alive>
    <component
      v-bind:is="currentTabComponent"
      class="dynamic-component-demo-tab"
    ></component>
  </keep-alive>
</div>
<script>
new Vue({
  el: '#dynamic-component-keep-alive-demo',
  data: {
    currentTab: 'Posts',
    tabs: ['Posts', 'Archive']
  },
  computed: {
    currentTabComponent: function () {
      return 'tab-' + this.currentTab.toLowerCase()
    }
  }
})
</script>
{% endraw %}

Հիմա _Գրառումներ_ tab-ը պահում է իր state-ը (ընտրված գրառումը) և նույնիսկ նրա render չեղած ժամանակ։ Նայեք [այս օրինակը](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-keep-alive-with-dynamic-components) ավելի ամբողջական կոդով։

<p class="tip">Նշում որ՝ `<keep-alive>`-ը պահանջում է որ կոմպոնենտները որոնք փոփոխվում են ունենան անուններ, կամ օգտագործելով `name` ընտրանքը կոմպոնենտում, կամ լոկալ/գլոբալ գրանցման միջոցով։</p>

Նայեք մանրամասները `<keep-alive>`-ի վերաբերյալ այստեղ [API reference](../api/#keep-alive)։

## Async Կոմպոնենտներ

<div class="vueschool"><a href="https://vueschool.io/lessons/dynamically-load-components?friend=vuejs" target="_blank" rel="sponsored noopener" title="Free Vue.js Async Components lesson">Դիտեք այս անվճար դասը Vue School-ում</a></div>

Մեծ ծրագրերում, մենք կարիք ունենք բաժանելու ծրագիրը փոքր մասերի և միայն բեռնել կոմպոնենտը սերվերից երբ անհրաժեշտ է։ Որպեսզի մենք այդ գործընթացը հեշտացնենք, Vue-ն առաջարկում է սահմանել ձեր կոմպոնենտները որպես factory ֆունկցիա որը ասինխրոն կերպով լուծում է ձեր կոմպոնենտի սահմանումը։ Vue-ն նաև կարձակի factory ֆունկցիան երբ կոմպոնենտը պետք է render լինի և cache կանի արդյունքը ապագա re-render-ների համար։ Օրինակ՝

``` js
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // Փոխանցեք կոմպոնենտի սահմանումը resolve callback-ում
    resolve({
      template: '<div>Ես async եմ!</div>'
    })
  }, 1000)
})
```

Ինչպես տեսնում եք, factory ֆունկցիան ստանում է `resolve` callback, որը պետք է աշխատի երբ որ դուք ստանում եք ձեր կոմպոնենտի սահմանումը սերվերից։ Դուք նաև կարող է կանչեք `reject(reason)`—ը որպեսզի նշեք, որ բեռնելը ձախողվել է։ `setTimeout`-ը այստեղ ուղղակի ցուցադրելու համար է; թե ինչպես ստանալ կոմպոնենտը դա դուք պետք է որոշեք։ Մեկ առաջարկվող մոտեցումը դա async կոմպոնենտների օգտագործումն է [Webpack's code-splitting feature-ի](https://webpack.js.org/guides/code-splitting/) հետ հանդերձ:

``` js
Vue.component('async-webpack-example', function (resolve) {
  // Սա կպահանջի հատուկ գրելաձև որը կհրահանգի Webpack-ին
  // ավտոմատ բաժանել ձեր build արած կոդը դեպի bundle
  // որը բեռնված է Ajax հարցումներով։
  require(['./my-async-component'], resolve)
})
```

Դուք նաև կարող եք վերադարձնել `Promise` factory ֆունկցիայում, այնպես որ, Webpack 2 և ES2015 գրելաձևի միջոցով դուք կարող եք անել հետևյալը։

``` js
Vue.component(
  'async-webpack-example',
  // դինամիկ իմպորտը վերադարձնում է Promise
  () => import('./my-async-component')
)
```

Երբ օգտագործում ենք [լոկալ գրանցում](components-registration.html#Local-Registration), դուք նաև կարող եք ուղիղ ձևով տրամադրել ֆունկցիա որը վերադարձնում է `Promise`։

``` js
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})
```

<p class="tip">Եթե դուք <strong>Browserify-ի</strong> օգտագործող եք ով ցանկություն ունի օգտագործելու async կոմպոնենտներ, նրա ստեղծողը ցավոք այդ [հարցին պատասխանել է](https://github.com/substack/node-browserify/issues/58#issuecomment-21978224) որ async բեռնումը «դա ինչ որ մի բան է որը Browserify-ը երբեք չի ունենա»։  Պաշտոնապես, գոնե, Browserify-ի համայնքը գտել է [որոշ շրջանցումներ](https://github.com/vuejs/vuejs.org/issues/620), որը կարող է օգտակար լինել գոյություն ունեցող բարդ ծրագրերի համար։ Մյուս դեպքերում, մենք խորհուրդ ենք տալիս օգտագործելու Webpack որը ունի, first-class async համապատասխանություն։

### Բեռնման Վիճակի Կառավարում

> Նոր 2.3.0+-ի մեջ

Async կոմպոնենտի factory-ին կարող է նաև վերադարձնել օբյեկտ հետևյալ ձևով։

``` js
const AsyncComponent = () => ({
  // Այն կոմպոնենտը որը ցանկանում ենք բեռնել (պետք է լինի Promise)
  component: import('./MyComponent.vue'),
  // Այն կոմպոնենտը որը ցանկանում ենք օգտագործել երբ async կոմպոնենտը բեռնվում է
  loading: LoadingComponent,
  // Այն կոմպոնենտը որը ցանկանում ենք օգտագործել եթե բեռնումը ձախողվում է
  error: ErrorComponent,
  // Հետաձգումը նախքան բեռնված կոմպոնենտի ցուցադրումը։ Հիմնական արժեք: 200ms։
  delay: 200,
  // Սխալ կոմպոնենտը կցուցադրվի եթե timeout-ը
  // տրամադրված է և երկարացված: Հիմնական արժեք։ Infinity (անվերջ)
  timeout: 3000
})
```

> Նշում որ դուք պետք է օգտագործեք [Vue Router-ը](https://github.com/vuejs/vue-router) 2.4.0+ եթե ցանկանում եք օգտագործել վերևում նշված գրելաձևը route կոմպոնենտների համար։
