---
title: Ցանկերի Մատուցում
type: ուղեցույց
order: 8
---

<div class="vueschool"><a href="https://vueschool.io/lessons/vuejs-loops?friend=vuejs" target="_blank" rel="sponsored noopener" title="Սովորեք թե ինչպես render անել ցուցակներ Vue School-ում">Սովորեք թէ ինչպես տպել ցուցակներ Vue School-ի անվճար դասում</a></div>


## Map-անելով Զանգվածը դեպի Էլեմենտներ `v-for`-ի շնորհիվ

Մենք կարող ենք օգտագործել `v-for` ուղղորդիչը որպեսզի render անենք ցուցակ կախված զանգվածից։ `v-for` ուղղորդիչը պահանջում է հատուկ գրելաձև հետևյալ կերպով `item in items`, որտեղ `items`-ը հիմնական տվյալների զանգվածն է և `item`-ը որպես **ծածկանուն** զանգվածի էլեմենտի համար,

``` html
<ul id="example-1">
  <li v-for="item in items" :key="item.message">
    {{ item.message }}
  </li>
</ul>
```

``` js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

Արդյունքում:

{% raw %}
<ul id="example-1" class="demo">
  <li v-for="item in items" :key="item.message">
    {{item.message}}
  </li>
</ul>
<script>
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
</script>
{% endraw %}

`v-for`-ի մեջի բլոկներում մենք ունենք մուտք գործելու հնարավորություն դեպի ծնողի հատկություններ։ `v-for`—ը նաև ունի երկրորդ արգումենտ որը կարող է լրացվել ըստ ցանկությամբ, այն վերադարձնում է ընթացող էլեմենտի ինդեքսը։

``` html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

``` js
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Ծնող',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

Արդյունքում․

{% raw%}
<ul id="example-2" class="demo">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
<script>
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Ծնող',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
</script>
{% endraw %}

Դուք կարող եք նաև օգտագործել `of` որպես սահմանափակիչ `in`-ի փոխարեն, այնպես որ այն մոտիկ կլինի JavaScript-ի գրելաձևին ընթացողների համար․

``` html
<div v-for="item of items"></div>
```

## `v-for`-ը Օբյեկտի հետ

Դուք նաև կարող եք օգտագործել `v-for`-ը որպեսզի ընթանաք օբյեկտի հատկությունների միջով։

``` html
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
```

``` js
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'ԻՆչպես ստեղծել ցանկեր Vue-ում',
      author: 'Պողոս Պետրոսյան',
      publishedAt: '2016-04-10'
    }
  }
})
```

Result:

{% raw %}
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
<script>
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'ԻՆչպես ստեղծել ցանկեր Vue-ում',
      author: 'Պողոս Պետրոսյան',
      publishedAt: '2016-04-10'
    }
  }
})
</script>
{% endraw %}

Դուք նաև կարող եք տրամադրել երկրորդ արգումենտը հատկության անվան համար (հայտնի է նաև որպես բանալի)․

``` html
<div v-for="(value, name) in object">
  {{ name }}: {{ value }}
</div>
```

{% raw %}
<div id="v-for-object-value-name" class="demo">
  <div v-for="(value, name) in object">
    {{ name }}: {{ value }}
  </div>
</div>
<script>
new Vue({
  el: '#v-for-object-value-name',
  data: {
    object: {
      title: 'ԻՆչպես ստեղծել ցանկեր Vue-ում',
      author: 'Պողոս Պետրոսյան',
      publishedAt: '2016-04-10'
    }
  }
})
</script>
{% endraw %}

ԵՎ ևս մեկը ինդեքսի համար․

``` html
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```

{% raw %}
<div id="v-for-object-value-name-index" class="demo">
  <div v-for="(value, name, index) in object">
    {{ index }}. {{ name }}: {{ value }}
  </div>
</div>
<script>
new Vue({
  el: '#v-for-object-value-name-index',
  data: {
    object: {
      title: 'ԻՆչպես ստեղծել ցանկեր Vue-ում',
      author: 'Պողոս Պետրոսյան',
      publishedAt: '2016-04-10'
    }
  }
})
</script>
{% endraw %}

<p class="tip">Երբ օբյեկտի միջով ենք ընթանում, հերթականությունը կախված է `Object.keys()`-ի հաշվարկված հերթականությանը, որը երաշխավորված **չէ** որ հետևողական է JavaScript engine-ի տեղադրումներում։</p>

## Վիճակի Պահպանումը

Երբ Vue—ն թարմացնում է էլեմենտների ցանկը որոնք render են եղել `v-for`-ի շնորհիվ, հիմնականում այն օգտագործում է «տեղում լուծելու» ստրատեգիա։ Եթե հերթականությունը տվյալների փոփոխվել է, ի փոխարեն շարժելով DOM-ի էլեմենտները որպեսզի համապատասխանեն տվյալների էլեմենտների հետ, Vue-ն կուղղի ամեն էլեմենտը հենց տեղում և կհամոզվի թե ինչ պետք է render լինի այդ ինդեքսում։ Սա նման է `track-by="$index"`-ին որը Vue 1.x.-ի մեջ էր։

Այս հիմանական ռեժիմը արդյունավետ է, բայց **միայն հարմար է երբ ձեր ցուցակների render-ի output-ը կախված չէ ժառանգող կոմպոնենտի state-ից կամ ժամանակավոր DOM-ի state-ից (օրինակ form-ի մուտքագրված արժեքները)**։

Որպեսզի տալ Vue-ին հուշում որ այն կարողնա հետևի ամեն node-ի ինքնությանը, և դատելով դրանից վերօգտագործելով և վերադասավորով էլեմենտները, դուք պետք է տրամադրեք հատուկ `key` ատրիբուտ ամեն էլեմենտի համար․

``` html
<div v-for="item in items" v-bind:key="item.id">
  <!-- բովանդակություն -->
</div>
```

Խորհուրդ է տրվում տրամադրել `key` ատրիբուտ `v-for`—ի հետ երբ հնարավոր է, բացառությամբ եթե ընթացվող DOM-ի բովանդակությունը պարզ է, կամ դուք դիտավորյալ հենվել եք հիմնական behavior-ի և peformance—ի զարգացման համար։

Այն սովորական մեխանիզմ է Vue-ի համար որպեսզի այն ճանաչի node-երը, `key`-ն նաև ունի ուրիշ կիրառումները որոնք կոնկրետ կապված չեն `v-for`—ի հետ, մենք կնայենք դրանք ավելի ուշ ուղեցույցի մեջ։

<p class="tip">Չ՛օգտագործեք ոչ-հասարակ արժեքներ ինչպիսին են օբյեկտները և զանգվածները որպես `v-for`-ի key-եր։ Փոխարենը օգտագործեք string կամ թվային արժեքներ։</p>

Ավելի մանրամասնը `key` ատրիբուտի օգտագործման համար, կարող եք նայել այս [`key` API փաստաթղթերը](https://vuejs.org/v2/api/#key)։

## Զանգվածի Փոփոխման Հայտնաբերումը

### Մուտացիայի Մեթոդները

Vue-ն փաթաթվում է դիտարկված զանգվածի մուտացիայի մեթոդների վրա, որ նրանք նաև արձակեն տեսքի թարմացումներ։ Փաթաթված մեթոդները հետևյալն են․

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

Դուք կարող եք բացել console—ը և խաղալ նախորդ օրինակների հետ՝ `items`-ը զանգվածը կանչելով նրանց մուտացիայի մեթոդները։ Օրինակի համար․ `example1.items.push({ message: 'Baz' })`:

### Զանգվածի Փոխանակումը

Մուտացիայի մեթոդները, ինչպես անունն է հուշում, մուտացիայի են ենթարկում սկզբնական զանգվածը որի վրա նրանք կանչվել են։ Համեմատաբար, կան նաև ոչ-մուտացիոն մեթոդներ, օրինակ՝ `filter()`, `concat()` և `slice()`, որոնք մուտացիայի չեն ենթարկում սկզբնական զանգվածը բայց **միշտ վերադարձնում են նոր զանգված**։ Երբ աշխատում եք ոչ-մուտացիոն մեթոդներով, դուք կարող եք փոխարինել հին զանգվածները նորով․

``` js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

Դուք հնարավոր է որ կմտածեք որ դրա պատճառով Vue-ն կչեղարկի արդեն գոյություն ունեցող DOM և re-render կանի ամբողջ ցանկը - բարեբախտորեն, սա այդ դեպքը չէ։ Vue-ն ներառում է որոշ հաշվարկներ որպեսզի դարձնի DOM էլեմենտը մաքսիմալ վերօգտագործելի, այնպես որ փոխարինելով զանգվածը մեկ այլ զանգվածով որը պարունակում է համընկնող օբյեկտներ շատ արդյունավետ գործողություն է։

### Զգուշացումներ

JavaScript-ի սահմանափակումների պատճառով, կան տիպերի փոփոխություններ որոնք Vue-ն **չի կարող նկատել** զանգվածների և օբյեկտների հետ։ Այս դեպքերը քննարկվում են [ռեակտիվության](reactivity.html#Change-Detection-Caveats) բաժնում։

## Ֆիլտրված/Դասավորված Արդյունքների Ցուցադրումը

Երբեմն մենք ցանկանում ենք ցուցադրել ֆիլտրված կամ դասավորված տարբերակը զանգվածի առանց մուտացիայի ենթարկելու կամ սկզբնական տվյալների վերականգման։ Այս դեպում, դու կարող եք ստեղծել հաշվարկված հատկություն որը վերադարձնում է ֆիլտրված կամ դասավորված զանգված։

Օրինակի համար․

``` html
<li v-for="n in evenNumbers">{{ n }}</li>
```

``` js
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

Որոշ դեպքերում որտեղ հաշվարկված հատկությունները հասանելի չեն (օրինակ՝ ներքին `v-for`-ի ցիկլերում), դուք կարող եք օգտագործել մեթոդ։

```html
<ul v-for="set in sets">
  <li v-for="n in even(set)">{{ n }}</li>
</ul>
```

```js
data: {
  sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

## `v-for`-ը Միջակայքով

`v-for`-ը նաև կարող է ստանալ integer: Այս դեպքում այն կկրկնի ձևանմուշը բազմաթիվ անգամ։

``` html
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```

Արդյունքում․

{% raw %}
<div id="range" class="demo">
  <span v-for="n in 10">{{ n }} </span>
</div>
<script>
  new Vue({ el: '#range' })
</script>
{% endraw %}

## `v-for`-ը `<template>`-ի վրա

Նման լինելով ձևանմուշի  `v-if`-ին, դուք կարող եք նաև օգտագործել `<template>` tag-ը `v-for`-ի հետ որպեսզի render անել բազմաթիվ էլեմենտների բլոկներ։ Օրինակի համար․

``` html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```

## `v-for`-ը `v-if`-ի հետ

<p class="tip">Նշում որ խորհուրդ **չի** տրվում օգտագործել `v-if`-ը և `v-for`-ը միասին։ Դիմեք [ոճի ուղեցույցին](/v2/style-guide/#Avoid-v-if-with-v-for-essential) մանրամասների համար։</p>

Երբ նրանք գոյություն ունեն նույն node-ում, `v-for`-ը ունի ավելի բարձր կարևորություն քան `v-if`—ը։ Սա նշանակում է որ `v-if`—ը կաշխատի ամեն ցիկլի ընթացքի վրա առանձին։ Սա կարող է օգտակար լինել երբ դուք ցանկանում եք render անել node—երը միայն _որոշ_ էլեմենտների համար, ինչպես ներքևում է․

``` html
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```

Վերևում գրվածը միայն render է անում todo-ները որոնք ավարտված չեն։

Եթե փոխարենը, դուք մտադրություն ունեք պայմանականորեն բաց թողնել ցիկլի կատարումը, դուք կարող եք տեղադրել `v-if` փաթաթվող էլէմենտի վրա (կամ [`<template>`](conditional.html#Conditional-Groups-with-v-if-on-lt-template-gt)-ի վրա)։ Օրինակի համար․

``` html
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
```

## `v-for`-ը Կոմպոնենտի հետ

> Այս բաժինը ենթադրում է որ դուք ունեք [Կոմպոնենտների](components.html) գիտելիքներ։ Ազատ ըզգացեք բաց թողնելու այս բաժին և վերադառնալ հետո։

Դուք կարող եք ուղիղ օգտագործել `v-for`-ը custom կոմպոնենտի վրա, ինչպես ցանկացած հասարակ էլեմենտ․

``` html
<my-component v-for="item in items" :key="item.id"></my-component>
```

> 2.2.0+-ի մեջ, երբ օգտագործում ենք `v-for`-ը կոմպոնենտի հետ, [`key`](list.html#key)-ն հիմա պարտադիր է։

Սակայն, սա ավտոմատ կերպով չի փոխանցի որևէ տվյալներ կոմպոնենտին, որովհետև կոմպոնենտները ունեն մեկուսացված scope-եր։ Որպեսզի փոխանցել ընթացող տվյալները դեպի կոմպոնենտ, մենք պետք է նաև օգտագործենք prop-ները:

``` html
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
```

Պատճառը ավտոմատ կերպով չ՛ներարկելու `item`-ը դեպի կոմպոնենտ որովհետև դա կոմպոնենտը սերտորեն զուգորդվում է `v-for`-ի աշխատանքի ժամանակ։ Լինելով բացահայտ թե որտեղից է իր տվյալները գալիս դարձնում է կոմպոնենտը վերօգտագործելի այլ դեպքերում։

Այստեղ է ամբողջ օրինակը հասարակ todo ցանկի համար․

``` html
<div id="todo-list-example">
  <form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">Add a todo</label>
    <input
      v-model="newTodoText"
      id="new-todo"
      placeholder="E.g. Feed the cat"
    >
    <button>Add</button>
  </form>
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:key="todo.id"
      v-bind:title="todo.title"
      v-on:remove="todos.splice(index, 1)"
    ></li>
  </ul>
</div>
```

<p class="tip">Նշեք `is="todo-item"` ատրիբուտը։ Սա հարկավոր է DOM ձևանմուշներում, որովհետև միայն `<li>` էլեմենտն է ճիշտ `<ul>`-ի ներսում։ Այն նույնն է ինչ `<todo-item>`-ը, բայց աշխատում է պոտենցիալ բրաուզերի parsing error-ի հետ։ Նայեք [DOM Ձևանմուշի Parsing-ի Զգուշացումները](components.html#DOM-Template-Parsing-Caveats) որպեսզի իմանալ ավելին։</p>

``` js
Vue.component('todo-item', {
  template: '\
    <li>\
      {{ title }}\
      <button v-on:click="$emit(\'remove\')">Remove</button>\
    </li>\
  ',
  props: ['title']
})

new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',
    todos: [
      {
        id: 1,
        title: 'Do the dishes',
      },
      {
        id: 2,
        title: 'Take out the trash',
      },
      {
        id: 3,
        title: 'Mow the lawn'
      }
    ],
    nextTodoId: 4
  },
  methods: {
    addNewTodo: function () {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText
      })
      this.newTodoText = ''
    }
  }
})
```

{% raw %}
<div id="todo-list-example" class="demo">
  <form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">Add a todo</label>
    <input
      v-model="newTodoText"
      id="new-todo"
      placeholder="E.g. Feed the cat"
    >
    <button>Add</button>
  </form>
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:key="todo.id"
      v-bind:title="todo.title"
      v-on:remove="todos.splice(index, 1)"
    ></li>
  </ul>
</div>
<script>
Vue.component('todo-item', {
  template: '\
    <li>\
      {{ title }}\
      <button v-on:click="$emit(\'remove\')">Remove</button>\
    </li>\
  ',
  props: ['title']
})

new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',
    todos: [
      {
        id: 1,
        title: 'Do the dishes',
      },
      {
        id: 2,
        title: 'Take out the trash',
      },
      {
        id: 3,
        title: 'Mow the lawn'
      }
    ],
    nextTodoId: 4
  },
  methods: {
    addNewTodo: function () {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText
      })
      this.newTodoText = ''
    }
  }
})
</script>
{% endraw %}
