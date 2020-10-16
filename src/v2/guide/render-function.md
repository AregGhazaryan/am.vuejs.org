---
title: Render Ֆունկցիաներ և JSX
type: ուղեցույց
order: 303
---

## Հիմքեր

Vue֊ն խորհուրդ է տալիս օգտագործել templates որպեսզի կառուցել ձեր HTML֊ը շատ դեպքերում։ Սակայն կան դեպքեր, երբ որ դուք պետք է օգտագործեք JavaScript֊ի ամբողջ ծրագրային ուժը։ Այստեղ դուք կարող եք օգտագործել **render ֆունկցիան**, compiler֊ին մոտիկ այլընտրանք template֊ների համար։

Եկեք սուզվենք դեպի սովորական օրինակը որտեղ`render` ֆունկցիան կլինի պրակտիկ։ Պատկերացրեք որ դուք ցանկանում եք գեներացնել heading֊ով հղումներ․

``` html
<h1>
  <a name="hello-world" href="#hello-world">
    Բարև Աշխարհ!
  </a>
</h1>
```

Վերևի HTML֊ի համար, դուք կորոշեք թե ցանկանում եք այս կոմպոնենտի ինտերֆեյսը․

``` html
<anchored-heading :level="1">Hello world!</anchored-heading>
```

Երբ որ դուք սկսում եք կոմպոնենտով այն միայն գեներացնում է heading կախված `level` prop֊ից, դուք արագորեն հասնում եք հետևյալին․

``` html
<script type="text/x-template" id="anchored-heading-template">
  <h1 v-if="level === 1">
    <slot></slot>
  </h1>
  <h2 v-else-if="level === 2">
    <slot></slot>
  </h2>
  <h3 v-else-if="level === 3">
    <slot></slot>
  </h3>
  <h4 v-else-if="level === 4">
    <slot></slot>
  </h4>
  <h5 v-else-if="level === 5">
    <slot></slot>
  </h5>
  <h6 v-else-if="level === 6">
    <slot></slot>
  </h6>
</script>
```

``` js
Vue.component('anchored-heading', {
  template: '#anchored-heading-template',
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```

Այդ ձևանմուշը լավը չէ։ Այն ոչ միայն բառալի է, բայց նաև կրկնօրինակում է `<slot></slot>`֊ը ամեն heading֊ի աստիճանի համար և պետք է կրկնենք նույն բանը ամեն հղման էլեմենտի համար։

Երբ ձևանմուշները աշխատում են լավ հիմնական կոմպոնենտների համար, շատ պարզ է որ սա նրանցից մեկը չէ։ Այնպես որ փորձենք վերագրել այն `render` ֆունկցիայով․

``` js
Vue.component('anchored-heading', {
  render: function (createElement) {
    return createElement(
      'h' + this.level,   // tag֊ի անունը
      this.$slots.default // ժառանգողների զանգված
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```

Ավելի պարզ! Որոշ չափով։ Կոդը ավելի կարճ է, բայց այն պահանջում է բավականին մեծ ծանոթություն Vue֊ի instance֊ի հատկությունների հետ։ Այս դեպքում, մենք պետք է իմանանք որ երբ որ փոխանցում ենք ժառանգողներին առանց `v-slot` ուղղորդիչի դեպի կոմպոնենտ, ինչպես `Բարև Աշխարհ!` է `anchored-heading`֊ի ներսում, այս ժառանգողները տեղադրված են կոմպոնենտի instance֊ում ավելի կոնկրետ `$slots.default`֊ում։ Եթե դուք չեք կարդացել, **խորհուրդ է տրվում ծանոթանալ [instance հատկությունների API](../api/#Instance-Properties)֊ի հետ նախքան render ֆունկցիաների մեջ սուզվելը։**

## Node֊եր, Ծառեր, և Virtual DOM

Նախքան սուզվենք դեպի render ֆունկցիաներ, կարևոր է իմանալ միքիչ թէ ինչպես են բրաուզերները աշխատում։ Վերցեք այս HTML֊ը օրինակի համար․

```html
<div>
  <h1>My title</h1>
  Some text content
  <!-- TODO: Add tagline  -->
</div>
```

Երբ բրաուզերը կարդում է այս կոդը, այն կառուցում է [«DOM node֊երի» ծառ](https://javascript.info/dom-nodes) որպեսզի օգնի հետևողական լինել ամեն ինչին, ինչպես որ դուք կկառուցեք ընտանիքի ծառ որպեսզի հետևողական լինել ձեր շարունակվող սերունդներին։

DOM node֊երի ծառը HTML֊ի համար նշված վերևում այսպիսի տեսք ունի»

![DOM Ծառի Վիզուալիզացիա](/images/dom-tree.png)

Ամեն էլեմենտը node է։ Ամեն մասը տեքստի node է։ Նույնիսկ comment֊ները node֊եր են! node֊ը պարզապես մի մասն է էջի։ ԵՎ ինչպես ընտանիքի ծառն է, ամեն node֊ը կարող է ունենալ ժառանգողներ (օրինակ՝ ամեն մասնիկ կարող է պարունակել այլ մասնիկներ)։

Թարմացնելով բոլոր այս node֊երը արդյունավետորեն կարող է լինել դժվար, բայց բարեբախտորեն, դուք չպետք է այն ձեռքով անեք։ Փոխարենը, դուք պետք է տեղյակ պահեք Vue֊ի թե ինչ HTML էք դուք ցանկանում էջի վրա, template֊ում․

```html
<h1>{{ blogTitle }}</h1>
```

Կամ render ֆունկցիա․

``` js
render: function (createElement) {
  return createElement('h1', this.blogTitle)
}
```

և երկու դեպքերում էլ, Vue ավտոմատ կերպով էջը թարմացված է պահում, նույնիսկ եթե `blogTitle`֊ը փոխվում է։

### Virtual DOM

Vue իրականացնում է սա կառուցելով **virtual DOM** որպեսզի հետևի փոփոխությունների որպեսզի այն տանի դեպի իրական DOM։ Զննելով հետևյալ տողը․

``` js
return createElement('h1', this.blogTitle)
```

Ի՞նչ է `createElement`֊ը իրոք վերադարձնում։ Այն _կոնկրետ_ իրական DOM էլեմենտ չէ։ Այն հնարավոր է ավելի կոնկրետ անվանվի `createNodeDescription`, որովհետև այն պարունակում է ինֆորմացիա նկարագրող Vue֊ին թե ինչ տիպի node պետք է render լինի էջում, ներառելով նկարագրությունը ցանկացած child node֊երի։ Մենք կանվանենք այս node֊ի նկարագրությունը "virtual node", կրճատ **VNode**։ "Virtual DOM"֊ը այն է ինչ որ անվանում ենք ամբող VNode֊երի ծառին, կառուցված Vue կոմպոնենտների ծառով։

## `createElement` Արգումենտները

Հաջորդը դուք պետք է ծանոթանաք թե ինչպես օգտագործել template֊ի հատկությունները `createElement` ֆունկցիայում։ Այստեղ կարող եք տեսնել այն արգումենտները որոնք `createElement`֊ը ընդհունում է․

``` js
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // HTML tag֊ի անուն, կոմպոնենտի ընտրանք, կամ async
  // ֆունկցիա վերադարձնող նրացնից մեկը։ Պահանջվում է։
  'div',

  // {Object}
  // Տվյալների օբյեկտ համապատասխանող ատրիբուտներին
  // որոնց դուք կարող էք օգտագործել template֊ում։ Պարտադիր չէ։
  {
    // (նայեք մանրամասները հաջորդ բաժնում որը գտնվում է ներքևում)
  },

  // {String | Array}
  // Ժառանգող VNode֊եր, կառուցված օգտագործելով `createElement()`,
  // կամ օգտագործելով string֊ներ որպեսզի ստանալ text VNodes'֊ը։ Պարտադիր չէ։
  [
    'Որևէ տեքստ որը գալիս է սկզբում։',
    createElement('h1', 'A headline'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)
```

### Տվյալների Օբյեկտը Մանրամասն

Մեկ բան է պետք նշել․ նման լինելով թե ինչպես `v-bind:class`֊ը և `v-bind:style` ունեն հատուկ վերլուծումներ ձևանմուշներում, նրանք ունեն իրենց բարձր կարգի դաշտերը VNode տվյալների օբյեկտում։ Այս օբյեկտը նաև թույլ է տալիս ձեզ կապելու սովորական HTML ատրիբուտներ ինչպես նաև DOM հատկություններ ինչպիսին `innerHTML` է (սա կփոխարինվի `v-html` ուղղորդիչը)․

``` js
{
  // Նույն API֊ը ինչ `v-bind:class`֊ն է, ընդհունելով կամ
  // string, օբյեկտ, կամ զանգված կազմված string֊ներից կամ օբյեկտներից։
  class: {
    foo: true,
    bar: false
  },
  // Նույն API֊ը ինչ `v-bind:style`֊ն է, ընդհունելով կամ
  // string, օբյեկտ, կամ զանգված կազմված օբյեկտներից։
  style: {
    color: 'red',
    fontSize: '14px'
  },
  // Սովորական HTML ատրիբուտներ
  attrs: {
    id: 'foo'
  },
  // Կոմպոնենտի prop֊ներ
  props: {
    myProp: 'bar'
  },
  // DOM հատկություններ
  domProps: {
    innerHTML: 'baz'
  },
  // Event handler֊ները nest են եղած `on`֊ի տակ, չնայած
  // փոփոխիչները ինչպիսին `v-on:keyup.enter`֊ն է այլևս չեն
  // համապատասխանում։ Փոխարենը դուք պետք է ձեռքով ստուգեք
  // keyCode֊ը handler֊ում։
  on: {
    click: this.clickHandler
  },
  // Միայն կոմպոնենտների համար։ Թույլ է տալիս ձեզ լսելու 
  // ներքին event֊ներին, քան այն event֊ներին որոնք արձակվել են
  // կոմպոնենտից օգտագործելով `vm.$emit`֊ը։
  nativeOn: {
    click: this.nativeClickHandler
  },
  // Custom ուղղորդիչներ։ Նշում որ `կապումների`
  // `հին արժեքը` չի կարող տեղադրվել, որովհետև Vue֊ն հետևում փոփոխություններին
  // ձեր փոխարեն։
  directives: [
    {
      name: 'my-custom-directive',
      value: '2',
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ],
  // Scope եղած սլոտները հետևյալ ձևով
  // { name: props => VNode | Array<VNode> }
  scopedSlots: {
    default: props => createElement('span', props.text)
  },
  // Անունը սլոտի, եթե կոմպոնենտը 
  // ժառանգում է այլ կոմպոնենտից
  slot: 'name-of-slot',
  // Այլ հատուկ կարգի աստիճանի հատկություններ
  key: 'myKey',
  ref: 'myRef',
  // Եթե դուք կիրառում էք նույն ref անունը մի քանի
  // էլեմենտներին render ֆունկցիայում։ Սա կդարձնի `$refs.myRef` զանգված։
  refInFor: true
}
```

### Ամբողջական Օրինակ

Այս գիտելիքներով, մենք կարող ենք ավարտել կոմպոնենտը որը սկսել ենք․

``` js
var getChildrenTextContent = function (children) {
  return children.map(function (node) {
    return node.children
      ? getChildrenTextContent(node.children)
      : node.text
  }).join('')
}

Vue.component('anchored-heading', {
  render: function (createElement) {
    // ստեղծեք kebab-case id
    var headingId = getChildrenTextContent(this.$slots.default)
      .toLowerCase()
      .replace(/\W+/g, '-')
      .replace(/(^-|-$)/g, '')

    return createElement(
      'h' + this.level,
      [
        createElement('a', {
          attrs: {
            name: headingId,
            href: '#' + headingId
          }
        }, this.$slots.default)
      ]
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```

### Սահմանափակումները

#### VNode֊երը Պետք է Լինեն Հատուկ

Բոլոր VNode֊երը կոմպոնենտի ծառում պետք է լինեն հատուկ։ Սա նշանակում է որ հետևյալ render ֆունկցիան սխալ է․

``` js
render: function (createElement) {
  var myParagraphVNode = createElement('p', 'hi')
  return createElement('div', [
    // Սխալ - կրկնօրինակված VNode֊ներ՛
    myParagraphVNode, myParagraphVNode
  ])
}
```

Եթե դուք իրոք ցանկանում եք կրկնօրինակել նույն էլեմենտ/կոմպոնենտը մի քանի անգամ, դուք կարող եք օգտագործել factory ֆունկցիա։ Օրինակի համար, հետևյալ render ֆունկցիան ճիշտ է 20 հատ նույնականացված պարբերություններ render անելու համար․

``` js
render: function (createElement) {
  return createElement('div',
    Array.apply(null, { length: 20 }).map(function () {
      return createElement('p', 'hi')
    })
  )
}
```

## Template֊ի Փոխարինումը Հասարակ JavaScript֊ով

### `v-if` և `v-for`

Երբեմն ինչ որ մի բան կարող է ավելի հեշտ ստացվել հասարակ JavaScript֊ում, Vue֊ի render ֆունկցիաները չեն տրամադրում ուղիղ այլընտրանք։ Օրինակի համար, ձևանմուշում օգտագործելով `v-if`֊ը և `v-for`֊ը․

``` html
<ul v-if="items.length">
  <li v-for="item in items">{{ item.name }}</li>
</ul>
<p v-else>Ոչինչ չգտնվեց.</p>
```

Սա կարող է վերագրվել JavaScript֊ի օգնությամբ օգտագործելով `if`/`else` և `map` render ֆունկցիայում․

``` js
props: ['items'],
render: function (createElement) {
  if (this.items.length) {
    return createElement('ul', this.items.map(function (item) {
      return createElement('li', item.name)
    }))
  } else {
    return createElement('p', 'Ոչինչ չգտնվեց.')
  }
}
```

### `v-model`

Ուղիղ այլընտրանք չկա `v-model`֊ի համար render ֆունկցիաներում - դուք պետք է տեղադրեք այդ տրաբանությունը ինքներտ․

``` js
props: ['value'],
render: function (createElement) {
  var self = this
  return createElement('input', {
    domProps: {
      value: self.value
    },
    on: {
      input: function (event) {
        self.$emit('input', event.target.value)
      }
    }
  })
}
```

Սա է ցածր կարգի անցնելու ծախսը, բայց այն նաև տրամադրում է ավելի շատ կառավարում ի համեմատ `v-model`-ի։

### Event և Key Փոփոխիչներ

`.passive`, `.capture` և `.once` event֊ի փոփոխիչների համար, Vue֊ն տրամադրում է prefix֊ներ ուոնք կարող են օգտագործվել `on`֊ի հետ․

| Փոփոխիչ(ներ) | Prefix |
| ------ | ------ |
| `.passive` | `&` |
| `.capture` | `!` |
| `.once` | `~` |
| `.capture.once` կամ<br>`.once.capture` | `~!` |

Օրինակի համար․

```javascript
on: {
  '!click': this.doThisInCapturingMode,
  '~keyup': this.doThisOnce,
  '~!mouseover': this.doThisOnceInCapturingMode
}
```

Բոլոր այլ event և key փոփոխիչների համար, prefix անհրաժեշտ չէ, որովհետև դուք կարող օգտագործել event֊ի մեթոդները handler֊ում․

| Փոփոխիչ(ներ) | Handler֊ում  |
| ------ | ------ |
| `.stop` | `event.stopPropagation()` |
| `.prevent` | `event.preventDefault()` |
| `.self` | `if (event.target !== event.currentTarget) return` |
| Key֊եր:<br>`.enter`, `.13` | `if (event.keyCode !== 13) return` (փոխեք `13`֊ը դեպի [մեկ այլ key code](http://keycode.info/) այլ key modifier֊ների համար) |
| Փոփոխիչների Key֊եր:<br>`.ctrl`, `.alt`, `.shift`, `.meta` | `if (!event.ctrlKey) return` (փոխեք `ctrlKey` դեպի `altKey`, `shiftKey`, կամ `metaKey`, համապտասխանաբար) |

Այստեղ օրինակն է բոլոր փոփոխիչները միաժամանակ օգտագործված․

```javascript
on: {
  keyup: function (event) {
    // Կանխել եթե էլեմենտը որը արձակում է event֊ը 
    // այլ էլեմենտը չէ որին որ event֊ը կապված է
    if (event.target !== event.currentTarget) return
    // Կանխել եթե կոճակը enter֊ը չէ
    // key (13) և shift կոճակը չի սեղմված եղել
    // նույն ժամանակ
    if (!event.shiftKey || event.keyCode !== 13) return
    // Կանգնացնել event֊ի propagation֊ը
    event.stopPropagation()
    // Կանխում է հիմնական keyup handler֊ը այս էլեմենտի համար
    event.preventDefault()
    // ...
  }
}
```

### Սլոտները

Դուք կարող էք մուտք գործել դեպի ստատիկ սլոտի բովանդակություններ ինչպիսին են Զանգվածները VNode֊երի [`this.$slots`֊ից](../api/#vm-slots)․

``` js
render: function (createElement) {
  // `<div><slot></slot></div>`
  return createElement('div', this.$slots.default)
}
```

ԵՎ մուտք գործել scope֊ված սլոտներ որպես ֆունկցիաներ որոնք վերադարձնում են VNode֊եր [`this.$scopedSlots`֊ի](../api/#vm-scopedSlots)․

``` js
props: ['message'],
render: function (createElement) {
  // `<div><slot :text="message"></slot></div>`
  return createElement('div', [
    this.$scopedSlots.default({
      text: this.message
    })
  ])
}
```

Որպեսզի փոխանցել scope֊ված սլոտներ ժառանգող կոմպոնենտին օգտագործելով render ֆունկցիաներ, օգտագործեք `scopedSlots` դաշտը VNode տվյալում

``` js
render: function (createElement) {
  // `<div><child v-slot="props"><span>{{ props.text }}</span></child></div>`
  return createElement('div', [
    createElement('child', {
      // pass `scopedSlots` in the data object
      // in the form of { name: props => VNode | Array<VNode> }
      scopedSlots: {
        default: function (props) {
          return createElement('span', props.text)
        }
      }
    })
  ])
}
```

## JSX

Եթե դուք գրում եք բավականին շատ `render` ֆունկցիաներ, կարող է լինել ցավոտ այսպիսի բան գրելը․

``` js
createElement(
  'anchored-heading', {
    props: {
      level: 1
    }
  }, [
    createElement('span', 'Hello'),
    ' world!'
  ]
)
```

Հատկապես երբ template֊ի version֊ը շատ պարզ է համեմատաբար․

``` html
<anchored-heading :level="1">
  <span>Hello</span> world!
</anchored-heading>
```

Այդ պատճառով գոյություն ունի [Babel plugin](https://github.com/vuejs/jsx) որպեսզի օգտագործել JSX Vue֊ի հետ, ետ տանելով մեզ գրելաձև որը նման է template֊ներին․

``` js
import AnchoredHeading from './AnchoredHeading.vue'

new Vue({
  el: '#demo',
  render: function (h) {
    return (
      <AnchoredHeading level={1}>
        <span>Բարև</span> աշխարհ!
      </AnchoredHeading>
    )
  }
})
```

<p class="tip">Փոխանուն ստեղծելով `createElement`֊ից դեպի `h` շատ հազվադեպ է պատահում Vue֊ի էկոհամակարգում և հատկապես պահանջվում է JSX֊ի կողղմից։ Սկսելով [3.4.0 տարբերակով](https://github.com/vuejs/babel-plugin-transform-vue-jsx#h-auto-injection) Babel plugin֊ից Vue֊ի համար, մենք ավտոմատ կերպով ներարկում ենք `const h = this.$createElement` ցանկացած մեթոդում և getter֊ում (ոչ ֆունկցիաներում կամ աղեղով ֆունկցիաներում), հայտարարված ES2015 գրելաձևով որը ունի JSX, այնպես որ դուք կարող էջ չօգտագործել `(h)` պարամետրը։ Plugin֊ի նախկին տարբերակներում, ձեև ծրագրիը կնետի error եթե `h`֊ը հասանելի չէր scope֊ում։</p>

For more on how JSX maps to JavaScript, see the [usage docs](https://github.com/vuejs/jsx#installation).

## Functional Components

The anchored heading component we created earlier is relatively simple. It doesn't manage any state, watch any state passed to it, and it has no lifecycle methods. Really, it's only a function with some props.

In cases like this, we can mark components as `functional`, which means that they're stateless (no [reactive data](../api/#Options-Data)) and instanceless (no `this` context). A **functional component** looks like this:

``` js
Vue.component('my-component', {
  functional: true,
  // Props are optional
  props: {
    // ...
  },
  // To compensate for the lack of an instance,
  // we are now provided a 2nd context argument.
  render: function (createElement, context) {
    // ...
  }
})
```

> Note: in versions before 2.3.0, the `props` option is required if you wish to accept props in a functional component. In 2.3.0+ you can omit the `props` option and all attributes found on the component node will be implicitly extracted as props.
> 
> The reference will be HTMLElement when used with functional components because they’re stateless and instanceless.

In 2.5.0+, if you are using [single-file components](single-file-components.html), template-based functional components can be declared with:

``` html
<template functional>
</template>
```

Everything the component needs is passed through `context`, which is an object containing:

- `props`: An object of the provided props
- `children`: An array of the VNode children
- `slots`: A function returning a slots object
- `scopedSlots`: (2.6.0+) An object that exposes passed-in scoped slots. Also exposes normal slots as functions.
- `data`: The entire [data object](#The-Data-Object-In-Depth), passed to the component as the 2nd argument of `createElement`
- `parent`: A reference to the parent component
- `listeners`: (2.3.0+) An object containing parent-registered event listeners. This is an alias to `data.on`
- `injections`: (2.3.0+) if using the [`inject`](../api/#provide-inject) option, this will contain resolved injections.

After adding `functional: true`, updating the render function of our anchored heading component would require adding the `context` argument, updating `this.$slots.default` to `context.children`, then updating `this.level` to `context.props.level`.

Since functional components are just functions, they're much cheaper to render.

They're also very useful as wrapper components. For example, when you need to:

- Programmatically choose one of several other components to delegate to
- Manipulate children, props, or data before passing them on to a child component

Here's an example of a `smart-list` component that delegates to more specific components, depending on the props passed to it:

``` js
var EmptyList = { /* ... */ }
var TableList = { /* ... */ }
var OrderedList = { /* ... */ }
var UnorderedList = { /* ... */ }

Vue.component('smart-list', {
  functional: true,
  props: {
    items: {
      type: Array,
      required: true
    },
    isOrdered: Boolean
  },
  render: function (createElement, context) {
    function appropriateListComponent () {
      var items = context.props.items

      if (items.length === 0)           return EmptyList
      if (typeof items[0] === 'object') return TableList
      if (context.props.isOrdered)      return OrderedList

      return UnorderedList
    }

    return createElement(
      appropriateListComponent(),
      context.data,
      context.children
    )
  }
})
```

### Passing Attributes and Events to Child Elements/Components

On normal components, attributes not defined as props are automatically added to the root element of the component, replacing or [intelligently merging with](class-and-style.html) any existing attributes of the same name.

Functional components, however, require you to explicitly define this behavior:

```js
Vue.component('my-functional-button', {
  functional: true,
  render: function (createElement, context) {
    // Transparently pass any attributes, event listeners, children, etc.
    return createElement('button', context.data, context.children)
  }
})
```

By passing `context.data` as the second argument to `createElement`, we are passing down any attributes or event listeners used on `my-functional-button`. It's so transparent, in fact, that events don't even require the `.native` modifier.

If you are using template-based functional components, you will also have to manually add attributes and listeners. Since we have access to the individual context contents, we can use `data.attrs` to pass along any HTML attributes and `listeners` _(the alias for `data.on`)_ to pass along any event listeners.

```html
<template functional>
  <button
    class="btn btn-primary"
    v-bind="data.attrs"
    v-on="listeners"
  >
    <slot/>
  </button>
</template>
```

### `slots()` vs `children`

You may wonder why we need both `slots()` and `children`. Wouldn't `slots().default` be the same as `children`? In some cases, yes - but what if you have a functional component with the following children?

``` html
<my-functional-component>
  <p v-slot:foo>
    first
  </p>
  <p>second</p>
</my-functional-component>
```

For this component, `children` will give you both paragraphs, `slots().default` will give you only the second, and `slots().foo` will give you only the first. Having both `children` and `slots()` therefore allows you to choose whether this component knows about a slot system or perhaps delegates that responsibility to another component by passing along `children`.

## Template Compilation

You may be interested to know that Vue's templates actually compile to render functions. This is an implementation detail you usually don't need to know about, but if you'd like to see how specific template features are compiled, you may find it interesting. Below is a little demo using `Vue.compile` to live-compile a template string:

<iframe src="https://codesandbox.io/embed/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-template-compilation?codemirror=1&hidedevtools=1&hidenavigation=1&theme=light&view=preview" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" title="vue-20-template-compilation" allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>
