---
title: Event-ի Handling-ը
type: ուղեցույց
order: 9
---

<div class="vueschool"><a href="https://vueschool.io/lessons/vuejs-user-events?friend=vuejs" target="_blank" rel="sponsored noopener" title="Սովորեք թե ինչպես ձեռնարկել event-ներ Vue School-ում">Սովորեք թե ինչպես ձեռնարկել event-ներ Vue School-ի անվճար դասում</a></div>

## Event-ներին Լսելը

Մենք կարող ենք օգտագործել `v-on` ուղղորդիչը որպեսզի լսել DOM event-ներին և աշխատացնել որոշ JavaScript նրանց արձակումից հետո։

Օրինակի համար․

```html
<div id="example-1">
  <button v-on:click="counter += 1">Ավելացնել 1</button>
  <p>Վերևի կոճակը սեղմվել է {{ counter }} անգամ։</p>
</div>
```

```js
var example1 = new Vue({
  el: "#example-1",
  data: {
    counter: 0,
  },
});
```

Արդյունքը․

{% raw %}

<div id="example-1" class="demo">
  <button v-on:click="counter += 1">Ավելացնել 1</button>
  <p>Վերևի կոճակը սեղմվել է {{ counter }} անգամ։</p>
</div>
<script>
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
</script>
{% endraw %}

## Մեթոդ Event-ի Handler-ները

Տրամաբանությունը շատ event handler-ներում կարող է լինել ավելի բարդ, այնպես որ պահպանելով ձեր JavaScript-ը `v-on` ատրիբուտի արժեքում պիտանի է։ Այս պատճառով `v-on`-ը կարող է նաև ընդունել անունը մեթոդի որը որ դուք ցանկանում եք կանչել։

Օրինակի համար՝

```html
<div id="example-2">
  <!-- `greet`-ը ներքևում հայտարարված մեթոդի անունն է -->
  <button v-on:click="greet">Բարևել</button>
</div>
```

```js
var example2 = new Vue({
  el: "#example-2",
  data: {
    name: "Vue.js",
  },
  // հայտարարեք մեթոդները `methods` օբյեկտի ներսում
  methods: {
    greet: function (event) {
      // `this`-ը մեթոդի ներսում տանում է դեպի Vue-ի instance—ը
      alert("Բարև " + this.name + "!");
      // `event`-ը native DOM event է
      if (event) {
        alert(event.target.tagName);
      }
    },
  },
});

// դուք կարող եք կանչել մեթոդներ նաև JavaScript-ում
example2.greet(); // => 'Բարև Vue.js!'
```

Արդյունքում․

{% raw %}

<div id="example-2" class="demo">
  <button v-on:click="greet">Բարևել</button>
</div>
<script>
var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  methods: {
    greet: function (event) {
      alert('Բարև ' + this.name + '!')
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})
</script>
{% endraw %}

## Մեթոդները Inline Handler-ներում

Փոխարեն կապելով ուղիղ մեթոդի անունին, մենք նաև կարող ենք օգտագործել մեթոդներ inline JavaScript հայտարարություններում․

```html
<div id="example-3">
  <button v-on:click="say('բարև')">Ասել Բարև</button>
  <button v-on:click="say('ի՞նչ')">Ասել Ի՞նչ</button>
</div>
```

```js
new Vue({
  el: "#example-3",
  methods: {
    say: function (message) {
      alert(message);
    },
  },
});
```

Արդյունքում․
{% raw %}

<div id="example-3" class="demo">
  <button v-on:click="say('բարև')">Ասել Բարև</button>
  <button v-on:click="say('ի՞նչ')">Ասել Ի՞նչ</button>
</div>
<script>
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
</script>
{% endraw %}

Երբևմն մենք նաև պետք է մուտք գործենք օրիգինալ DOM event inline հայտարարության handler-ում։ Դուք կարող եք փոխանցել այն մեթոդին օգտագործելով հատուկ `$event` փոփոխականը․

```html
<button v-on:click="warn('Form-ը չի կարող կիրառվել դեռ։', $event)">
  Կիրառել
</button>
```

```js
// ...
methods: {
  warn: function (message, event) {
    // հիմա մենք ունենք մուտք դեպի native event
    if (event) {
      event.preventDefault()
    }
    alert(message)
  }
}
```

## Event-ի Փոփոխիչներ

Շատ սովորական է երբ անհրաժեշտ է կանչել `event.preventDefault()`-ը կամ `event.stopPropagation()`-ը event handler-ների ներսում։ Չնայած որ մենք կարող ենք անել սա ավելի հեշտ մեթոդների ներսում, բայց ավելի լավ կլինի եթե մեթոդները տվյալների տրամաբանության մասին են քան թե ինչպես DOM event-ների հետ աշխատելու մանրամասների մասին։

Որպեսզի լուծենք այս հարցը, Vue-ն տրամադրում է **event-ի փոփոխիչներ** `v-on`-ի համար։ Հիշեցում որ փոփոխիչները հաջորդում են ուղղորդիչների ֆունկցիաներին կետով։

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

```html
<!-- click event-ի տարածումը կդադարեցվի -->
<a v-on:click.stop="doThis"></a>

<!-- submit event-ը այլևս էջը չի վերբեռնի -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- փոփոխիչները կարող են շղթայվել -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- ուղղակի փոփոխիչը -->
<form v-on:submit.prevent></form>

<!-- օգտագործեք որպեսզի ստանալ ռեժիմը երբ ավելացնում եք event listener -->
<!-- օրինակ event որը նշում է միջև գտնվող էլեմենտ աշխատում է այստեղ նաղքան էլեմենտի կողմից աշխատելը -->
<div v-on:click.capture="doThis">...</div>

<!-- միայն արձակողի handler-ը եթե event.target-ը նույն էլեմենտն է -->
<!-- այն ժառանգող էլեմենտից չէ -->
<div v-on:click.self="doThat">...</div>
```

<p class="tip">Հերթականությունը նշանակություն ունի երբ օգտագործում ենք փոփոխիչներ որովհետև հարաբերական կոդը գեներացվել է նույն հերթականությամբ։ Դրանից դատելով օգտագործելով `v-on:click.prevent.self`-ը կկանխի **բոլոր սեղմումները** երբ `v-on:click.self.prevent`-ը միայն կկանխի սեղմումները էլեմենտի վրա։</p>

> Նոր 2.1.4+-ի մեջ

```html
<!-- click event-ը միայն կարձակվի մեկ անգամ -->
<a v-on:click.once="doThis"></a>
```

Ի համեմատ այն փոփոխիչների, որոնք միայն բացառիկ են native DOM event-ներին, `.once` փոփոխիչը կարող է նաև օգտագործվել [կոմպոնենտի event-ներում](components-custom-events.html)։ Եթե դուք չեք կարդացել կոմպոնենտների մասին դեռ, մի անհանգստացեք։

> Նոր 2.3.0+-ի մեջ

Vue-ն նաև առաջարկում է `.passive` փոփոխիչը, համապատասխան [`addEventListener`-ի `passive` ընտրանքին](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#Parameters)։

```html
<!-- scroll event-ի հիմնական behavior-ը (scroll անելը) կկայանա -->
<!-- միանգամից, փոխարեն սպասելով `onScroll`-ին որպեսզի ավարտվի -->
<!-- այս դեպքում այն պարունակում է `event.preventDefault()`-ը -->
<div v-on:scroll.passive="onScroll">...</div>
```

`.passive` փոփոխիչը հատկապես օգտակար է որպեսզի բարելավի performance-ը շարժական սարքերի համար (հեռախոսների, պլանշետների և այլն)։

<p class="tip">Չօգօտագործեք `.passive` և `.prevent`-ը միասին, որովհետև `.prevent`-ը կանտեսվի և ձեր բրաուզերը հավանականորեն ցույց կտա նախազգուշացում։ Հիշեք, `.passive`-ը կապ է հաստատում բրաուզերի հետ պահանջելով որ դուք _չեք_ ցանկանում կանխել event-ի հիմնական behavior-ը։</p>

## Կոճակի Փոփոխիչներ

Երբ լսում ենք ստեղնաշարի event-ներին, մեզ հաճախ հարկավոր է ստուգել հատուկ կոճակի համար։ Vue-ն թույլ է տալիս ավելացնելու կոճակի փոփոխիչներ `v-on`-ի համար երբ լսում ենք կոճակի event-ներին․

```html
<!-- միայն կանչել `vm.submit()`-ը երբ `կոճակը` `Enter`-ն է -->
<input v-on:keyup.enter="submit" />
```

Դուք կարող եք ուղիղ օգտագործել ցանկացած ճիշտ կոճակի անվանում որը կարող եք ստանալ [`KeyboardEvent.key`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) որպես փոփոխիչներ փոխակերպելով նրանց դեպի kebab-case:

```html
<input v-on:keyup.page-down="onPageDown" />
```

Վերևի օրինակում, handler-ը միայն կկանչվի եթե `$event.key`-ն հավասար է `«PageDown»`-ին։

### Կոճակների Կոդերը

<p class="tip">`keyCode`-ի event-ները [հնացած են](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode) և հնարավոր է որ չեն համապատասխանի նոր բրաուզերներին։</p>

`keyCode` ատրիբուտների օգտագործում նաև թույլ է տրվում․

```html
<input v-on:keyup.13="submit" />
```

Vue-ն տրամադրում է անուններ հաճախ օգտագործվող կոճակների կոդերի համար երբ անհրաժեշտ է հին բրաուզերների համար․

- `.enter`
- `.tab`
- `.delete` (կապվում է «Delete» և «Backspace» կոճակները)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

<p class="tip">Մի քանի կոճակներ (`.esc` և բոլոր սլաքների կոճակները) ունեն անհետևողական `key` արժեք IE9-ի մեջ, այնպես որ այս ներքուստ կառուցված անունները ցանկալի են եթե ձեզ անհրաժեշտ է IE9-ի համապատասխանեցումը։</p>

Դուք նաև կարող եք [հայտարարել custom կոճակի modifier-ի անուն](../api/#keyCodes) գլոբալ `config.keyCodes` օբյեկտի շնորհիվ․

```js
// միացրեք `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112;
```

## Համակարգի Փոփոխիչ Կոճակները

> Նոր 2.1.0+-ի մեջ

Դուք կարող եք օգտագործել հետևյալ փոփոխիչները որպեսզի արձակել մկնիկի կամ ստեղնաշարի event-ի լսողներ միայն երբ համապատասխան փոփոխիչի կոճակն է սեղմված․

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

> Նշում: Macintosh-ի ստեղնաշարներում, մետան հրամանի կոճակն է (⌘)։ Windows-ի ստեղնաշարներում, մետան Windows-ի կոճակն է (⊞)։ Sun Microsystems-ի ստեղնաշարերում, մետան նշաված է որպես ադամանդ (◆)։ Որոշ ստեղնաշարերում, հատկապես MIT և Lisp մեքենայի ստեղնաշարները և կատարելագործողների, ինչպիսիք են Knight ստեղնաշարը, space-cadet ստեղնաշարը, մետան անվանված է «META» կամ «Meta»:

Օրինակի համար․

```html
<!-- Alt + C -->
<input v-on:keyup.alt.67="clear" />

<!-- Ctrl + Click -->
<div v-on:click.ctrl="doSomething">Անել մի բան</div>
```

<p class="tip">Նշում որ փոփոխիչի կոճակները տարբերվում են հասարակ կոճակներից և երբ օգտագործվում են `keyup` event-ների հետ, նրանք պետք է լինեն սեղմված երբ event-ը արձակված է։ Այլ բառերով, `keyup.ctrl`-ը միայն կարձակի եթե դուք բաց թողնեք կոճակը սեղմած պահելով `ctrl`-ը։ Այն չի արձակվի եթե դուք բաց թողնեք միայն `ctrl` կոճակը։ Եթե դուք ցանկանում եք այդպիսի վարքագիծ, օգտագործեք `keyCode`-ը `ctrl`-ի փոխարեն․ `keyup.17`։</p>

### `.exact` Փոփոխիչը

> Նոր 2.5.0+-ի մեջ

`.exact` փոփոխիչը թույլ է տալիս կառավարելու հատուկ կոմբինացիա համակարգի փոփոխիչների որոնք պետք է արձակեն event:

```html
<!-- Սա կաշխատի նույնիսկ եթե Alt կամ Shift-ը նույնպես սեղմված է -->
<button v-on:click.ctrl="onClick">A</button>

<!-- Սա միայն կաշխատի երբ Ctrl-ը սեղմված է առանց հավելյալ կոճակների -->
<button v-on:click.ctrl.exact="onCtrlClick">A</button>

<!-- Սա միայն կաշխատի երբ ոչ մի համակարգի փոփոխիչներ սեղմված չեն -->
<button v-on:click.exact="onClick">A</button>
```

### Մկնիկի Կոճակի Փոփոխիչներ

> Նոր 2.2.0+-ի մեջ

- `.left`
- `.right`
- `.middle`

Այս փոփոխիչները արգելում են handler-ին այն event-ներից որոնք արձակվել են հատուկ մկնիկի կոճակի օգնությամբ։

## Ի՞նչու Օգտագործել Listener-ներ HTML-ի Մեջ

Դուք հնարավոր է որ մտահոգված կլինեք որ այս ամբողջ event լսելու մոտեցումները խախտում են հին լավ կաննոները «մտահոգությունների տարանջատումների» մասին։ Մնացածում վստահ եղեք - քանի որ Vue-ի handler ֆունկցիաները և արտահայտությունները խստորեն կապված են ViewModel-ին որը կառավարում է ընտացիկ տեսքը, այն չի առաջացնի որևէ պահպանման դժվարություններ։ Ավելին, կան մի քանի առավելություններ `v-on`—ի օգտագործելու մեջ․

1. Ավելի հեշտ է փնտրելու handler ֆունկցիաների իրականացումները ձեր JS կոդում փոքրացնելով HTML ձևանմուշը։

2. Քանի որ դուք չպետք է ձեռքով միացնեք event-ի լսողներ JS-ում, ձեր ViewModel կոդը կարող է մաքուր տրամաբանություն լինել և DOM—ից ազատ։ Սա դարձնում է ավելի հեշտ թեստեր անելու համար։

3. Երբ ViewModel-ը ոչնչացված է, բոլոր event-ի լսողները ավտոմատ կերպով հեռացվում են։ Դուք չպետք անհանգստանաք նրանց մաքրելու մասին։
