---
title: Form-երի Input-ի Կապումները
type: ուղեցույց
order: 10
---

## Սովորական Օգտագործումը

Դուք կարող եք օգտագործել `v-model` ուղղորդիչը որպեսզի ստեղծել երկու ճանապարհ տվյալների կապում form-ի input-ի, textarea-ի, և select էլեմենտների։ Այն ավտոմատ կերպով վերցնում է ճիշտ ճանապարհը որպեսզի թարմացնի էլեմենտը կախված մուտքագրման տիպից։ Չնայած որ այն մի փոքր կախարդական է, `v-model`-ը ըստ էության գրելաձևի շաքար է որպեսզի թարմացնի տվյալները օգտագործողի մուտքագրման event-ների համար, և հավելյալ վերաբերմունք է ցույց տալիս բացառիկ դեպքերի համար։

<p class="tip">`v-model`-ը կանտեսի սկզբնական `value`-ն, `checked`-ը, կամ `selected` ատրիբուտները որոնք գտնվել են ցանկացած form էլեմենտների վրա։ Այն միշտ կստանա Vue instance-ի տվյալները որպես ծշմարտության աղբյուր։ Դուք պետք է հայտարարեք սկզբնական արժեքը JavaScript-ի կողմից, `data` ընտրանքի մեջ որը գտնվում է ձեր կոմպոնենտում։</p>

`v-model`-ը ներքուստ օգտագործում է տարբեր հատկություններ և բաց է թողնում տարբեր event-ներ տարբեր input էլեմենտների համար․

- text և textarea էլեմենտները օգտագործում են `value` հատկությունը և `input` event-ը;
- checkbox-երը և radiobutton-ները օգտագործում են `checked` հատկությունը և `change` event-ը;
- select դաշտերը օգտագործում են `value`-ն որպես հատկություն և `change`-ը որպես event.

<p class="tip" id="vmodel-ime-tip">Լեզուների համար որոնք պահանջում են [IME](https://en.wikipedia.org/wiki/Input_method) (Չինարեն, Ճապոներեն, Կորեերեն, և այլն ․․․), դուք կնկատեք որ `v-model`-ը չի թարմացվում IME-ի ձևակերպման ժամանակ։ Եթե դուք նույնպես ցանկանում եք գործարկել այս թարմացումները, փոխարենը օգտագործեք `input` eventy:</p>

### Տեքստ

```html
<input v-model="message" placeholder="խմբագրիր ինձ" />
<p>Նամակը : {{ message }}</p>
```

{% raw %}

<div id="example-1" class="demo">
  <input v-model="message" placeholder="խմբագրիր ինձ">
  <p>Նամակը : {{ message }}</p>
</div>
<script>
new Vue({
  el: '#example-1',
  data: {
    message: ''
  }
})
</script>
{% endraw %}

### Բազմատող տեքստ

```html
<span>Բազմատող նամակը:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br />
<textarea v-model="message" placeholder="ավելացրեք միքանի տողեր"></textarea>
```

{% raw %}

<div id="example-textarea" class="demo">
  <span>Բազմատող նամակը:</span>
  <p style="white-space: pre-line;">{{ message }}</p>
  <br>
  <textarea v-model="message" placeholder="ավելացրեք միքանի տողեր"></textarea>
</div>
<script>
new Vue({
  el: '#example-textarea',
  data: {
    message: ''
  }
})
</script>
{% endraw %}

{% raw %}

<p class="tip">Ինտերպոլացիան textarea-ների վրա (<code>&lt;textarea&gt;{{text}}&lt;/textarea&gt;</code>) չի աշխատի։ Փոխարենը օգտագործեք <code>v-model</code>-ը։</p>
{% endraw %}

### Checkbox

Մեկ checkbox, boolean արժեք․

```html
<input type="checkbox" id="checkbox" v-model="checked" />
<label for="checkbox">{{ checked }}</label>
```

{% raw %}

<div id="example-2" class="demo">
  <input type="checkbox" id="checkbox" v-model="checked">
  <label for="checkbox">{{ checked }}</label>
</div>
<script>
new Vue({
  el: '#example-2',
  data: {
    checked: false
  }
})
</script>
{% endraw %}

Բազմաթիվ checkbox-եր, կապված նույն զանգվածին․

``` html
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
<br>
<span>Նշված անունները: {{ checkedNames }}</span>
```

```js
new Vue({
  el: '...',
  data: {
    checkedNames: [],
  },
});
```

{% raw %}

<div id="example-3" class="demo">
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Նշված անունները: {{ checkedNames }}</span>
</div>
<script>
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
</script>
{% endraw %}

### Radio

```html
<input type="radio" id="one" value="One" v-model="picked" />
<label for="one">Մեկ</label>
<br />
<input type="radio" id="two" value="Two" v-model="picked" />
<label for="two">Երկու</label>
<br />
<span>Ընտրածները: {{ picked }}</span>
```

{% raw %}

<div id="example-4" class="demo">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">Մեկ</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Երկու</label>
  <br>
  <span>Ընտրածները: {{ picked }}</span>
</div>
<script>
new Vue({
  el: '#example-4',
  data: {
    picked: ''
  }
})
</script>
{% endraw %}

### Select

Մեկ select:

```html
<select v-model="selected">
  <option disabled value="">Ընտրեք մեկը</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<span>Նշվածը: {{ selected }}</span>
```

```js
new Vue({
  el: "...",
  data: {
    selected: "",
  },
});
```

{% raw %}

<div id="example-5" class="demo">
  <select v-model="selected">
    <option disabled value="">Ընտրեք մեկը</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Նշվածը: {{ selected }}</span>
</div>
<script>
new Vue({
  el: '#example-5',
  data: {
    selected: ''
  }
})
</script>
{% endraw %}

<p class="tip">Երե սկզբնական արժեքը ձեր `v-model` արտահայտության չի համապատասխանում ընտրանքներից որև մեկին, `<select>` էլեմենտը render կլինի «չնշված» վիճակում։ iOS-ում, սա չի թողնի օգտագործողին նշել առաջին ընտրանքը, որովհետև iOS-ը չի արձակում `change` event այս դեպքում։ Այս պատճառով խորհուրդ է տրվում տրամադրել `disabled` ընտրանք դատարկ արժեքով, ինչպես ներկայացված է վերևում գտվող օրինակում։</p>

Միքանի select (կապված զանգվածին):

```html
<select v-model="selected" multiple>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<br />
<span>Նշվածները: {{ selected }}</span>
```

{% raw %}

<div id="example-6" class="demo">
  <select v-model="selected" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Նշվածները: {{ selected }}</span>
</div>
<script>
new Vue({
  el: '#example-6',
  data: {
    selected: []
  }
})
</script>
{% endraw %}

Դինամիկ ընտրանքներ render են եղած `v-for`-ով․

```html
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Նշվածները: {{ selected }}</span>
```

```js
new Vue({
  el: "...",
  data: {
    selected: "A",
    options: [
      { text: "Մեկ", value: "A" },
      { text: "Երկու", value: "B" },
      { text: "Երեք", value: "C" },
    ],
  },
});
```

{% raw %}

<div id="example-7" class="demo">
  <select v-model="selected">
    <option v-for="option in options" v-bind:value="option.value">
      {{ option.text }}
    </option>
  </select>
  <span>Նշվածը: {{ selected }}</span>
</div>
<script>
new Vue({
  el: '#example-7',
  data: {
    selected: 'A',
    options: [
      { text: 'Մեկ', value: 'A' },
      { text: 'Երկու', value: 'B' },
      { text: 'Երեք', value: 'C' }
    ]
  }
})
</script>
{% endraw %}

## Արժեքի Կապումները

Radio, checkbox և select ընտրանքները, `v-model`-ին կապված արժեքները սովորականում ստատիկ string-ներ են լինում (կամ boolean-ներ checkbox-երի համար)․

```html
<!-- `picked`-ը string է երբ «a»-ն նշված է -->
<input type="radio" v-model="picked" value="a" />

<!-- `toggle`-ը կամ true է կամ false -->
<input type="checkbox" v-model="toggle" />

<!-- `selected`-ը string է «abc» երբ առաջին ընտրանքն է նշված -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

Բայց երբեմն, մենք ցանկանում ենք կապել արժեքը դինամիկ հատկույանը Vue instance-ում։ Մենք կարող ենք օգտագործել `v-bind`-ը դրա համար։ Ի հավելումն, օգտագործելով `v-bind` թույլ է տալիս մեզ կապել input-ի արժեքը ոչ string արժեքներին։

### Checkbox

```html
<input type="checkbox" v-model="toggle" true-value="yes" false-value="no" />
```

```js
// երբ նշված է․
vm.toggle === "yes";
// երբ նշված չէ․
vm.toggle === "no";
```

<p class="tip">`true-value` և `false-value` ատրիբուտները չեն ազդում input-ի `value` ատրիբուտի վրա, որովհետև բրաուզերները չեն ներառում չնշված checkbox-երը երբ որ form-ը կիրառվում է։ Որպեսզի երաշխավորենք որ երկու արժեքներից մեկը կիրառված է form-ում (օրինակի համար «yes» կամ «no»), փոխարենը օգտագործեք radio input-ները։</p>

### Radio

```html
<input type="radio" v-model="pick" v-bind:value="a" />
```

```js
// when checked:
vm.pick === vm.a;
```

### Select-ի Option-ները

```html
<select v-model="selected">
  <!-- inline օբյեկտի գրառում -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```

```js
// երբ նշված է:
typeof vm.selected; // => 'object'
vm.selected.number; // => 123
```

## Փոփոխիչներ

### `.lazy`

Հիմնականում, `v-model`-ը sync է լինում input-ի տվյալների հետ ամեն `input` event-ի ժամանակ (բացառությամբ IME կիրառմամբ, ինչպես [նշված է վերևում](#vmodel-ime-tip))։ Դուք կարող եք ավելացնել `lazy` փոփոխիչը որպեսզի sync անել `change` event-ներից _հետո_։

```html
<!-- sync է եղած "change"-ից հետո "input"-ի փոխարեն -->
<input v-model.lazy="msg" />
```

### `.number`

Եթե դուք ցանկանում եք որպեսզի օգտագործողի կողմից մուտքագրածը վերածվի թվի, դուք կարող եք ավելացնել `number` փոփոխիչը ձեր `v-model`-ով կարգավորված input-ների վրա․

```html
<input v-model.number="age" type="number" />
```

Սա կարող է լինել օգտակար, որովհետև նույնիսկ `type="number"`-ի հետ, արժեքը HTML input էլեմենտների միշտ վերադարձնում է string: եթե արժեքը չի կարող parse լինել `parseFloat()`-ի հետ, ուրեմն օրիգինալ արժեքն է վերադարձվել։

### `.trim`

Եթե դուք ցանկանում եք բացատները օգտագործողի մուտքագրվածի մեջ ավտոմատ կերպով կտրվեն, դուք կարող եք ավելացնել `trim` փոփոխիչը ձեր `v-model`-ով կարգավորված input-ներին․

```html
<input v-model.trim="msg" />
```

## `v-model`-ը Կոմպոնենտների Հետ

> Եթե դուք ծանոթ չեք Vue-ի կոմպոնենտներին, դուք կարող եք բաց թողնել սա հիմա։

HTML-ի ներքում կառուցված մուտքագրման տիպերը միշտ չեն հանդիպի ձեր պահանջների հետ։ Բարեբախտորեն, Vue-ի կոմպոնենտները թույլ են տալիս ձեզ կառուցել վերօգտագործվող input-ներ ամբողջությամբ customize անելով իրենց behavior-ը: Այս input—ները նույնիսկ աշխատում են `v-model`-ի հետ!

Որպեսզի իմանալ ավելին, կարդացեք [custom input-ների](components.html#Using-v-model-on-Components) մասին կոմպոնենտների ուղեցույցում։
