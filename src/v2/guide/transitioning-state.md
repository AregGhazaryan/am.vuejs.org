---
title: State-ի Transition-ները
type: ուղեցույց
order: 202
---

Vue-ի անցումային համակարգը տրամադրում է բազում հասարակ ճանապարհներ որպեսզի կառավարել անիմացիային մոտւքը, ելքը և ցանկը, իսկ ի՞նչ վերաբերյալ տվյալների անիմացիայի հետ․ Օրինակի համար․

- թվերը և հաշվարկները
- ցույց տրվող գույները
- SVG node-երի դիրքերը
- չափսերը և այլ հատկությունները էլեմենտների

Բոլորը սրանցից նախորոք արդեն տեղադրված են որպես հասարակ թվեր կամ կարող են վերափոխվել դեպի թվեր։ Այդ ամենից հետո, մենք կարող ենք անիմացիայի ենթարկել այս state-ի փոփոխությունները օգտագործելով 3-րդ կողմի գրադարաններ որպեսզի կապել state-ը, համախմբված Vue-ի ռեակտիվության և կոմպոնենտի համակարգների հետ։

## State-ի Անիմացիան Watcher-ներով

Watcher-ները թույլ են տալիս մեզ անիմացիայի ենթարկել փոփոխությունները ցանկացած թվային հատկության դեպի այլ հատկություն։ Դա կարող է հնչել բավականին բարդ, այնպես որ եկեք սուզվենք դեպի օրինակը օգտագործելով [GreenSock](https://greensock.com/)․

``` html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.2.4/gsap.min.js"></script>

<div id="animated-number-demo">
  <input v-model.number="number" type="number" step="20">
  <p>{{ animatedNumber }}</p>
</div>
```

``` js
new Vue({
  el: '#animated-number-demo',
  data: {
    number: 0,
    tweenedNumber: 0
  },
  computed: {
    animatedNumber: function() {
      return this.tweenedNumber.toFixed(0);
    }
  },
  watch: {
    number: function(newValue) {
      gsap.to(this.$data, { duration: 0.5, tweenedNumber: newValue });
    }
  }
})
```

{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.2.4/gsap.min.js"></script>
<div id="animated-number-demo" class="demo">
  <input v-model.number="number" type="number" step="20">
  <p>{{ animatedNumber }}</p>
</div>
<script>
new Vue({
  el: '#animated-number-demo',
  data: {
    number: 0,
    tweenedNumber: 0
  },
  computed: {
    animatedNumber: function() {
      return this.tweenedNumber.toFixed(0);
    }
  },
  watch: {
    number: function(newValue) {
      gsap.to(this.$data, { duration: 0.5, tweenedNumber: newValue });
    }
  }
})
</script>
{% endraw %}

Երբ դուք կթարմացնեք թիվը, փոփոխությունը երևում է ներքևի դաշտում։ Սա դառնում է լավ օրինակ, բայց ի՞նչ եթե վերաբերվում է որևէ բանի որը ուղիղ կերպով չի գրանցվել որպես թիվ, ցանկացած ճիշտ CSS գույնի նման օրինակի համար։ Այստեղ կարող էք նայել թէ ինչպես հասնել այդ արդյունքին օգտագործելով [Tween.js-ը](https://github.com/tweenjs/tween.js) և [Color.js-ը](https://github.com/brehaut/color-js)․

``` html
<script src="https://cdn.jsdelivr.net/npm/tween.js@16.3.4"></script>
<script src="https://cdn.jsdelivr.net/npm/color-js@1.0.3"></script>

<div id="example-7">
  <input
    v-model="colorQuery"
    v-on:keyup.enter="updateColor"
    placeholder="Enter a color"
  >
  <button v-on:click="updateColor">Update</button>
  <p>Preview:</p>
  <span
    v-bind:style="{ backgroundColor: tweenedCSSColor }"
    class="example-7-color-preview"
  ></span>
  <p>{{ tweenedCSSColor }}</p>
</div>
```

``` js
var Color = net.brehaut.Color

new Vue({
  el: '#example-7',
  data: {
    colorQuery: '',
    color: {
      red: 0,
      green: 0,
      blue: 0,
      alpha: 1
    },
    tweenedColor: {}
  },
  created: function () {
    this.tweenedColor = Object.assign({}, this.color)
  },
  watch: {
    color: function () {
      function animate () {
        if (TWEEN.update()) {
          requestAnimationFrame(animate)
        }
      }

      new TWEEN.Tween(this.tweenedColor)
        .to(this.color, 750)
        .start()

      animate()
    }
  },
  computed: {
    tweenedCSSColor: function () {
      return new Color({
        red: this.tweenedColor.red,
        green: this.tweenedColor.green,
        blue: this.tweenedColor.blue,
        alpha: this.tweenedColor.alpha
      }).toCSS()
    }
  },
  methods: {
    updateColor: function () {
      this.color = new Color(this.colorQuery).toRGB()
      this.colorQuery = ''
    }
  }
})
```

``` css
.example-7-color-preview {
  display: inline-block;
  width: 50px;
  height: 50px;
}
```

{% raw %}
<script src="https://cdn.jsdelivr.net/npm/tween.js@16.3.4"></script>
<script src="https://cdn.jsdelivr.net/npm/color-js@1.0.3"></script>
<div id="example-7" class="demo">
  <input
    v-model="colorQuery"
    v-on:keyup.enter="updateColor"
    placeholder="Enter a color"
  >
  <button v-on:click="updateColor">Թարմացնել</button>
  <p>Արդյունքը:</p>
  <span
    v-bind:style="{ backgroundColor: tweenedCSSColor }"
    class="example-7-color-preview"
  ></span>
  <p>{{ tweenedCSSColor }}</p>
</div>
<script>
var Color = net.brehaut.Color
new Vue({
  el: '#example-7',
  data: {
    colorQuery: '',
    color: {
      red: 0,
      green: 0,
      blue: 0,
      alpha: 1
    },
    tweenedColor: {}
  },
  created: function () {
    this.tweenedColor = Object.assign({}, this.color)
  },
  watch: {
    color: function () {
      function animate () {
        if (TWEEN.update()) {
          requestAnimationFrame(animate)
        }
      }

      new TWEEN.Tween(this.tweenedColor)
        .to(this.color, 750)
        .start()

      animate()
    }
  },
  computed: {
    tweenedCSSColor: function () {
      return new Color({
        red: this.tweenedColor.red,
        green: this.tweenedColor.green,
        blue: this.tweenedColor.blue,
        alpha: this.tweenedColor.alpha
      }).toCSS()
    }
  },
  methods: {
    updateColor: function () {
      this.color = new Color(this.colorQuery).toRGB()
      this.colorQuery = ''
    }
  }
})
</script>
<style>
.example-7-color-preview {
  display: inline-block;
  width: 50px;
  height: 50px;
}
</style>
{% endraw %}

## Դինամիկ State-ի Անցումները

Vue-ի անցումային կոմպոնենտներում, տվյալները որից կախված է state-ի անցումները կարող են թարմացվել ռեալ ժամանակում, որը հատկապես օգտակար է prototyping-ի համար՛ Նույնիսկ օգտագործելով հասարակ SVG polygon, դուք կարող եք հասնել լիքը էֆեկտների որոնց լիարժեք ստացումը կլինի բավականին բարդ մինչ դուք խախում էք փոփոխականների հետ մի փոքր։

{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/1.18.5/TweenLite.min.js"></script>
<div id="svg-polygon-demo" class="demo">
  <svg width="200" height="200" class="demo-svg">
    <polygon :points="points" class="demo-polygon"></polygon>
    <circle cx="100" cy="100" r="90" class="demo-circle"></circle>
  </svg>
  <label>Կողմերը: {{ sides }}</label>
  <input
    class="demo-range-input"
    type="range"
    min="3"
    max="500"
    v-model.number="sides"
  >
  <label>Մինիմում Շառավիղը: {{ minRadius }}%</label>
  <input
    class="demo-range-input"
    type="range"
    min="0"
    max="90"
    v-model.number="minRadius"
  >
  <label>Թարմացման Տեմպը: {{ updateInterval }} milliseconds</label>
  <input
    class="demo-range-input"
    type="range"
    min="10"
    max="2000"
    v-model.number="updateInterval"
  >
</div>
<script>
new Vue({
  el: '#svg-polygon-demo',
  data: function () {
    var defaultSides = 10
    var stats = Array.apply(null, { length: defaultSides })
      .map(function () { return 100 })
    return {
      stats: stats,
      points: generatePoints(stats),
      sides: defaultSides,
      minRadius: 50,
      interval: null,
      updateInterval: 500
    }
  },
  watch: {
    sides: function (newSides, oldSides) {
      var sidesDifference = newSides - oldSides
      if (sidesDifference > 0) {
        for (var i = 1; i <= sidesDifference; i++) {
          this.stats.push(this.newRandomValue())
        }
      } else {
        var absoluteSidesDifference = Math.abs(sidesDifference)
        for (var i = 1; i <= absoluteSidesDifference; i++) {
          this.stats.shift()
        }
      }
    },
    stats: function (newStats) {
      TweenLite.to(
        this.$data,
        this.updateInterval / 1000,
        { points: generatePoints(newStats) }
      )
    },
    updateInterval: function () {
      this.resetInterval()
    }
  },
  mounted: function () {
    this.resetInterval()
  },
  methods: {
    randomizeStats: function () {
      var vm = this
      this.stats = this.stats.map(function () {
        return vm.newRandomValue()
      })
    },
    newRandomValue: function () {
      return Math.ceil(this.minRadius + Math.random() * (100 - this.minRadius))
    },
    resetInterval: function () {
      var vm = this
      clearInterval(this.interval)
      this.randomizeStats()
      this.interval = setInterval(function () {
        vm.randomizeStats()
      }, this.updateInterval)
    }
  }
})

function valueToPoint (value, index, total) {
  var x     = 0
  var y     = -value * 0.9
  var angle = Math.PI * 2 / total * index
  var cos   = Math.cos(angle)
  var sin   = Math.sin(angle)
  var tx    = x * cos - y * sin + 100
  var ty    = x * sin + y * cos + 100
  return { x: tx, y: ty }
}

function generatePoints (stats) {
  var total = stats.length
  return stats.map(function (stat, index) {
    var point = valueToPoint(stat, index, total)
    return point.x + ',' + point.y
  }).join(' ')
}
</script>
<style>
.demo-svg { display: block; }
.demo-polygon { fill: #41B883; }
.demo-circle {
  fill: transparent;
  stroke: #35495E;
}
.demo-range-input {
  display: block;
  width: 100%;
  margin-bottom: 15px;
}
</style>
{% endraw %}

Նայեք [այս օրինակը](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-dynamic-state-transitions) վերևում բերված օրինակի ամբողջովին կոդի համար։

## Անցումների Կազմակերպումը դեպի Կոմպոնենտներ

Լիքը state-ի անցումները կարող է դժվարացնել Vue-ի instace-ը կամ կոմպոնենտը։ Բարեբախտաբար, շատ անիմացիաները կարողո են դուրս բերվել դեպի առանձին ժառանգող կոմպոնենտներ։ Եկեք ստեղծենք մի օրինակ օգտագործելով անիմացիայի integer-ը որը նշել էինք մեր վաղ օրինակում․

``` html
<script src="https://cdn.jsdelivr.net/npm/tween.js@16.3.4"></script>

<div id="example-8">
  <input v-model.number="firstNumber" type="number" step="20"> +
  <input v-model.number="secondNumber" type="number" step="20"> =
  {{ result }}
  <p>
    <animated-integer v-bind:value="firstNumber"></animated-integer> +
    <animated-integer v-bind:value="secondNumber"></animated-integer> =
    <animated-integer v-bind:value="result"></animated-integer>
  </p>
</div>
```

``` js
// Այս բարդ կապման տրամաբանությունը կարող է հիմա վերօգտագործվել ցանկացած
// integerner-ների միջև որոնցով մենք կցանկանանք անիմացյիա անել մեր ծրագրում։
// Կոմպոնենտները նաև տրամադրում են մաքուր ինտերֆեյս ավելի դինամիկ
// և բարդ անցումների կոնֆիգուրացիայի համար։
Vue.component('animated-integer', {
  template: '<span>{{ tweeningValue }}</span>',
  props: {
    value: {
      type: Number,
      required: true
    }
  },
  data: function () {
    return {
      tweeningValue: 0
    }
  },
  watch: {
    value: function (newValue, oldValue) {
      this.tween(oldValue, newValue)
    }
  },
  mounted: function () {
    this.tween(0, this.value)
  },
  methods: {
    tween: function (startValue, endValue) {
      var vm = this
      function animate () {
        if (TWEEN.update()) {
          requestAnimationFrame(animate)
        }
      }

      new TWEEN.Tween({ tweeningValue: startValue })
        .to({ tweeningValue: endValue }, 500)
        .onUpdate(function () {
          vm.tweeningValue = this.tweeningValue.toFixed(0)
        })
        .start()

      animate()
    }
  }
})

// Ամբողջ բարդությունը հիմա ջնջվել է հիմնական Vue instance-ից՛
new Vue({
  el: '#example-8',
  data: {
    firstNumber: 20,
    secondNumber: 40
  },
  computed: {
    result: function () {
      return this.firstNumber + this.secondNumber
    }
  }
})
```

{% raw %}
<script src="https://cdn.jsdelivr.net/npm/tween.js@16.3.4"></script>
<div id="example-8" class="demo">
  <input v-model.number="firstNumber" type="number" step="20"> +
  <input v-model.number="secondNumber" type="number" step="20"> =
  {{ result }}
  <p>
    <animated-integer v-bind:value="firstNumber"></animated-integer> +
    <animated-integer v-bind:value="secondNumber"></animated-integer> =
    <animated-integer v-bind:value="result"></animated-integer>
  </p>
</div>
<script>
Vue.component('animated-integer', {
  template: '<span>{{ tweeningValue }}</span>',
  props: {
    value: {
      type: Number,
      required: true
    }
  },
  data: function () {
    return {
      tweeningValue: 0
    }
  },
  watch: {
    value: function (newValue, oldValue) {
      this.tween(oldValue, newValue)
    }
  },
  mounted: function () {
    this.tween(0, this.value)
  },
  methods: {
    tween: function (startValue, endValue) {
      var vm = this
      function animate () {
        if (TWEEN.update()) {
          requestAnimationFrame(animate)
        }
      }

      new TWEEN.Tween({ tweeningValue: startValue })
        .to({ tweeningValue: endValue }, 500)
        .onUpdate(function () {
          vm.tweeningValue = this.tweeningValue.toFixed(0)
        })
        .start()

      animate()
    }
  }
})
new Vue({
  el: '#example-8',
  data: {
    firstNumber: 20,
    secondNumber: 40
  },
  computed: {
    result: function () {
      return this.firstNumber + this.secondNumber
    }
  }
})
</script>
{% endraw %}

Ժառանգող կոմպոնենտներում, մենք կարող ենք օգտագործել ցանկացած հավաքածու բոլոր այն ստրատեգիաների որոնք նշվել են այս էջում, ի հանդերձ Vue-ի կողմից տրամադրվող [ներքին անցումային համակարգը](transitions.html)։ Միասին, կան շատ քիչ սահմանափակումներ արդյունքում։

## Դիզայների Կյանքի Բերումը

Որպեսզի անիմացիա ստեղծել, որոշ նկարագրումներով, նշանակում է կյանքի բերել։
To animate, by one definition, means to bring to life. Դժբախտաբար, երբ դիզայները ստեղծում են պատկերներ, լոգոներ, և կերպարներ, նրանք հիմնականում դրվում են որպես նկար կամ ստատիկ SVG-ներ։ Չնայած որ GitHub-ի ութոտնուկ կատուն, Twitter-ի ծիտիկը, և այլ լոգոներ իրենցից ներկայացնում են ապրող կենդանիներ, բայց նրանք ողջ չեն։

Vue-ն կարող է օգնել։ Մինչ SVG-ները ընդհամենը տվյալներ են, մեզ միայն օրինակներ են պետք թե ինչպես են այս կենդանիները լինում զվարթ, մտածող կամ զգոն ժամանակ։ Այնուհետև Vue-ն կարող է օգնել անցում կատարել state-երի միջև, դարձնելով ձեր գլխավոր էջերը, բեռնման ցուցանակները և notification-ները ավելի ավելի հուզականորեն համոզիչ։

Sarah Drasner-ը ցույց է տալիս ներքևի demo-ում, օգտագործելով չիշտ տեմպով ինտերակտիվ state-ի փոփոխություններ․

<p data-height="265" data-theme-id="light" data-slug-hash="YZBGNp" data-default-tab="result" data-user="sdras" data-embed-version="2" data-pen-title="Vue-controlled Wall-E" class="codepen">See the Pen <a href="https://codepen.io/sdras/pen/YZBGNp/">Vue-controlled Wall-E</a> by Sarah Drasner (<a href="https://codepen.io/sdras">@sdras</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
