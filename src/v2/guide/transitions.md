---
title: Մուտք/Ելք և Ցանկի Անցումներ
type: ուղեցույց
order: 201
---

## Ակնարկ

Vue-ն տրամադրում է բազմաթիվ ուղիներ որպեսզի կիրառել անցումային էֆեկտներ երբ մասնիկներ են տեղադրված, թարմացված, կամ ջնջված DOM-ից։ Սա ներառում է գործիքներ որպեսզի․

- ավտոմատ կերպով կիրառել class-ներ CSS անցումների և անիմացիաների համար
- ինտեգրացնել 3-րդ կողմի CSS անիմացիայի գրադարաններ, ինչպիսին Animate.css-ն է
- օգտագործելլ JavaScript որպեսզի ուղիղ կերպով մանիպուլացիայի ենթարկել DOM-ը անցումային hook-երի ժամանակ
- ինտեգրացնել 3-րդ կողմի JavaScript անիմացիայի գրադարաններ, ինչպիսին Velocity.js-ն է

Այս էջում, մենք միայն կնշենք մուտքը, ելքը, և ցանկի անցումները, բայց դուք կարող եք նայել հաջորդ բաաժինը [անցումների վիճակի կառավարման](transitioning-state.html) վերաբերյալ։

## Մեկական Էլեմենտների/Կոմպոնենտների Անցումները

Vue-ն տրամադրում է `transition` փաթաթվող կոմպոնենտ, թույլ տալով ձեզ ավելացնելու մուտքի/ելքի անցումներ ցանկացած էլեմենտի կամ կոմպոնենտի համար հետևյալ մեջբերումներում․

- Պայմանական rendering (օգտագործելով `v-if`)
- Պայմանական ցուցադրում (օգտագործելով `v-show`)
- Դինամիկ կոմպոնենտներ
- Կոմպոնենտի արմատային node-եր

Հետևյալը օրինակն է թե ինչպես այն կլինի իրականում․

``` html
<div id="demo">
  <button v-on:click="show = !show">
    Փոխարկել
  </button>
  <transition name="fade">
    <p v-if="show">բարև</p>
  </transition>
</div>
```

``` js
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})
```

``` css
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
```

{% raw %}
<div id="demo">
  <button v-on:click="show = !show">
    Փոխարկել
  </button>
  <transition name="demo-transition">
    <p v-if="show">բարև</p>
  </transition>
</div>
<script>
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})
</script>
<style>
.demo-transition-enter-active, .demo-transition-leave-active {
  transition: opacity .5s
}
.demo-transition-enter, .demo-transition-leave-to {
  opacity: 0
}
</style>
{% endraw %}

Երբ էլեմենտը փաթաթված է `transition` կոմպոնենտում որը տեղադրված կամ ջնջված է, ահա թե ինչ է կատարվում․

1. Vue-ն ավտոմատ կերպով կնկատի եթե նշված էլեմենտը ունի CSS անցումներ կամ անիմացիաներ կիրառված։ Եթե կիրառված են նրանք, CSS անցումների class-ները կլինեն ավելացված/ջնջված ճիշտ ժամանակներում։

2. Եթե անցումային կոմպոնենտը տրամադրել է [JavaScript hook-եր](#JavaScript-Hooks), այդ hook-երը կկանչվեն ճիշտ ժամանակներում։

3. Եթե ոչ CSS անցումներ/անիմացիաներ են նկատվել և ոչ ել JavaScript hook-եր են տրամադրված, DOM-ի գործողությունները տեղադրման կամ/և ջնջվելու համար կկատարվեն անմիջապես հաջորդ շրջանակում (Նշում․ սա բրաուզերների անիմացիայի շրջանակ է, որը տարբերվում է Vue-ի `nextTick`-ի նկարագրումից)

### Անցումային Class-ներ

Կան վեց class-ներ որոնք կիրառրվում են մուտքի/ելքի անցումներին։

1. `v-enter`․ Սկզբնական վիճակը մուտքի համար։ Ավելանում է երբ էլեմենտը տեղադրված է, ջնջվում է մեկ շրջանակ հետո երբ էլէմենտը տեղադրված է։

2. `v-enter-active`․ Ակտիվ վիճակը մուտքի համար։ Կիրառված է մուտքի ամբողջ ընթացքում։ Ավելացվում է նախքան էլեմենտը տեղադրված է, ջնջվում է երբ անցումը/անիմացիան ավարտվում է։ Այս class-ը կարելի է օգտագործել որպեսզի նկարագրել տևողությունը, կանխումը և տևողության կորությունը մուտքի անցման համար։

3. `v-enter-to`․ **Միայն հասանելի է 2.1.8+ տարբերակներում։** Վերջնական վիճակը մուտքի համար։ Ավելացվում է մեկ շրջանակ հետո երբ էլեմենտը տեղադրված է (միաժամանակ `v-enter`-ը ջնջվում է), ջնջվում է երբ անցումը/անիմացիան վերջանում է։

4. `v-leave`․ Սկզբնական վիճակը ելքի համար։ Ավելացվում է անմիջապես երբ ելքի անցումը արձակված է, ջնջվում է մեկ շրջանակ հետո։

5. `v-leave-active`․ Ակտիվ վիճակը ելքի համար։ Կիրառվում է ելքի ամբողջ ընթացքում։ Ավելացվում է անմիջապես երբ ելքի անցումը արձակված է, ջնջվում է երբ անցումը/անիմացիան ավարտվում է։ Այս class-ը կարելի է օգտագործել որպեսզի նկարագրել տևողությունը, կանխումը և տևողության կորությունը ելքի անցման համար։

6. `v-leave-to`: **Միայն հասանելի է 2.1.8+ տարբերակներում։** Վերջնական վիճակը ելքի համար։ Ավելացվում է մեկ շրջանակ հետո երբ ելքի անցումը արձակված է (միաժամանակ `v-leave`-ը ջնջվում է), ջնջվում է երբ անցումը/անիմացիան ավարտվում է։

![Անցումի Դիագրամ](/images/transition.png)

Այս class-ներից ամեն մեկը կնախածանցվի անցումի անունով։ Այստեղ `v-` նախածանցը հիմնականն է երբ դուք օգտագործում եք `<transition>` էլեմենտը առանց անունի։ Եթե դուք օգտագործեք `<transition name="my-transition">` օրինակի համար, այնուհետև `v-enter` class-ը փոխարենը կլինի `my-transition-enter`։

`v-enter-active` և `v-leave-active` կտա ձեզ հնարավորություն մատնանշելու տարբեր ժամանակի կորություններ մուտքի/ելքի անցումների համար, որոնք դուք կտեսնեք օրինակում հետևյալ բաժնի։

### CSS Անցումներ

Ամենատարածված անցումային տիպերից մեկը օգտագործում է CSS անցումներ։ Այստեղ է օրինակը․

``` html
<div id="example-1">
  <button @click="show = !show">
    Փոխարկել render
  </button>
  <transition name="slide-fade">
    <p v-if="show">բարև</p>
  </transition>
</div>
```

``` js
new Vue({
  el: '#example-1',
  data: {
    show: true
  }
})
```

``` css
/* Մուտքի և ելքի անիմացիաները կարող են օգտագործել տարբեր */
/* տևողություններ և ժամանակի ֆունկցիաներ։              */
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active 2.1.8 տարբերակից ներքև */ {
  transform: translateX(10px);
  opacity: 0;
}
```

{% raw %}
<div id="example-1" class="demo">
  <button @click="show = !show">
    Փոխարկել render
  </button>
  <transition name="slide-fade">
    <p v-if="show">բարև</p>
  </transition>
</div>
<script>
new Vue({
  el: '#example-1',
  data: {
    show: true
  }
})
</script>
<style>
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to {
  transform: translateX(10px);
  opacity: 0;
}
</style>
{% endraw %}

### CSS Անիմացիաներ

CSS անիմացիաները կիրառվում են նույն ձև ինչպես CSS անցումները, տարբերությունը կայանում է որ `v-enter`-ը չի ջնջվում անմիջապես երբ էլեմենտը տեղադրվում է, բայց `animationend` event-ում։

Այստեղ օրինակն է, բաց թողնելով նախածանցային CSS կանոնները հակիրճության համար։

``` html
<div id="example-2">
  <button @click="show = !show">Փոխարկել show-ը</button>
  <transition name="bounce">
    <p v-if="show">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris facilisis enim libero, at lacinia diam fermentum id. Pellentesque habitant morbi tristique senectus et netus.</p>
  </transition>
</div>
```

``` js
new Vue({
  el: '#example-2',
  data: {
    show: true
  }
})
```

``` css
.bounce-enter-active {
  animation: bounce-in .5s;
}
.bounce-leave-active {
  animation: bounce-in .5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
```

{% raw %}
<div id="example-2" class="demo">
  <button @click="show = !show">Փոխարկել show-ը</button>
  <transition name="bounce">
    <p v-show="show">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris facilisis enim libero, at lacinia diam fermentum id. Pellentesque habitant morbi tristique senectus et netus.</p>
  </transition>
</div>

<style>
  .bounce-enter-active {
    -webkit-animation: bounce-in .5s;
    animation: bounce-in .5s;
  }
  .bounce-leave-active {
    -webkit-animation: bounce-in .5s reverse;
    animation: bounce-in .5s reverse;
  }
  @keyframes bounce-in {
    0% {
      -webkit-transform: scale(0);
      transform: scale(0);
    }
    50% {
      -webkit-transform: scale(1.5);
      transform: scale(1.5);
    }
    100% {
      -webkit-transform: scale(1);
      transform: scale(1);
    }
  }
  @-webkit-keyframes bounce-in {
    0% {
      -webkit-transform: scale(0);
      transform: scale(0);
    }
    50% {
      -webkit-transform: scale(1.5);
      transform: scale(1.5);
    }
    100% {
      -webkit-transform: scale(1);
      transform: scale(1);
    }
  }
</style>
<script>
new Vue({
  el: '#example-2',
  data: {
    show: true
  }
})
</script>
{% endraw %}

### Custom Անցումային Class-ներ

Դուք կարող եք նաև մատնանշել custom անցումային class-ներ տրամադրելով հետևյալ ատրիբուտները․

- `enter-class`
- `enter-active-class`
- `enter-to-class` (2.1.8+)
- `leave-class`
- `leave-active-class`
- `leave-to-class` (2.1.8+)

Նրանք կվերագրեն հիմնական class անունները։ Սա հատկապես օգտակար է երբ դուք ցանկանում եք ձուլել Vue-ի անցումային համակարգը արդեն գոյություն ունեցող CSS անիմացիայի գրադարանի հետ, ինչպիսին [Animate.css](https://daneden.github.io/animate.css/) է։

Այստեղ է օրինակը․

``` html
<link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">

<div id="example-3">
  <button @click="show = !show">
    Փոխարկել render-ը
  </button>
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
  >
    <p v-if="show">բարև</p>
  </transition>
</div>
```

``` js
new Vue({
  el: '#example-3',
  data: {
    show: true
  }
})
```

{% raw %}
<link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">
<div id="example-3" class="demo">
  <button @click="show = !show">
    Փոխարկել render-ը
  </button>
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
  >
    <p v-if="show">բարև</p>
  </transition>
</div>
<script>
new Vue({
  el: '#example-3',
  data: {
    show: true
  }
})
</script>
{% endraw %}

### Անցումների և Անիմացիաների Օգտագործումը Միասին

Vue-ն պետք է միացնի event listener-ներ որպեսզի հասկանա թե երբ է անցումը ավարտվել։ Այն կարող է լինել `transitionend` կամ `animationend`, կախված լինելով կիրառված CSS կանոնի տիպից։ Եթե դուք միայն օգտագործում եք մեկը կամ մյուսը, Vue կարող է ավտոմատ կերպով նկատել ճիշտ տիպը։

Սակայն, որոշ դեպքերում դուք հնարավոր է որ կցանկանաք ունենալ երկուսն էլ մեկ էլեմենտի վրա, օրինակի համար ունենալով CSS անիմացիա որը բաց է թողնվում Vue-ի կողմից, CSS անցումային էֆեկտի հետ հանդերձ hover-ի ժամանակ։ Այս դեպքերում, դուք պետք է պարտադիր հայտարարեք տիպը որը որ դուք ցանկանում եք որ Vue-ն ունենա `type` ատրիբուտում, արժեքը լինելով `animation` կամ `transition`։

### Հատուկ Անցումային Տևողություններ

> Նոր 2.2.0+-ի մեջ

Շատ դեպքերում, Vue-ն կարող է ավտոմատ կերպով հասկանալ թե երբ է անցումը ավարտվել։ Հիմնականում, Vue-ն սպասում է առաջին `transitionend` կամ `animationend` event-ին արմատային անցումային էլեմենտում։ Սակայն, սա կարող է միշտ չլինել ցանկալի - օրինակի համար, մենք հնարավոր է որ կունենանք ժամանակագրված անցումային հաջորդականություն որտեղ որոշ ներդրված էլեմենտներ ունեն հետաձգված անցում կամ երկար անցման տևողություն քան արմատային անցումային էլեմենտը։

Այս դեպքերում դուք կարող եք հայտարարել անցման տևողությունը (միլիվարկյաններում) օգտագործելով `duration` հատկությունը `<transition>` կոմպոնենտում․

``` html
<transition :duration="1000">...</transition>
```

Դուք կարող եք նաև նկարագրել առանձին արժեքներ մուտքի և ելքի տևողությունների համար․

``` html
<transition :duration="{ enter: 500, leave: 800 }">...</transition>
```

### JavaScript-ի Hook-եր

Դուք կարող եք նաև հայտարարել JavaScript hook-եր ատրիբուտներում․

``` html
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>
```

``` js
// ...
methods: {
  // --------
  // ՄՈՒՏՔ
  // --------

  beforeEnter: function (el) {
    // ...
  },
  // done callback-ը պարտադիր չէ երբ
  // օգտագործում ենք CSS-ի հետ հանդերձ
  enter: function (el, done) {
    // ...
    done()
  },
  afterEnter: function (el) {
    // ...
  },
  enterCancelled: function (el) {
    // ...
  },

  // --------
  // ԵԼՔ
  // --------

  beforeLeave: function (el) {
    // ...
  },
  // done callback-ը պարտադիր չէ երբ
  // օգտագործում ենք CSS-ի հետ հանդերձ
  leave: function (el, done) {
    // ...
    done()
  },
  afterLeave: function (el) {
    // ...
  },
  // leaveCancelled-ը միայն հասանելի է v-show-ի հետ
  leaveCancelled: function (el) {
    // ...
  }
}
```

Այս hook-երը կարող են օգտագործվել CSS անցումների/անիմացիաների հետ հանդերձ կամ առանձին։

<p class="tip">Երբ օգտագործում ենք միայն JavaScript-ի անցումները, **`done` callback-ները միայն պահանջվում են `enter` և `leave` hook-երի համար**։ Հակառակ դեպքում, hook-երը կկանչվեն սինխռոն կերպով և անցումը միանգամից չի ավարտվի։</p>

<p class="tip">Նաև լավ միտք է ավելացնել `v-bind:css="false"` JavaScript-ի անցումների համար որ Vue-ն բաց թողնի CSS-ի նկատումը։ Սա նաև խուսափում է CSS-ի կանոններից որոնք միամիտ կխանգարեն անցումը։</p>

Հիմա եկեք սուզվենք դեպի օրինակը։ Այստեղ է JavaScript անցում օգտագործելով Velocity.js-ը․

``` html
<!--
Velocity-ն աշխատում է շատ նման լինելով jQuery.animate-ին և այն
շատ լավ ընտրանք է JavaScript անիմացիաների համար
-->
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>

<div id="example-4">
  <button @click="show = !show">
    Փոխարկել
  </button>
  <transition
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
    v-bind:css="false"
  >
    <p v-if="show">
      Դեմո
    </p>
  </transition>
</div>
```

``` js
new Vue({
  el: '#example-4',
  data: {
    show: false
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
      el.style.transformOrigin = 'left'
    },
    enter: function (el, done) {
      Velocity(el, { opacity: 1, fontSize: '1.4em' }, { duration: 300 })
      Velocity(el, { fontSize: '1em' }, { complete: done })
    },
    leave: function (el, done) {
      Velocity(el, { translateX: '15px', rotateZ: '50deg' }, { duration: 600 })
      Velocity(el, { rotateZ: '100deg' }, { loop: 2 })
      Velocity(el, {
        rotateZ: '45deg',
        translateY: '30px',
        translateX: '30px',
        opacity: 0
      }, { complete: done })
    }
  }
})
```

{% raw %}
<div id="example-4" class="demo">
  <button @click="show = !show">
    Փոխարկել
  </button>
  <transition
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <p v-if="show">
      Դեմո
    </p>
  </transition>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>
<script>
new Vue({
  el: '#example-4',
  data: {
    show: false
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
      el.style.transformOrigin = 'left'
    },
    enter: function (el, done) {
      Velocity(el, { opacity: 1, fontSize: '1.4em' }, { duration: 300 })
      Velocity(el, { fontSize: '1em' }, { complete: done })
    },
    leave: function (el, done) {
      Velocity(el, { translateX: '15px', rotateZ: '50deg' }, { duration: 600 })
      Velocity(el, { rotateZ: '100deg' }, { loop: 2 })
      Velocity(el, {
        rotateZ: '45deg',
        translateY: '30px',
        translateX: '30px',
        opacity: 0
      }, { complete: done })
    }
  }
})
</script>
{% endraw %}

## Անցումները Սկզբնական Render-ում

Եթե դուք նաև ցանկանում եք կիրառել անցումներ սկզբնական node-ի render—ում, դուք կարող եք ավելանցել `appear` ատրիբուտը․

``` html
<transition appear>
  <!-- ... -->
</transition>
```

ՀԻմնականում, սա կօգտագործի անցումները նշված մուտքի և ելքի համար։ եթե դուք ցանկանում եք, դուք կարող եք նշել custom CSS class-ներ․

``` html
<transition
  appear
  appear-class="custom-appear-class"
  appear-to-class="custom-appear-to-class" (2.1.8+)
  appear-active-class="custom-appear-active-class"
>
  <!-- ... -->
</transition>
```

և custom JavaScript hook-եր․

``` html
<transition
  appear
  v-on:before-appear="customBeforeAppearHook"
  v-on:appear="customAppearHook"
  v-on:after-appear="customAfterAppearHook"
  v-on:appear-cancelled="customAppearCancelledHook"
>
  <!-- ... -->
</transition>
```

Վերևում նշված օրինակում, կամ `appear` ատրիբուտը կամ ել `v-on:appear` hook կգործարկեն appear անցումը։

## Անցումը Էլեմենտների Միջև

Մենք քննարկում ենք [անցումը կոմպոնենտների միջև](#Transitioning-Between-Components) հետո, բայց դուք կարող եք նաև անցում կատարել անմշակ էլեմենտների միջև օգտագործելով `v-if`/`v-else`։ Շատ հաճախ օգտագործվող երկու էլեմենտ անցումներից մեկը դա ցանկերի խմբի և նամակ նկարագրող դատարկ ցանկ է․

``` html
<transition>
  <table v-if="items.length > 0">
    <!-- ... -->
  </table>
  <p v-else>Ներեցեք, ոչ մի ապրանք չի գտնվել։</p>
</transition>
```

Սա աշխատում է լավ, բայց կա մեկ նախազգուշացում որ պետք է դուք տեղյակ լինել․

<p class="tip">Երբ փոխարկում ենք էլեմենտների միջև որոնք ունեն նույն **tag-ի անունը**, դուք պետք է տեղյակ պահեք Vue-ին որ նրանք տարբեր էլեմենտներ են տրամադրելով նրանց հատուկ `key` ատրիբուտներ։ Հակառակ դեպքում, Vue-ի compiler-ը միայն կփոխանակի բովանդակությունը էլեմենտի արդյունավետության համար։ Նույնիսկ երբ տեխնիկապես անիմաստ է, **շատ ճիշտ է միշտ ունենալ մի քանի մասնիկեր `<transition>` կոմպոնենտում։**</p>

Օրինակի համար․

``` html
<transition>
  <button v-if="isEditing" key="save">
    Save
  </button>
  <button v-else key="edit">
    Edit
  </button>
</transition>
```

Այս դեպքերում, դուք կարող եք նաև օգտագործել `key` ատրիբուտը որպեսզի անցում կատարել նույն էլեմենտի տարբեր վիճակներից։ `v-if` և `v-else`-ի փոխարեն, վերևում օրինակը կարող է վերագրվել ինչպես․

``` html
<transition>
  <button v-bind:key="isEditing">
    {{ isEditing ? 'Save' : 'Edit' }}
  </button>
</transition>
```

Իրականում հնարավոր է անցում կատարել ցանկացած էլեմենտների քանակի միջև, կամ օգտագործելով բազմաթիվ `v-if`-ներ կամ կապելով մեկական էլեմենտը դինամիկ հատկության։ Օրինակի համար․

``` html
<transition>
  <button v-if="docState === 'saved'" key="saved">
    Խմբագրել
  </button>
  <button v-if="docState === 'edited'" key="edited">
    Հիշել
  </button>
  <button v-if="docState === 'editing'" key="editing">
    Չեղարկել
  </button>
</transition>
```

Որը կարող է գրվել որպես․

``` html
<transition>
  <button v-bind:key="docState">
    {{ buttonMessage }}
  </button>
</transition>
```

``` js
// ...
computed: {
  buttonMessage: function () {
    switch (this.docState) {
      case 'saved': return 'Խմբագրել'
      case 'edited': return 'Հիշել'
      case 'editing': return 'Չեղարկել'
    }
  }
}
```

### Անցումների Ռեժիմները

Կա դեռ մեկ խնդիր։ Փորձեք սեղմել ստորև կոճակի վրա․

{% raw %}
<div id="no-mode-demo" class="demo">
  <transition name="no-mode-fade">
    <button v-if="on" key="on" @click="on = false">
      միացված
    </button>
    <button v-else key="off" @click="on = true">
      անջատված
    </button>
  </transition>
</div>
<script>
new Vue({
  el: '#no-mode-demo',
  data: {
    on: false
  }
})
</script>
<style>
.no-mode-fade-enter-active, .no-mode-fade-leave-active {
  transition: opacity .5s
}
.no-mode-fade-enter, .no-mode-fade-leave-active {
  opacity: 0
}
</style>
{% endraw %}

Մինչ նա անցում է կատարում «միացված» և «անջատված» կոճակների միջև, երկու կոճակներնել rendered են եղած - մեկը անցում է կատարում դեպի դուրս միչև մյուսը ներս։ Սա հիմնական վարքն է `<transition>`-ի - մուտքը և ելքը կատարվում է միաժամանակ։

Որոշ դեպքերում սա լավ է աշխատում, ինչպես անցումային մասնիկներն են absolute տեղադրված մեկը մյուսի վրա։

{% raw %}
<div id="no-mode-absolute-demo" class="demo">
  <div class="no-mode-absolute-demo-wrapper">
    <transition name="no-mode-absolute-fade">
      <button v-if="on" key="on" @click="on = false">
        միացված
      </button>
      <button v-else key="off" @click="on = true">
        անջատված
      </button>
    </transition>
  </div>
</div>
<script>
new Vue({
  el: '#no-mode-absolute-demo',
  data: {
    on: false
  }
})
</script>
<style>
.no-mode-absolute-demo-wrapper {
  position: relative;
  height: 18px;
}
.no-mode-absolute-demo-wrapper button {
  position: absolute;
}
.no-mode-absolute-fade-enter-active, .no-mode-absolute-fade-leave-active {
  transition: opacity .5s;
}
.no-mode-absolute-fade-enter, .no-mode-absolute-fade-leave-active {
  opacity: 0;
}
</style>
{% endraw %}

ԵՎ հետո երևի նաև translate արած որ նրանք նման լինեն slide անցումների․

{% raw %}
<div id="no-mode-translate-demo" class="demo">
  <div class="no-mode-translate-demo-wrapper">
    <transition name="no-mode-translate-fade">
      <button v-if="on" key="on" @click="on = false">
        միացված
      </button>
      <button v-else key="off" @click="on = true">
        անջատված
      </button>
    </transition>
  </div>
</div>
<script>
new Vue({
  el: '#no-mode-translate-demo',
  data: {
    on: false
  }
})
</script>
<style>
.no-mode-translate-demo-wrapper {
  position: relative;
  height: 18px;
}
.no-mode-translate-demo-wrapper button {
  position: absolute;
}
.no-mode-translate-fade-enter-active, .no-mode-translate-fade-leave-active {
  transition: all 1s;
}
.no-mode-translate-fade-enter, .no-mode-translate-fade-leave-active {
  opacity: 0;
}
.no-mode-translate-fade-enter {
  transform: translateX(31px);
}
.no-mode-translate-fade-leave-active {
  transform: translateX(-31px);
}
</style>
{% endraw %}

Միաժամանակ մուտքի և ելքի անցումները միշտ ցանկալի չեն, այդ դեպքում Vue-ն տրամադրում է այլընտրանքային **անցումային ռեժիմներ**․

- `in-out`․ նոր էլեմենտի անցումները առաջինը, հետո երբ ավարտվում է, ընթացիկ էլեմենտի անցումները դեպի դուրս։

- `out-in`․ Ընթացիկ էլեմենտի անցումները դուրս առաջինը, հետո երբ ավարտվում է, նոր էլեմենտի անցումները դեպի ներս։

Հիմա եկեք թարմացնենք անցումները մեր միացնել/անջատել կոճակների համար `out-in`-ի հետ հանդերձ․

``` html
<transition name="fade" mode="out-in">
  <!-- ... կոճակները ... -->
</transition>
```

{% raw %}
<div id="with-mode-demo" class="demo">
  <transition name="with-mode-fade" mode="out-in">
    <button v-if="on" key="on" @click="on = false">
      միացնել
    </button>
    <button v-else key="off" @click="on = true">
      անջատել
    </button>
  </transition>
</div>
<script>
new Vue({
  el: '#with-mode-demo',
  data: {
    on: false
  }
})
</script>
<style>
.with-mode-fade-enter-active, .with-mode-fade-leave-active {
  transition: opacity .5s
}
.with-mode-fade-enter, .with-mode-fade-leave-active {
  opacity: 0
}
</style>
{% endraw %}

Մեկ ատրիբուտի հետ հանդերձ, մենք ուղղել ենք հիմնական անցումը առանց ավելացնելու հատուկ ոճեր։

`in-out` ռեժիմը չի օգտագործվում հաճախ, բայց կարող եք որոշ դեպքերում օգտակար լինել որոշակի տարբեր անցումային էֆեկտի համար։ Եկեք փորձենք միացնել այն slide-fade անցման հետ որի վրա մենք աշխատել էինք ավելի վաղ․

{% raw %}
<div id="in-out-translate-demo" class="demo">
  <div class="in-out-translate-demo-wrapper">
    <transition name="in-out-translate-fade" mode="in-out">
      <button v-if="on" key="on" @click="on = false">
        միացնել
      </button>
      <button v-else key="off" @click="on = true">
        անջատել
      </button>
    </transition>
  </div>
</div>
<script>
new Vue({
  el: '#in-out-translate-demo',
  data: {
    on: false
  }
})
</script>
<style>
.in-out-translate-demo-wrapper {
  position: relative;
  height: 18px;
}
.in-out-translate-demo-wrapper button {
  position: absolute;
}
.in-out-translate-fade-enter-active, .in-out-translate-fade-leave-active {
  transition: all .5s;
}
.in-out-translate-fade-enter, .in-out-translate-fade-leave-active {
  opacity: 0;
}
.in-out-translate-fade-enter {
  transform: translateX(31px);
}
.in-out-translate-fade-leave-active {
  transform: translateX(-31px);
}
</style>
{% endraw %}

Լավն է, չ՞ե

## Անցումը Կոմպոնենտների Միջև

Անցումը կոմպոնենտների միջև ավելի հեշտ է - մեզանից նույնիսկ հարկավոր չե ավելացնել `key` ատրիբուտը։ Փոխարենը, մենք փաթաթում ենք [դինամիկ կոմպոնենտ](components.html#Dynamic-Components)․

``` html
<transition name="component-fade" mode="out-in">
  <component v-bind:is="view"></component>
</transition>
```

``` js
new Vue({
  el: '#transition-components-demo',
  data: {
    view: 'v-a'
  },
  components: {
    'v-a': {
      template: '<div>Կոմպոնենտ Ա</div>'
    },
    'v-b': {
      template: '<div>Կոմպոնենտ Բ</div>'
    }
  }
})
```

``` css
.component-fade-enter-active, .component-fade-leave-active {
  transition: opacity .3s ease;
}
.component-fade-enter, .component-fade-leave-to
/* .component-fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
```

{% raw %}
<div id="transition-components-demo" class="demo">
  <input v-model="view" type="radio" value="v-a" id="a" name="view"><label for="a">Ա</label>
  <input v-model="view" type="radio" value="v-b" id="b" name="view"><label for="b">Բ</label>
  <transition name="component-fade" mode="out-in">
    <component v-bind:is="view"></component>
  </transition>
</div>
<style>
.component-fade-enter-active, .component-fade-leave-active {
  transition: opacity .3s ease;
}
.component-fade-enter, .component-fade-leave-to {
  opacity: 0;
}
</style>
<script>
new Vue({
  el: '#transition-components-demo',
  data: {
    view: 'v-a'
  },
  components: {
    'v-a': {
      template: '<div>Կոմպոնենտ Ա</div>'
    },
    'v-b': {
      template: '<div>Կոմպոնենտ Բ</div>'
    }
  }
})
</script>
{% endraw %}

## Ցանկի Անցումները

Մինչ դեռ, մենք կառավարել ենք անցումները հետևյալների համար․

- Անհատական node-երի
- Բազմաթիվ node-երի որտեղ միայն 1-ն է render լինում միաժամանակ

Իսկ ի՞նչ եթե մենք ցանկանում ենք render անել ցանկ մասնիկների միաժամանակ, օրինակի համար `v-for`-ի հետ։ Այս դեպքում, մենք կօգտագործենք `<transition-group>` կոմպոնենտը։ Նախքան դեպի օրինակը սուզվելը, կան մի քանի բաներ որոնք կարևոր են իմանալ այս կոմպոնենտի մասին․

- Ի տարբերություն `<transition>`-ի, այն render է անում իրական էլեմենտ․ `<span>` հիմնականում։ Դուք կարող եք փոխել էլեմենտը որը render է եղած `tag` ատրիբուտով։
- [Անցումային ռեժիմները](#Transition-Modes) հասանելի չեն լինի, որովհետև նրանք այլևս չեն փոփոխի փոխադարձ բացառող էլեմենտների միջև։
- Էլեմենտները ներսում **միշտ պահանջվում են** որպեսզի ունենալ հատուկ `key` ատրիբուտ։
- CSS անցումային class-ները կկիրառվեն ներքին էլմենենտներին և ոչ խմբին/պարունակողին։

### Ցանկի Մուտքի/Ելքի Անցումները

Հիմա եկեք սուզվենք դեպի օրինակը, անցումների մուտքը և ելքը օգտագործելով նույն CSS class-ները մենք նախկինում օգտագործեցինք․

``` html
<div id="list-demo">
  <button v-on:click="add">Ավելացնել</button>
  <button v-on:click="remove">Ջնջել</button>
  <transition-group name="list" tag="p">
    <span v-for="item in items" v-bind:key="item" class="list-item">
      {{ item }}
    </span>
  </transition-group>
</div>
```

``` js
new Vue({
  el: '#list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
  }
})
```

``` css
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
```

{% raw %}
<div id="list-demo" class="demo">
  <button v-on:click="add">Ավելացնել</button>
  <button v-on:click="remove">Ջնջել</button>
  <transition-group name="list" tag="p">
    <span v-for="item in items" :key="item" class="list-item">
      {{ item }}
    </span>
  </transition-group>
</div>
<script>
new Vue({
  el: '#list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
  }
})
</script>
<style>
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style>
{% endraw %}

Կա մեկ խնդիր այս օրինակի հետ։ Եթե դուք ավելացնում կամ ջնջում եք մասնիկ, այդ մասնիկի հարևանները միանգամից կդիպչեն դեպի իրենց նոր տեղերը գեղեցիկ անցումի փոխարեն։ Մենք կուղղենք այն հետո։

### Ցանկի Շարժի Անցումները

`<transition-group>` կոմպոնենը ունի մեկ այլ առավելություն։ Այն ոչ միայն կարող է անիմացիայի ենթարկել մուտքը և ելքը, բայց նաև փոփոխությունները դիրքում։ Միակ նոր գաղափարը որը դուք պետք է իմանալ որպեսզի օգտագործել դա **`v-move` class-ի** հավելումն է, որը ավելացվում է երբ մասնիկները փոխում են դիրքերը։ Ինչպես նաև այլ class-ներ, իր նախածանցը կհամապատասխանի `name` ատրիբուտի արժեքին և դուք կարող եք ձեռքով նշել class-ը `move-class` ատրիբուտի շնորհիվ։

Այս class-ը հատկապես օգտակար է նշելու անցման ժամանակը և ժամանակի կորությունը, ինչպես կտեսնեք այն ներքևում․

``` html
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>

<div id="flip-list-demo" class="demo">
  <button v-on:click="shuffle">Խառնել</button>
  <transition-group name="flip-list" tag="ul">
    <li v-for="item in items" v-bind:key="item">
      {{ item }}
    </li>
  </transition-group>
</div>
```

``` js
new Vue({
  el: '#flip-list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9]
  },
  methods: {
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})
```

``` css
.flip-list-move {
  transition: transform 1s;
}
```

{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>
<div id="flip-list-demo" class="demo">
  <button v-on:click="shuffle">Shuffle</button>
  <transition-group name="flip-list" tag="ul">
    <li v-for="item in items" :key="item">
      {{ item }}
    </li>
  </transition-group>
</div>
<script>
new Vue({
  el: '#flip-list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9]
  },
  methods: {
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})
</script>
<style>
.flip-list-move {
  transition: transform 1s;
}
</style>
{% endraw %}

Սա կարող է թվալ կախարդական, բայց իրականում, Vue-ն օգտագործում է անիմացիոն տեխնիկա անվանված [FLIP](https://aerotwist.com/blog/flip-your-animations/) որպեսզի սահուն անցում կատարի էլեմենտների հին դիրքից դեպի նրանց նոր դիրք օգտագործելով transform-ներ։

Մենք կարող ենք միացնել այս տեխնիկան մեր նախկին տեղադրումների հետ որպեսզի անիմացիայի ենթարկել ամեն հասանելի փոփոխությունը մեր ցանկում։

``` html
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>

<div id="list-complete-demo" class="demo">
  <button v-on:click="shuffle">Խառնել</button>
  <button v-on:click="add">Ավելացնել</button>
  <button v-on:click="remove">Ջնջել</button>
  <transition-group name="list-complete" tag="p">
    <span
      v-for="item in items"
      v-bind:key="item"
      class="list-complete-item"
    >
      {{ item }}
    </span>
  </transition-group>
</div>
```

``` js
new Vue({
  el: '#list-complete-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})
```

``` css
.list-complete-item {
  transition: all 1s;
  display: inline-block;
  margin-right: 10px;
}
.list-complete-enter, .list-complete-leave-to
/* .list-complete-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
.list-complete-leave-active {
  position: absolute;
}
```

{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>
<div id="list-complete-demo" class="demo">
  <button v-on:click="shuffle">Խառնել</button>
  <button v-on:click="add">Ավելացնել</button>
  <button v-on:click="remove">Ջնջել</button>
  <transition-group name="list-complete" tag="p">
    <span v-for="item in items" :key="item" class="list-complete-item">
      {{ item }}
    </span>
  </transition-group>
</div>
<script>
new Vue({
  el: '#list-complete-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})
</script>
<style>
.list-complete-item {
  transition: all 1s;
  display: inline-block;
  margin-right: 10px;
}
.list-complete-enter, .list-complete-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.list-complete-leave-active {
  position: absolute;
}
</style>
{% endraw %}

<p class="tip">Մեկ կարևոր նշում որ այս FLIP անցումները չեն աշխատում էլեմենտների հետ որոնք ունեն `display: inline` հատկությունը։ Որպես այլընտրանք, դուք կարող եք օգտագործել `display: inline-block` կամ տեղադրել էլեմենտները flex մեջբերումում։</p>

Այս FLIP անիմացիաները նաև սահմանափակված չեն մեկ առանցքի շուրջ։ Մասնիկները բազմաչափ ցանցում կարող են նաև [անցում կատարել](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-list-move-transitions)։

{% raw %}
<div id="sudoku-demo" class="demo">
  <strong>Ծույլ Sudoku</strong>
  <p>Անընդհատ սեղմեք խառնել կոճակը մինչ կհաղթեք։</p>
  <button @click="shuffle">
    Խառնել
  </button>
  <transition-group name="cell" tag="div" class="sudoku-container">
    <div v-for="cell in cells" :key="cell.id" class="cell">
      {{ cell.number }}
    </div>
  </transition-group>
</div>
<script>
new Vue({
  el: '#sudoku-demo',
  data: {
    cells: Array.apply(null, { length: 81 })
      .map(function (_, index) {
        return {
          id: index,
          number: index % 9 + 1
        }
      })
  },
  methods: {
    shuffle: function () {
      this.cells = _.shuffle(this.cells)
    }
  }
})
</script>
<style>
.sudoku-container {
  display: flex;
  flex-wrap: wrap;
  width: 238px;
  margin-top: 10px;
}
.cell {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 25px;
  height: 25px;
  border: 1px solid #aaa;
  margin-right: -1px;
  margin-bottom: -1px;
}
.cell:nth-child(3n) {
  margin-right: 0;
}
.cell:nth-child(27n) {
  margin-bottom: 0;
}
.cell-move {
  transition: transform 1s;
}
</style>
{% endraw %}

### Ցնցող Ցուցակի Անցումներ

Կապ հաստատելով JavaScript անցումների հետ տվյալների ատրիբուտների շնորհիվ, նաև հնարավոր է ցնցել անցումները ցանկում․

``` html
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>

<div id="staggered-list-demo">
  <input v-model="query">
  <transition-group
    name="staggered-fade"
    tag="ul"
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <li
      v-for="(item, index) in computedList"
      v-bind:key="item.msg"
      v-bind:data-index="index"
    >{{ item.msg }}</li>
  </transition-group>
</div>
```

``` js
new Vue({
  el: '#staggered-list-demo',
  data: {
    query: '',
    list: [
      { msg: 'Bruce Lee' },
      { msg: 'Jackie Chan' },
      { msg: 'Chuck Norris' },
      { msg: 'Jet Li' },
      { msg: 'Kung Fury' }
    ]
  },
  computed: {
    computedList: function () {
      var vm = this
      return this.list.filter(function (item) {
        return item.msg.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1
      })
    }
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
      el.style.height = 0
    },
    enter: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 1, height: '1.6em' },
          { complete: done }
        )
      }, delay)
    },
    leave: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 0, height: 0 },
          { complete: done }
        )
      }, delay)
    }
  }
})
```

{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>
<div id="example-5" class="demo">
  <input v-model="query">
  <transition-group
    name="staggered-fade"
    tag="ul"
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <li
      v-for="(item, index) in computedList"
      v-bind:key="item.msg"
      v-bind:data-index="index"
    >{{ item.msg }}</li>
  </transition-group>
</div>
<script>
new Vue({
  el: '#example-5',
  data: {
    query: '',
    list: [
      { msg: 'Bruce Lee' },
      { msg: 'Jackie Chan' },
      { msg: 'Chuck Norris' },
      { msg: 'Jet Li' },
      { msg: 'Kung Fury' }
    ]
  },
  computed: {
    computedList: function () {
      var vm = this
      return this.list.filter(function (item) {
        return item.msg.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1
      })
    }
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
      el.style.height = 0
    },
    enter: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 1, height: '1.6em' },
          { complete: done }
        )
      }, delay)
    },
    leave: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 0, height: 0 },
          { complete: done }
        )
      }, delay)
    }
  }
})
</script>
{% endraw %}

## Վերօգտագործվող Անցումներ

Անցումները կարող են վերօգտագործվել Vue-ի կոմպոնենտի համակարգի շնորհիվ։ Որպեսզի ստեղծել վերօգտագործվեղ անցումներ, մեզ ընդհամենը պետք է տեղադրել `<transition>` կամ `<transition-group>` կոմպոնենտը արմատում, այնուհետև փոխանցել ցանկացած ժառանգողներ դեպի անցումային կոմպոնենտ։

Այստեղ է օրինակը օգտագործելով ձևանմուշ կոմպոնենտ․

``` js
Vue.component('my-special-transition', {
  template: '\
    <transition\
      name="very-special-transition"\
      mode="out-in"\
      v-on:before-enter="beforeEnter"\
      v-on:after-enter="afterEnter"\
    >\
      <slot></slot>\
    </transition>\
  ',
  methods: {
    beforeEnter: function (el) {
      // ...
    },
    afterEnter: function (el) {
      // ...
    }
  }
})
```

ԵՎ [ֆունկցիոնալ կոմպոնենտները](render-function.html#Functional-Components) հատկապես շատ հարմար են այս հանձնարարության համար․

``` js
Vue.component('my-special-transition', {
  functional: true,
  render: function (createElement, context) {
    var data = {
      props: {
        name: 'very-special-transition',
        mode: 'out-in'
      },
      on: {
        beforeEnter: function (el) {
          // ...
        },
        afterEnter: function (el) {
          // ...
        }
      }
    }
    return createElement('transition', data, context.children)
  }
})
```

## Դինամիկ Անցումներ

Այո, նույնիսկ անցումները Vue-ում տվյալներով հենված են՜ Ամենա հասարակ օրինակը դինամիկ անցումների դա `name` ատրիբուտի կապումն է դինամիկ հատկությանը։

```html
<transition v-bind:name="transitionName">
  <!-- ... -->
</transition>
```

Սա կարող է օգտակար լինել եթե դուք հայտարարել եք CSS անցումներ/անիմացիաներ օգտագործելով Vue-ի անցումների class-ի հարմարավետությունները և ցանկանում եք փոփոխել նրանց միջև։

Իրականում չնայած, ցանկացած անցման ատրիբուտ կարող է դինամիկորեն կապվել։ ԵՎ ոչ միայն ատրիբուտները։ Մինչ event hook-երը մեթոդներ են, նրանք կարող են մուտք գործել դեպի տվյալները նույն մեջբերման մեջ։ Սա նշանակում է կախված լինելով ձեր կոմպոնենտնի վիճակի վրա, ձեր JavaScript անցումները կարող են ունենալ այլ վարք։

``` html
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>

<div id="dynamic-fade-demo" class="demo">
  Fade In: <input type="range" v-model="fadeInDuration" min="0" v-bind:max="maxFadeDuration">
  Fade Out: <input type="range" v-model="fadeOutDuration" min="0" v-bind:max="maxFadeDuration">
  <transition
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <p v-if="show">Բարև</p>
  </transition>
  <button
    v-if="stop"
    v-on:click="stop = false; show = false"
  >Սկսել անիմացիան</button>
  <button
    v-else
    v-on:click="stop = true"
  >Ավարտել</button>
</div>
```

``` js
new Vue({
  el: '#dynamic-fade-demo',
  data: {
    show: true,
    fadeInDuration: 1000,
    fadeOutDuration: 1000,
    maxFadeDuration: 1500,
    stop: true
  },
  mounted: function () {
    this.show = false
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
    },
    enter: function (el, done) {
      var vm = this
      Velocity(el,
        { opacity: 1 },
        {
          duration: this.fadeInDuration,
          complete: function () {
            done()
            if (!vm.stop) vm.show = false
          }
        }
      )
    },
    leave: function (el, done) {
      var vm = this
      Velocity(el,
        { opacity: 0 },
        {
          duration: this.fadeOutDuration,
          complete: function () {
            done()
            vm.show = true
          }
        }
      )
    }
  }
})
```

{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>
<div id="dynamic-fade-demo" class="demo">
  Fade In: <input type="range" v-model="fadeInDuration" min="0" v-bind:max="maxFadeDuration">
  Fade Out: <input type="range" v-model="fadeOutDuration" min="0" v-bind:max="maxFadeDuration">
  <transition
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <p v-if="show">Բարև</p>
  </transition>
  <button
    v-if="stop"
    v-on:click="stop = false; show = false"
  >Սկսել անիմացիան</button>
  <button
    v-else
    v-on:click="stop = true"
  >Ավարտել</button>
</div>
<script>
new Vue({
  el: '#dynamic-fade-demo',
  data: {
    show: true,
    fadeInDuration: 1000,
    fadeOutDuration: 1000,
    maxFadeDuration: 1500,
    stop: true
  },
  mounted: function () {
    this.show = false
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
    },
    enter: function (el, done) {
      var vm = this
      Velocity(el,
        { opacity: 1 },
        {
          duration: this.fadeInDuration,
          complete: function () {
            done()
            if (!vm.stop) vm.show = false
          }
        }
      )
    },
    leave: function (el, done) {
      var vm = this
      Velocity(el,
        { opacity: 0 },
        {
          duration: this.fadeOutDuration,
          complete: function () {
            done()
            vm.show = true
          }
        }
      )
    }
  }
})
</script>
{% endraw %}

Վերջապես, ճիշտ ուղին ստեղծելու դինամիկ անցումները դա կոմպոնենտների շնորհիվ է որոնք ընդհունում են prop-ներ որպեսզի փոփոխել անցումների օգտագործման հիմնական ձևը։ Սա կարող է թվալ բարդ, բայց այն սահմանափակված է միայն պատկերացուման կողմից։

