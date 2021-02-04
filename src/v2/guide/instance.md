---
title: Vue-ի Instance-ը
type: ուղեցույց
order: 3
---

## Vue-ի Instance-ի Ստեղծումը

Ամեն Vue ծրագիր սկսվում է նոր **Vue instance**-ի ստեծումից որը կատարվում է `Vue` ֆունկցիայի շնորհիվ․

```js
var vm = new Vue({
  // ընտրանքներ
});
```

Չնայած դրան խիստ կապված չէ [MVVM pattern-ը](https://en.wikipedia.org/wiki/Model_View_ViewModel), Vue-ի ոճը որոշ չափով ոգեշնչվել է դրանով։ Որպես հարմարանք, մենք հաճախ օգտագործում ենք փոփոխական `vm` (ViewModel-ի կարճ տարբերակը) որպեսզի դիմենք մեր Vue-ի instance-ին։

Երբ որ դուք ստեծում եք Vue instance, դուք փոխանցում եք **ընտրանքների օբյեկտ**։ Մեծամանսությունը այս ուղեցույցում նկարագրում է թե ինչպես դուք կարող եք օգտագործել այս ընտրանքները որպեսզի ստեծեք ձեր ցանկացած behavior-ը։ Դուք նաև կարող եք զննել ընտրանքների ամբողջ ցանկը [API reference-ում](../api/#Options-Data)։

Vue-ի ծրագիրը բաղկացած է **արմատային Vue instance—ից** ստեղծված `new Vue`-ով, ըստ ցանկությամբ տեղադրված վերօգտագործվող կոմպոնենտների, ծառի մեջ։ Օրինակի համար, todo—ի ծրագրի կոմպոնենտի ծառը կարող է այսպիսին լինել․

```
Root Instance
└─ TodoList
   ├─ TodoItem
   │  ├─ TodoButtonDelete
   │  └─ TodoButtonEdit
   └─ TodoListFooter
      ├─ TodosButtonClear
      └─ TodoListStatistics
```

Մենք մանրամասն կխոսանք [կոմպոնենտի համակարգի](components.html) մասին ավելի ուշ։ Այժմ, ուղակի իմացեք որ բոլոր Vue կոմպոնենտները նաև Vue instance-ներ են, և ընդհունում են նույն ընտրանքների օբյեկտը (բացառությամբ մի քանի արմատին հատուկ ընտրանքներ):

## Տվյալներ և Մեթոդներ

Երբ Vue instance—ը ստեղծված է, այն ավելացնում է բոլոր հատկությունները որոնք գտնվում են իր `data` օբյեկտում Vue-ի **ռեակտիվության համակարգում**։ Երբ արժեքները այս հատկությունների փոփոխվում է, տեսքը «ռեակցիա» կտա, և կթարմացնի նոր արժեքներին համապատասխան։

```js
// Մեր data օբյեկտը
var data = { a: 1 };

// Օբյեկտը որը ավելացվել է Vue instance-ին
var vm = new Vue({
  data: data,
});

// Ստանալով հատկությունները instance-ում
// վերադարձնում է սկզբնական տվյալները
vm.a == data.a; // => true

// Դնելով հատկություն instance-ում
// նաև ազդում է սկզբնական տվյալների վրա
vm.a = 2;
data.a; // => 2

// ... և հակառակը
data.a = 3;
vm.a; // => 3
```

Երբ այս տվյալները փոփոխվում է, տեսքը նորից render կլինի։ Պետք է նաև նշվի որ այդ հատկությունները որոնք գտնվում են `data`-ում միայն **ռեակտիվ** են եթե գոյություն են ունեցել երբ instance—ը ստեղծվել է։ Սա նշանակում է եթե դուք ավելացնեք նոր հատկություն, ինչպես այստեղ․

```js
vm.b = "բարև";
```

Այնուհետև այս փոփոխությունները `b`-ին չէն արձակի որևէ տեսքի թարմացումներ։ Եթե դուք գիտեք որ ձեզ պետք կգա հատկություն հետո, բայց այս սկզբում լինում է դատարկ կամ գոյություն չունեցող, դուք պետք է տեղադրեք սկբնական արժեքը։ Օրինակի համար․

```js
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

Միակ բացառությունը եթե այն կօգտագործի `Object.freeze()`, որը թույլ չի տալիս որ գոյություն ունեցող հատկությունները փոփոխվել, որը նաև նշանակում է որ ռեակտիվության համակարգը չի կարող _հետևի_ փոփոխություններին։

```js
var obj = {
  foo: "bar",
};

Object.freeze(obj);

new Vue({
  el: "#app",
  data: obj,
});
```

```html
<div id="app">
  <p>{{ foo }}</p>
  <!-- սա այլևս չի թարմացնի `foo`-ն՜ -->
  <button v-on:click="foo = 'baz'">Փոխիր այն</button>
</div>
```

Ի հավելումն տվյալների հատկություններին, Vue-ի instance-ը բացահայտում է բազմաթիվ օգտակար instance—ի հատկություններ և մեթոդներ։ Նրանք prefix են եղած `$` նշանով, որպեսզի տարբերվեն օգտագործողի կողմից հայտարարած հատկություններից։ Օրինակի համար․

```js
var data = { a: 1 };
var vm = new Vue({
  el: "#example",
  data: data,
});

vm.$data === data; // => true
vm.$el === document.getElementById("example"); // => true

// $watch-ը instance-ի մեթոդ է
vm.$watch("a", function (newValue, oldValue) {
  // Այս callback-ը կաշխատի երբ `vm.a` փոփոխվում է
});
```

Ապագայում դուք կարող եք խորհրդակցվել [API reference-ով](../api/#Instance-Properties) որը պարունակում է instance—ի հատկությունների և մեթոդների ամբողջ ցանկը։

## Instance-ի Lifecycle Hook-երը

<div class="vueschool"><a href="https://vueschool.io/lessons/understanding-the-vuejs-lifecycle-hooks?friend=vuejs" target="_blank" rel="sponsored noopener" title="Դիտեք Vue.js Lifecycle Hook-երի Դասը">Դիտեք անվճար դաս Vue School-ում</a></div>

Ամեն Vue instance անցնում է մի քանի քայլերի միջով երբ այն ստեղծվում է - օրինակի համար, այն պետք է տեղադրի տվյալների դիտարկում, compile անի ձևանմուշը, տեղադրի instance-ը DOM-ում, և թարմացնի DOM-ը երբ տվյալները փոփոխվում են։ Դրանց հետ համատեղ, այն նաև աշխատացնում է ֆունկցիաներ անվանված **lifecycle hooks**, որը տալիս է օգտագործողին հնարավորություն որպեսզի ավելացնի իր կոդը կոնկրետ փուլում։

Օրինակի համար, [`created`](../api/#created) hook-ը կարող է օգտագործվի որպեսզի աշխատացնի կոդը instance—ի ստեղծումից հետո․

```js
new Vue({
  data: {
    a: 1,
  },
  created: function () {
    // `this`-ը նշում է vm instance-ը
    console.log("a-ն: " + this.a + " է");
  },
});
// => "a-ն: 1 է"
```

Կան նաև այլ hook-եր որոնք պետք է կանչվեն տարբեր փուլերում, ինչպիսիք են [`mounted`](../api/#mounted),[`updated`](../api/#updated), և [`destroyed-ը`](../api/#destroyed)։ Բոլոր lifecycle hook-երը կանչվում են իրենց `this`-ով որը նշում է Vue-ի instance-ը նրա օգտագործման ժամանակ։

<p class="tip">Մի օգտագործեք [սլաքով ֆունկցիաները](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) ընտրանքի հատկության կամ callback—ի համար, ինչպիսին են `created: () => console.log(this.a)` կամ `vm.$watch('a', newValue => this.myMethod())`։ Որովհետև սլաքով ֆունկցիան չունի `this`, `this`-ը կվերաբերվի որպես ցանկացած այլ փոփոխական որը գտնվում է ծնող scope-ում, որը հաճախ հանգեցնում է սխալների ինչպիսիք են `Uncaught TypeError: Cannot read property of undefined` կամ `Uncaught TypeError: this.myMethod is not a function`։</p>

## Lifecycle Դիագրամ

Ներքևում դիագրամն է instance-ի lifecycle-ի։ Կարիք չկա մանրամասն հասկանալու այն ինչ կատարվում է նկարում, բայց ինչքան կառուցեք կամ սովորեք, այն կլինի օգտակար ռեսուրս։

![Vue Instance-ի Lifecycle-ը](/images/lifecycle.png)
