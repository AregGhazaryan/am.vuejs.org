---
title: Ֆիլտրներ
type: ուղեցույց
order: 305
---

Vue.js-ը թույլ է տալիս ձեզ հայտարարել ֆիլտրներ որոնք կարող են օգտագործվել որպեսզի կիրառեն սովորական տեքստի ձևաչափում։ Ֆիլտրները կարելի է օգտագործել երկու տեղերում․ **ձևավոր փակագծերով և `v-bind` արտահայտություններում** (երկուսնել հասանելի են 2.0.0+ տարբերակներում)։ Ֆիլտրները պետք է կցվեն JavaScript-ի արտահայտության վերջում, նշված «խողովակի» նշանով (|).

```html
<!-- ձևավոր փակագծերում -->
{{ message | capitalize }}

<!-- v-bind-ում -->
<div v-bind:id="rawId | formatId"></div>
```

Դուք կարող եք հայտարարել լոկալ ֆիլտրներ կոմպոնենտի ընտրանքներում․

```js
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```

կամ հայտարարեք ֆիլտր գլոբալ կերպով նախքան Vue instance-ի ստեղծումը․

```js
Vue.filter("capitalize", function (value) {
  if (!value) return "";
  value = value.toString();
  return value.charAt(0).toUpperCase() + value.slice(1);
});

new Vue({
  // ...
});
```

Երբ գլոբալ ֆիլտրը ունի նույն անունը որպես լոկալ ֆիլտեր, լոկալ ֆիլտրը կնշվի։

Ներքևում օրինակ է մեր `capitalize` ֆիլտերի օգտագործման համար․

{% raw %}

<div id="example_1" class="demo">
  <input type="text" v-model="message">
  <p>{{ message | capitalize }}</p>
</div>
<script>
  new Vue({
    el: '#example_1',
    data: function () {
      return {
        message: 'john'
      }
    },
    filters: {
      capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
      }
    }
  })
</script>
{% endraw %}

Ֆիլտերի ֆունկցիան միշտ ստանում է արտահայտության արժեքը (նախկինում գրված շղթային արդյունքը) որպես իր առաջին արգումենտ։ Վերևի օրինակում, `capitalize` ֆիլտր ֆունկցիան կստանա `message`-ի արժեքը որպես արգումենտ։

Ֆիլտրները կարող են շղթայվել․

```html
{{ message | filterA | filterB }}
```

Այս դեպքում, `filterA`-ն, որը հայտարարված է մեկ արգումենտով, կստանա `message`-ի արժեքը, և անյուհետև `filterB` ֆունկցիան կկանչվի `filterA`-ի արժեքով փոխանցված `filterB`-ի մեկ արգումենտում։

Ֆիլտրները JavaScript ֆունկցիաներ են, որից դատելով նրանք կարող են ստանալ արգումենտներ․

```html
{{ message | filterA('arg1', arg2) }}
```

Այստեղ `filterA`-ն հայտարարված է որպես ֆունկցիան ստանալով երեք արգումենտներ։ `message`-ի արժեքը կփոխանցվի առաջին արգումենտին։ Պարզ string ՝«arg1»`-ը կփոխանցվի`filterA`-ին որպես իր երկրորդ արգումենտ, և`arg` արտահայտության արժեքը կհաշվարկվի և կփոխանցվի որպես երորդ արգումենտ։
