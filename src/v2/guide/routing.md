---
title: Routing
type: ուղեցույց
order: 501
---

## Պաշտոնական Router

Շատ Մեկ Էջ Ծրագրերում, խորհուրդ է տրվում օգտագործել պաշտոնապես համապատասխանող [vue-router գրադարանը](https://github.com/vuejs/vue-router)։ Ավելի մանրամասների համար, նայեք vue-router-ի [փաստաթղթերը](https://router.vuejs.org/)։

## Հասարակ Routing Զրոյից

Եթե դուք պետք է օգտագործեք շատ հասարակ routing և չեք ցանկանում օգտագործել մեծածավալ router-ի գրադարան, դուք կարող եք հասնել դրան դինամիկորեն render անելով էջի կարգի կոմպոնենտը հետևյալ կերպով․

``` js
const NotFound = { template: '<p>Էջ չի գտնվել</p>' }
const Home = { template: '<p>Հիմնական էջ</p>' }
const About = { template: '<p>Մեր մասին</p>' }

const routes = {
  '/': Home,
  '/about': About
}

new Vue({
  el: '#app',
  data: {
    currentRoute: window.location.pathname
  },
  computed: {
    ViewComponent () {
      return routes[this.currentRoute] || NotFound
    }
  },
  render (h) { return h(this.ViewComponent) }
})
```

Միակցված HTML5 History API-ի հետ, դուք կարող էք կառուցել շատ հասարակ բայց լիովին ֆունկցիոնալ օգտագործողի-կողմի router։ Որպեսզի այն տեսնել կիրառման ժամանակ, նայեք այս [ծրագրի օրինակը](https://github.com/chrisvfritz/vue-2.0-simple-routing-example)։

## 3-րդ Կողմի Router-ների Տեղադրումը

Եթե ցանկանում եք օգտագործում եք 3-րդ կողմի router, ինչպիսին է [Page.js-ը](https://github.com/visionmedia/page.js) կամ [Director-ը](https://github.com/flatiron/director), տեղադրումը [բավականին պարզ է](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/compare/master...pagejs)։ Այստեղ կարող եք նայել [ամբողջական օրինակը](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/tree/pagejs) օգտագործելով Page.js-ը։
