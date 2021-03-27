---
title: Հաշվարկված Հատկություններ և Watcher-ներ
type: ուղեցույց
order: 5
---

## Հաշվարկված Հատկություններ

<div class="vueschool"><a href="https://vueschool.io/lessons/vuejs-computed-properties?friend=vuejs" target="_blank" rel="sponsored noopener" title="Սովորեք թե ինչպես են հաշվարկված հատկությունները աշխատում Vue School-ի հետ">Սովորեք Vue School-ում թե ինչպես են հաշվարկված հատկությունները աշխատում</a></div>

Ձևանմուշում գտվող արտահայտությունները շատ հարմար են օգտագործման համար, բայց նրանք նախատեսված են հասարակ գործողությունների համար։ Դնելով շատ տրամաբանություն ձևանմուշներում կարող է անիմաստ մեծացնել նրա չափսերը և դժվարացնել նրանց պահպանումը։ Օրինակի համար․

``` html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

Այս կետից սկսած, ձևանուշը այլևս պարզ և դեկլարատիվ չէ։ Դուք պետք է ավելի երկար նայեք նրան որ հասկանաք որ սա տպելու է `message`-ը հակառակ։ Խնդիրը ավելի է բարդանում երբ որ դուք ցանկանում եք ներառել նամակը հակառակ, ձեր ձևանմուշում մեկից ավելի անգամ։

Այդ պատճառով ցանկացած բարդ տրամաբանության համար, դուք պետք է օգտագործեք **հաշվարկված հատկություններ**։

### Սովորական Օրինակ

``` html
<div id="example">
  <p>Օրիգինալ Նամակը: "{{ message }}"</p>
  <p>Հաշվարկված Հակառակ Նամակը: "{{ reversedMessage }}"</p>
</div>
```

``` js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Բարև'
  },
  computed: {
    // հաշվարկված ստացող
    reversedMessage: function () {
      // `this`-ը ուղղում է դեպի vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
```

Արդյունքում:

{% raw %}
<div id="example" class="demo">
  <p>Օրիգինալ Նամակը: «{{ message }}»</p>
  <p>Հաշվարկված Հակառակ Նամակը: «{{ reversedMessage }}»</p>
</div>
<script>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Բարև'
  },
  computed: {
    reversedMessage: function () {
      return this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Այստեղ մենք հայտարարել ենք հաշվարկված հատկություն `reversedMessage`։ Այս ֆունկցիան որը մենք տրամադրել ենք կօգտագործվի որպես ստացող ֆունկցիա `vm.reversedMessage` հատկության համար։

``` js
console.log(vm.reversedMessage) // => 'ևրաԲ'
vm.message = 'Ցտեսություն'
console.log(vm.reversedMessage) // => 'նույթուսետՑ'
```

Դուք կարող եք բացել console—ը և խաղալ vm օրինակի հետ ինքներտ։ `vm.reversedMessage`-ի արժեքը միշտ կախված է `vm.message`-ի արժեքից։

Դուք կարող եք կապել տվյալներ հաշվարկված հատկություններին ձևանմուշների մեջ ինչպես մենք անում ենք սովորական հատկությունների հետ։ Vue-ն տեղյակ է որ `vm.reversedMessage`-ը կախված է `vm.message`-ի հետ, այնպես որ այն կթարմացնի ցանկացած կապումներ որոնք կախված են `vm.reversedMessage`-ից երբ `vm.message`-ը փոխվում է։ ԵՎ ամենալավ մասը այն է որ մենք ստեղծել ենք այս կախվածություն ունեցող հարաբերությունը դեկլարատիվ ձևով։ Հաշվարկված ստացող ֆունկցիան չունի կողմնակի ազդեցություններ, սա հեշտացնում է թեստեր անելը և հասկանալը։

### Հաշվարկված Caching-ը ընդեմ Մեթոդների

Դուք հնարավոր է որ նկատել եք որ մենք կարող ենք հասնել նույն արդյունքին կանչելով մեթոդը արտահայտության մեջ․

``` html
<p>Հակառակ Նամակը: "{{ reverseMessage() }}"</p>
```

``` js
// կոմպոնենտի մեջ
methods: {
  reverseMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```

Հաշվարկված հատկությունների փոխարեն, մենք կարող ենք հայտարարել նույն ֆունկցիան որպես մեթոդ։ Վերջնական արդյունքում, երկու մոտեցումները նույնն են։ Սակայն, տարբերությունը կայանում է որ **հաշվարկված հատկությունները cache են եղած հիմնված իրենց ռեակտիվ կախվածություններից**։ Հաշվարկված հատկությունը միայն կվերահաշվարկվի երբ իր որոշ ռեակտիվ կախվածությունները փոփոխվել են։ Սա նշանակում է որ քանի դեռ `message`-ը չի փոխվել, բազմաթիվ մուտքեր դեպի `reversedMessage` հաշվարկված հատկություն կվերադարձնի նախկինում հաշվարկված արդյունքը առանց ֆունկցիան նորից աշխատացնելու։

Սա նաև նշանակում է որ հետևյալ հաշվարկված հատկությունը երբեք չի թարմացվի, որովհետև `Date.now()`-ը ռեակտիվ կախվածություն չէ․

``` js
computed: {
  now: function () {
    return Date.now()
  }
}
```

Համեմատաբար, մեթոդի կանչելը **միշտ** կաշխատացնի ֆունկցիան երբ re-render է լինում։

Ի՞նչու է մեզ պետք caching-ը։ Պատկերացրենք որ մենք ունենք բարդ հաշվարկված հատկություն **A** անվանումով, որը պահանջում է պտտվել մեծ զանգվածի միջով և կատարել լիքը հաշվարկումներ։ Այնուհետև մենք հնարավոր է ունենանք այլ հաշվարկված հատկություններ որոնք իրենց հերթին կախված են **A**-ից։ Առանց caching-ի, մենք կաշխատացնենք **A**-ի ստացողին ավելի շատ քան մեզ անհրաժեշտ է՜ Եթե դուք չեք ցանկանում caching օգտագործել, օգտագործեք մեթոդ փոխարենը։

### Հաշվարկված ընդեմ Դիտվող Հատկություն

Vue-ն տրամադրում է ընդհանուր դիտարկում և ռեակտիվություն տվյալների փոփոխությունների համար Vue instance-ում․ **դիտորդ հատկություններ**։ Երբ որ դուք ունեք որոշ տվյալներ որոնք պետք է փոփոխվեն հիմնված այլ տվյալների վրա, և գայթակղիչ է `watch`—ի վերօգտագործումը հատկապես երբ որ դուք նախկինում օգտագործել եք AngularJS։ Սակայն, հաճախ ավելի լավ միտք է օգտագործելու հաշվարկված հատկություններ քան հրամայական `watch` callback-ներ։ Համարեք այս օրինակը․

``` html
<div id="demo">{{ fullName }}</div>
```

``` js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

Վերևի կոդը հրամայական է և կրկնվող։ Համեմատեք այն հաշվարկված հատկության տարբերակի հետ․

``` js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

Ավելի լավ է, չէ՞

### Հաշվարկված Սահմանողներ

Հաշվարկված հատկությունները հիմնականում ստցաող (getter) են միայն, բայց մենք կարող ենք նաև տրամադրել սահմանող (setter) եթե ձեզ այն անհրաժեշտ է․

``` js
// ...
computed: {
  fullName: {
    // ստացող
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // սահմանող
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

Հիմա երբ որ դուք աշխատացնեք `vm.fullName = 'John Doe'`, սահմանողը կկանչվի և `vm.firstName`-ը և `vm.lastName`-ը կթարմացվեն համապատասխանորեն։

## Դիտորդներ (Watchers)

Երբ հաշվարկված հատկությունները ավելի ճիշտ են որոշ դեպքերում, կան նաև դեպքեր երբ custom watcher-ը անհրաժեշտ է: Այս պատճառով Vue-ն տրամադրում է ավելի հասարակ ճանապարհ արձագանքելու տվյալների փոխոխություններին `watch` ընտրանքի միջոցով։ Սա շատ օգտակար է երբ դուք ցանկանում եք կատարել ասինխռոն կամ երկարացված գործողություններ response-ի մեջ որպեսզի փոփոխեք տվյալները։

Օրինակի համար՝

``` html
<div id="watch-example">
  <p>
    Հարցրեք այո/ոչ հարց․
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```

``` html
<!-- Քանի դեռ կան հարուստ ajax գրադարանների էկոհամակարգեր -->
<!-- և հավաքածուներ հիմնական նպատակների սպասարկվող մեթոդներ, Vue-ի core-ը -->
<!-- կարողացել է մնալ փոքր առանց վերստեղծելու նրանց։ Սա նաև -->
<!-- տալիս է ձեզ ազատություն որպեսզի դուք օգտագործեք այն ինչի հետ որ դուք ծանոթ եք։ -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'Ես չեմ կարող տալ ձեզ պատասխան մինչ դուք ինձ հարց կտաք!'
  },
  watch: {
    // երը հարցը փոխվում է, այս ֆունկցիան կաշխատի
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Սպասում որ դուք դադարեցնեք գրելը...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // _.debounce-ը ֆունկցիա է տրամադրված lodash-ի կողմից որպեսզի սահմանափակել թե
    // ինչքան հաճախ երկար գործողությունը կարող է աշխատել։
    // Այս դեպքում, մենք ուզում ենք սահմանափակել թե որքան մենք կարող ենք մուտք գործել
    // yesno.wtf/api, սպասելով մինչ օգտագործողը վերջացրել է
    // գրելը նախքան ստեղծելով ajax հարցումը։ Որպեսզի սովորենք
    // ավելին _.debounce ֆունկցիայի (և իր զարմիկ
    // _.throttle-ի մասին), այցելեք: https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Հարցերը սովորականում պարունակում են հարցական։ ;-)'
        return
      }
      this.answer = 'Մտածում եմ...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Սխալ! Չկարողացա Կապ Հաստատել API-ի Հետ։ ' + error
        })
    }
  }
})
</script>
```

Արդյունքը:

{% raw %}
<div id="watch-example" class="demo">
  <p>
    Հարցրեք այո/ոչ հարց․
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'Ես չեմ կարող տալ ձեզ պատասխան մինչ դուք ինձ հարց կտաք!'
  },
  watch: {
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Սպասում որ դուք դադարեցնեք գրելը...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Հարցերը սովորականում պարունակում են հարցական։ ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Սխալ! Չկարողացա Կապ Հաստատել API-ի Հետ։ ' + error
        })
    }
  }
})
</script>
{% endraw %}

Այս դեպքում, օգտագործելով `watch` ընտրանքը մեզ թույլ է տալիս կատարել ասինխռոն գործողություն (API մուտք գործել), սահմանափակել թե որքան հաճախ մենք կարող ենք կատարել այս գործողությունը, և սահմանել միջնորդ վիճակները մինչ մենք կստանանք վերջնական պատասխանը։ Նշվածներից ոչ մեկը հնարավոր չէր լինի հաշվարկված հատկությունով։


Հավելյալ `watch` ընտրանքին, դուք նաև կարող եք օգտագործել հրամայական [vm.$watch API-ը](../api/#vm-watch)։
