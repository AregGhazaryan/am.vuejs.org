---
title: Class-ների և Ոճերի Կապը
type: ուղեցույց
order: 6
---

Հասարակ անհրաժեշտություն է տվյալների միացման համար մանիպուլացիայի ենթարկել էլեմենտի class-ները և նրա inline ոճերը։ Քանի որ երկուսնել ատրիբուտ են, մենք կարող ենք օգտագործել `v-bind`-ը որպեսզի կառավարենք նրանց։ Մենք միայն պետք է հաշվարկենք վերջնական string մեր արտահայտության հետ։ Սակայն, խառնվելը string-ի միացումներին նյարդայնացնող է և սխալներ կարող է առաջացնել։ Այդ պատճառով, Vue-ն տրամադրում է հատուկ բարելավումներ երբ `v-bind`-ն է օգտագործվում `class` և `style`-ի հետ: String-ներին ավելացված, արտահայտությունները կարող են արժեքավորվել օբյեկտներում կամ զանգվածներում։

## HTML Class-ների Կապը
<div class="vueschool"><a href="https://vueschool.io/lessons/vuejs-dynamic-classes?friend=vuejs" target="_blank" rel="sponsored noopener" title="Free Vue.js Dynamic Classes Lesson">Դիտեք անվճար վիդեո դաս Vue School-ում</a></div>

### Օբյեկտի Գրելաձև

Մենք կարող ենք փոխանցել օբյեկտ `v-bind:class`-ին որպեսզի դինամիկորեն միացնենք կամ անջատենք class—ները։

``` html
<div v-bind:class="{ active: isActive }"></div>
```

Վերևի գրելաձևը նշանակում է որ `active` class-ը կորոշվի ըստ `isActive` տվյալի հատկանիշի [ճշմարտությանը](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)։

Դուք կարող եք ունենալ մի քանի class-ներ որոնք toggle են լինում ունենալով համապատասխան դաշտերը ձեր օբյեկտում։ Ի հավելումն, `v-bind:class` ուղղորդիչը կարող է նաև գոյակցել պարզ `class` ատրիբուտի հետ։ Ինչպես տրված է հետևյալ ձևանմուշում։

``` html
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
```

ԵՎ հետևյալ տվյալները։

``` js
data: {
  isActive: true,
  hasError: false
}
```

Այն կարտատպի որպես;

``` html
<div class="static active"></div>
```

Երբ `isActive` կամ `hasError`-ը փոփոխվում է, class-ները կթարմացվեն համապատասխանորեն։ Օրինակ՝ եթե `hasError`-ը դառնա `true`, class-ները կդառնան `"static active text-danger"`:

Կապված օբյեկտը չպետք է լինի inline:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

Սա կտպի նույն արդյունքը։ Որը մենք նույպես կարող ենք կապել [հաշվարկած հատկանիշում](computed.html) որը վերադարձնում է օբյեկտ։ Սա հասարակ և հզոր ձևավորում է։

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

### Զանգվածի Գրելաձև

Մենք կարող ենք փոխանցել զանգված `v-bind:class`-ին որպեսզի այն կիրառի class-ների ցանկ։

``` html
<div v-bind:class="[activeClass, errorClass]"></div>
```
``` js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

Որը կտպի։

``` html
<div class="active text-danger"></div>
```

Եթե դուք ցանկանում եք միացնել կամ անջատել class-ը ցանկի մեջ պայմանականորեն, դուք կարող եք փորձել եռյակ արտահայտությունը։

``` html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

Սա միշտ կկիրառի `errorClass`, բայց միայն կկիրառի `activeClass` երբ `isActive`-ը ճշմարիտ է։

Սակայն, սա կարող է լինել մի փոքր բառալի, եթե ունեք բազմաթիվ պայմանական class—ներ: Այդ իսկ պատճառով հնարավոր է օգտագործել օբյեկտի գրելաձևը զանգվածի գրելաձևի ներսում։

``` html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### Կոմպոնենտների Հետ

> Այս բաժինը ենթադրում է որ դուք ունեք [Vue Կոմպոնենտների](components.html) գիտելիքներ։ Ազատ ըզգացեք բաց թողնելու այս բաժին և վերադառնալ հետո։

Երբ դուք օգտագործում եք `class` ատրիբուտը custom կոմպոնենտի վրա, այդ class-ները կավելացվեն կոմպոնենի արմատային էլեմենտին։ Գոյություն ունեցող class-ները այս էլեմենտի վրա չեն վերագրվի։

Օրինակ՝ եթե դուք հայտարարեք այս կոմպոնենտը։

``` js
Vue.component('my-component', {
  template: '<p class="foo bar">Բարև</p>'
})
```

Այնուհետև եթե ավելացնեք մի քանի class-ներ երբ օգտագործում եք այն։

``` html
<my-component class="baz boo"></my-component>
```

Տպված HTML—ը կլինի։

``` html
<p class="foo bar baz boo">Բարև</p>
```

Նույն բանը ճշմարիտ է class-ների միացման ժամանակ։

``` html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

Երբ `isActive`-ը ճշմարիտ է, տպված HTML-ը կլինի։ 

``` html
<p class="foo bar active">Բարև</p>
```

## Կապվող Inline Ոճեր

### Օբյեկտի Գրելաձև 

Օբյեկտի գրելաձևը `v-bind:style`-ի շատ պարզ է - այն համարյա նման է CSS-ի, բայց այն JavaScript օբյեկտ է։ Դուք կարող եք օգտագործել camelCase կամ kebab-case (օգտագործեք չակերտներ kebab-case-ի դեպքում) CSS-ի հատկանիշների անունների համար։

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

Հաճախ շատ լավ գաղափար է ուղղակիորեն կապվել ոճային օբյեկտի հետ, որպեսզի ձևանմուշը ավելի մաքուր լինի։

``` html
<div v-bind:style="styleObject"></div>
```
``` js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

Նորից, օբյեկտի գրելաձևը շատ հաճախ օգտագործվում է հաշվարկված հատկությունների հետ որոնք վերադարձնում են օբյեկտներ։

### Զանգվածի Գրելաձև

Զանգվածի գրելաձևի `v-bind:style`-ում թույլ է տալիս ձեզ որպեսզի կիրառել միքանի ոճային օբյեկտներ նույն էլեմենտին։

``` html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### Ավտոմատ նախածանցում (prefixing)

Երբ օգտագործում ենք CSS հատկություն որը պահանջում է [vendor prefix](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix)-ներ `v-bind:style`-ում, օրինակ՝ `transform`, Vue-ն ավտոմատ կնկատի և կավելացնի անհրաժեշտ prefix-ները կիրառած ոճերի համար։

### Բազմակի Արժեքներ

> 2.3.0+

Սկսած 2.3.0+ տարբերակի մեջ դուք կարող եք տրամադրել զանգված բազմակի (նախածանցված) արժեքներ style հատկությանը, օրինակ։

``` html
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

Սա միայն կտպի վերջին արժեքը զանգվածի մեջ որը որ բրաուզերը հասկանում է։ Այս օրինակում, այն կտպի `display:flex` այն բրաուզերների համար որոնք աշխատում են flexbox-ի չնախածանցված տարբերակով։
