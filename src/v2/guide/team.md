---
title: Ծանոթացեք Թիմին
type: ուղեցույց
order: 803
---

{% raw %}
<script id="vuer-profile-template" type="text/template">
  <div class="vuer">
    <div class="avatar">
      <img v-if="profile.imageUrl"
        :src="profile.imageUrl"
        :alt="profile.name" width=80 height=80>
      <img v-else-if="profile.github"
        :src="'https://github.com/' + profile.github + '.png'"
        :alt="profile.name" width=80 height=80>
      <img v-else-if="profile.twitter"
        :src="'https://avatars.io/twitter/' + profile.twitter"
        :alt="profile.name" width=80 height=80>
    </div>
    <div class="profile">
      <h3 :data-official-title="profile.title">
        {{ profile.name }}
        <sup v-if="profile.title && titleVisible" v-html="profile.title"></sup>
      </h3>
      <dl>
        <template v-if="profile.reposOfficial">
          <dt>Կենտրոնացումը</dt>
          <dd>
            <ul>
              <li v-for="repo in profile.reposOfficial">
                <a :href="githubUrl('vuejs', repo)" target=_blank rel="noopener noreferrer">{{ repo.name || repo }}</a>
              </li>
            </ul>
          </dd>
        </template>
        <template v-if="profile.github && profile.reposPersonal">
          <dt>Էկոհամակարգ</dt>
          <dd>
            <ul>
              <li v-for="repo in profile.reposPersonal">
                <a :href="githubUrl(profile.github, repo)" target=_blank rel="noopener noreferrer">{{ repo.name || repo }}</a>
              </li>
            </ul>
          </dd>
        </template>
        <template v-if="profile.work">
          <dt>
            <i class="fa fa-briefcase"></i>
            <span class="sr-only">Աշխատանք</span>
          </dt>
          <dd v-html="workHtml"></dd>
        </template>
        <span v-if="profile.distanceInKm" class="distance">
          <dt>
            <i class="fa fa-map-marker"></i>
            <span class="sr-only">Հեռավորություն</span>
          </dt>
          <dd>
            Մասին
            <span
              v-if="profile.distanceInKm <= 150"
              :title="profile.name + ' is close enough to commute to your location.'"
              class="user-match"
            >{{ textDistance }} հեռու</span>
            <template v-else>{{ textDistance }} հեռու</template>
            in {{ profile.city }}
          </dd>
        </span>
        <template v-else-if="profile.city">
          <dt>
            <i class="fa fa-map-marker"></i>
            <span class="sr-only">Քաղաք</span>
          </dt>
          <dd>
            {{ profile.city }}
          </dd>
        </template>
        <template v-if="profile.languages">
          <dt>
            <i class="fa fa-globe"></i>
            <span class="sr-only">Լեզուներ</span>
          </dt>
          <dd v-html="languageListHtml" class="language-list"></dd>
        </template>
        <template v-if="profile.links">
          <dt>
            <i class="fa fa-link"></i>
            <span class="sr-only">Հղումներ</span>
          </dt>
          <dd>
            <ul>
              <li v-for="link in profile.links">
                <a :href="link" target=_blank>{{ minimizeLink(link) }}</a>
              </li>
            </ul>
          </dd>
        </template>
        <footer v-if="hasSocialLinks" class="social">
          <a class=github v-if="profile.github" :href="githubUrl(profile.github)">
            <i class="fa fa-github"></i>
            <span class="sr-only">Github</span>
          </a>
          <a class=twitter v-if="profile.twitter" :href="'https://twitter.com/' + profile.twitter">
            <i class="fa fa-twitter"></i>
            <span class="sr-only">Twitter</span>
          </a>
          <a class=codepen v-if="profile.codepen" :href="'https://codepen.io/' + profile.codepen">
            <i class="fa fa-codepen"></i>
            <span class="sr-only">CodePen</span>
          </a>
          <a class=linkedin v-if="profile.linkedin" :href="'https://www.linkedin.com/in/' + profile.linkedin">
            <i class="fa fa-linkedin"></i>
            <span class="sr-only">LinkedIn</span>
          </a>
        </footer>
      </dl>
    </div>
  </div>
</script>

<div id="team-members">
  <div class="team">

    <h2 id="active-core-team-members">
      Ակտիվ Հիմնական Թիմի Անդամներ
      <button
        v-if="geolocationSupported && !userPosition"
        @click="getUserPosition"
        :disabled="isSorting"
        class="sort-by-distance-button"
      >
        <i
          v-if="isSorting"
          class="fa fa-refresh rotating-clockwise"
        ></i>
        <template v-else>
          <i class="fa fa-map-marker"></i>
          <span>փնտրել ինձ մոտիկ</span>
        </template>
      </button>
    </h2>

    <p v-if="errorGettingLocation" class="tip">
      Չհաջողվեց ստանալ ձեր գտնվելու վայրը։
    </p>

    <p>
      Vue֊ի և իր էկոհամակարգի զարգացումը առաջնորդվում է միջազգային թիմի կողմից,
      որոնցից ոմանք նախընտրել են ներկայացնել ներքևում։
    </p>

    <p v-if="userPosition" class="success">
      Հիմնական թիմը դասավորվել է ըստ նրանց հեռավորության ձեզանից։
    </p>

    <vuer-profile
      v-for="profile in sortedTeam"
      :key="profile.name"
      :profile="profile"
      :title-visible="titleVisible"
    ></vuer-profile>
  </div>

  <div class="team">
    <h2 id="core-team-emeriti">
      Հիմնական Թիմ Emeriti
    </h2>

    <p>
      Այստեղ մենք հարգում ենք որոշ այլևս ոչ ակտիվ թիմի անդամների որոնք կատարել են թանկ աջակցություններ նախկինում։
    </p>

    <vuer-profile
      v-for="profile in teamEmeriti"
      :key="profile.name"
      :profile="profile"
      :title-visible="titleVisible"
    ></vuer-profile>
  </div>

  <div class="team">
    <h2 id="community-partners">
      Համայնքի Գործընկերները
      <button
        v-if="geolocationSupported && !userPosition"
        @click="getUserPosition"
        :disabled="isSorting"
        class="sort-by-distance-button"
      >
        <i
          v-if="isSorting"
          class="fa fa-refresh rotating-clockwise"
        ></i>
        <template v-else>
          <i class="fa fa-map-marker"></i>
          <span>փնտրել ինձ մոտիկ</span>
        </template>
      </button>
    </h2>

    <p v-if="errorGettingLocation" class="tip">
      Չհաջողվեց ստանալ ձեր գտնվելու վայրը։
    </p>

    <p>
      Որոշ անդամներ Vue համայնքի այնքան են հարստացրել այն, որ նրանք արժանի են հատուկ նշում։ Մենք զարգացրել ենք ավելի մոտիկ կապ այս հատուկ գործընկերների հետ, հաճախ քննարկելով նրանց հետ առաջիկա հատկությունների և նորությունների վերաբերյալ։
    </p>

    <p v-if="userPosition" class="success">
      Համայնքի գործընկերները դասավորվել են ըստ հեռավորության ձեզանից։
    </p>

    <vuer-profile
      v-for="profile in sortedPartners"
      :key="profile.name"
      :profile="profile"
      :title-visible="titleVisible"
    ></vuer-profile>
  </div>
</div>

<script>
(function () {
  var cityCoordsFor = {
    'Անսի, Ֆրանսիա': [45.899247, 6.129384],
    'Ալիկանտե, Իսպանիա' : [38.346543, -0.483838],
    'Ամստերդամ, Նիդերլանդներ': [4.895168, 52.370216],
    'Ատլանտա, Ջորջիա, ԱՄՆ': [33.749051, -84.387858],
    'Բանգալոր, Հնդկաստան': [12.971599, 77.594563],
    'Պեկին, Չինաստան': [39.904200, 116.407396],
    'Բորդո, Ֆրանսիա': [44.837789, -0.579180],
    'Բուխարեստ,  Ռումինիա': [44.426767, 26.102538],
    'Չենքդու, Չինաստան': [30.572815, 104.066801],
    'Չոնգքինգ, Չինաստան': [29.431586, 106.912251],
    'Դենվեր, Կալորադո, ԱՄՆ': [39.739236, -104.990251],
    'Դուբլին, Իռլանդիա': [53.349918, -6.260174],
    'Դուբնա, Ռուսաստան': [56.732020, 37.166897],
    'Իստ Լանսինգ, Միննեսոտա, ԱՄՆ': [42.736979, -84.483865],
    'Ֆորտ Վորթ, Տեքսաս, ԱՄՆ': [32.755331, -97.325735],
    'Հանգզոու, Չինաստան': [30.274084, 120.155070],
    'Ջերսի Սիթի, Նյու Ջերսի, ԱՄՆ': [40.728157, -74.558716],
    'Կինգսթոն, Ճամայկա': [18.017874, -76.809904],
    'Կրասնոդար, Ռուսաստան': [45.039267, 38.987221],
    'Լենսինգ, Մինեսոտա, ԱՄՆ': [42.732535, -84.555535],
    'Լոնդոն, ՄԹ': [51.507351, -0.127758],
    'Լիոն, Ֆրանսիա': [45.764043, 4.835659],
    'Մանհեիմ, Գերմանիա': [49.487459, 8.466039],
    'Մոսկվա, Ռուսաստան': [55.755826, 37.617300],
    'Մյունխ, Գերմանիա': [48.137154, 11.576124],
    'Օռլանդո, Ֆլորիդա, ԱՄՆ': [28.538335, -81.379236],
    'Փարիզ, Ֆրանսիա': [48.856614, 2.352222],
    'Պոզնան,  Լեհաստան': [52.4006553, 16.761583],
    'Սեուլ, Հարավային Կորեա': [37.566535, 126.977969],
    'Շանգհայ, Չինաստան': [31.230390, 121.473702],
    'Սինգապուր': [1.352083, 103.819839],
    'Սիդնեյ, Ավստրալիա': [-33.868820, 151.209290],
    'Տաքուառիտինգա, Բրազիլիա': [-21.430094, -48.515285],
    'Թեհրան, Իրան': [35.689197, 51.388974],
    'Թեսալոնիկի, Հունաստան': [40.640063, 22.944419],
    'Տոկյո, Ճապոնյա': [35.689487, 139.691706],
    'Տորոնտո, Կանադա': [43.653226, -79.383184],
    'Վրոքլավ, Լոհաստան': [51.107885, 17.038538],
    'Բոստոն, Մասաչուսետս, ԱՄՆ': [42.360081, -71.058884],
    'Կիև, Ուկրաինա': [50.450100, 30.523399],
    'Վաշինգտոն, ԿՇ, ԱՄՆ': [38.8935755,-77.0846156,12],
    'Խարկով, Լեհաստան': [50.064650, 19.936579],
    'Օսլո, Նորվեգիա': [59.911491, 10.757933],
    'Կանագավա, Ճապոնյա': [35.44778, 139.6425]
  }
  var languageNameFor = {
    en: 'English',
    nl: 'Nederlands',
    zh: '中文',
    vi: 'Tiếng Việt',
    pl: 'Polski',
    pt: 'Português',
    ru: 'Русский',
    jp: '日本語',
    fr: 'Français',
    de: 'Deutsch',
    el: 'Ελληνικά',
    es: 'Español',
    hi: 'हिंदी',
    fa: 'فارسی',
    ko: '한국어',
    ro: 'Română',
    uk: 'Українська',
    no: 'Norwegian'
  }

  var team = [{
    name: 'Evan You',
    title: 'Benevolent Dictator For Life',
    city: 'Ջերսի Սիթի, ՆՋ, ԱՄՆ',
    languages: ['zh', 'en'],
    github: 'yyx990803',
    twitter: 'youyuxi',
    work: {
      role: 'Ստեղծող',
      org: 'Vue.js'
    },
    reposOfficial: [
      'vuejs/*', 'vuejs-templates/*'
    ],
    links: [
      'https://www.patreon.com/evanyou'
    ]
  }]

  team = team.concat(shuffle([
    {
      name: 'Eduardo',
      title: 'Real-Time Rerouter',
      city: 'Փարիզ, Ֆրանսիա',
      languages: ['es', 'fr', 'en'],
      github: 'posva',
      twitter: 'posva',
      work: {
        role: 'Freelance Developer և Խորհրդատու',
      },
      reposOfficial: [
        'vuefire', 'vue-router'
      ],
      reposPersonal: [
        'vuex-mock-store', 'vue-promised', 'vue-motion'
      ],
      links: [
        'https://www.patreon.com/posva'
      ]
    },
    {
      name: 'Sodatea',
      city: 'Հանչժոու, Չինաստան',
      languages: ['zh', 'en'],
      github: 'sodatea',
      twitter: 'haoqunjiang',
      reposOfficial: [
        'vue-cli', 'vue-loader'
      ]
    },
    {
      name: 'Pine Wu',
      city: 'Շանհայ, China',
      languages: ['zh', 'en', 'jp'],
      github: 'octref',
      twitter: 'octref',
      work: {
        role: 'Nomad'
      },
      reposOfficial: [
        'vetur'
      ]
    },
    {
      name: 'Jinjiang',
      city: 'Սինգապուր',
      languages: ['zh', 'en'],
      github: 'jinjiang',
      twitter: 'zhaojinjiang',
      reposOfficial: [
        'cn.vuejs.org', 'vue-docs-zh-cn'
      ],
      reposPersonal: [
        'vue-a11y-utils', 'vue-mark-display', 'mark2slides', 'vue-keyboard-over'
      ]
    },
    {
      name: 'Katashin',
      title: 'One of a Type State Manager',
      city: 'Սինգապուր',
      languages: ['jp', 'en'],
      work: {
        role: 'Ծրագրավորող',
        org: 'ClassDo',
        orgUrl: 'https://classdo.com'
      },
      github: 'ktsn',
      twitter: 'ktsn',
      reposOfficial: [
        'vuex', 'vue-class-component'
      ],
      reposPersonal: [
        'vue-designer'
      ]
    },
    {
      name: 'Kazupon',
      title: 'Validated Internationalizing Missionary',
      city: 'Տոկիո, Ճապոնիա',
      languages: ['jp', 'en'],
      github: 'kazupon',
      twitter: 'kazu_pon',
      work: {
        role: 'Ինժիներ',
        org: 'PLAID, Inc.',
        orgUrl: 'https://plaid.co.jp'
      },
      reposOfficial: [
        'vuejs.org', 'jp.vuejs.org'
      ],
      reposPersonal: [
        'vue-i18n', 'vue-cli-plugin-i18n', 'vue-i18n-loader', 'eslint-plugin-vue-i18n', 'vue-i18n-extensions', 'vue-cli-plugin-p11n'
      ],
      links: [
        'https://www.patreon.com/kazupon'
      ]
    },
    {
      name: 'Rahul Kadyan',
      title: 'Ecosystem Glue Chemist',
      city: 'Բանգալոր, Հնդկաստան',
      languages: ['hi', 'en'],
      work: {
        role: 'Ծրագրավորող',
        org: 'Grammarly',
        orgUrl: 'https://grammarly.com/'
      },
      github: 'znck',
      twitter: 'znck0',
      reposOfficial: [
        'rollup-plugin-vue', 'vue-issue-helper'
      ],
      reposPersonal: [
        'vue-developer-experience', 'prop-types', 'grammarly'
      ],
      links: [
        'https://znck.me'
      ]
    },
    {
      name: 'Linusborg',
      title: 'Hive-Mind Community Wrangler (Probably a Bot)',
      city: 'Մանհեիմ, Գերմանիա',
      languages: ['de', 'en'],
      github: 'LinusBorg',
      twitter: 'Linus_Borg',
      reposOfficial: [
        'vuejs/*'
      ],
      reposPersonal: [
        'portal-vue'
      ],
      links: [
        'https://forum.vuejs.org/'
      ]
    },
    {
      name: 'Guillaume Chau',
      title: 'Client-Server Astronaut',
      city: 'Լիոն, Ֆրանսիա',
      languages: ['fr', 'en'],
      github: 'Akryum',
      twitter: 'Akryum',
      work: {
        role: 'Frontend Ծրագրավորող',
        org: 'Livestorm',
        orgUrl: 'https://livestorm.co/'
      },
      reposOfficial: [
        'vue-devtools',
        'vue-cli',
        'vue-curated'
      ],
      reposPersonal: [
        'vue-apollo', 'vue-meteor', 'vue-virtual-scroller', 'v-tooltip'
      ],
      links: [
        'http://patreon.com/akryum'
      ]
    },
    {
      name: 'Sarah Drasner',
      city: 'Դենվեր, ԿՈ, ԱՄՆ',
      languages: ['en'],
      work: {
        role: 'Ծրագրավորողի Փորձի Ղեկավար',
        org: 'Netlify',
        orgUrl: 'https://www.netlify.com/'
      },
      github: 'sdras',
      twitter: 'sarah_edo',
      codepen: 'sdras',
      reposOfficial: [
        'vuejs.org'
      ],
      reposPersonal: [
        'intro-to-vue', 'vue-vscode-snippets', 'vue-vscode-extensionpack', 'sample-vue-shop'
      ],
      links: [
        'https://sarah.dev/'
      ]
    },
    {
      name: 'Damian Dulisz',
      title: 'Dark Mage of Plugins, News, and Confs',
      city: 'Վրոցլավ, Լեհաստան',
      languages: ['pl', 'en'],
      github: 'shentao',
      twitter: 'DamianDulisz',
      work: {
        role: 'Խորհրդատու'
      },
      reposOfficial: [
        'news.vuejs.org'
      ],
      reposPersonal: [
        'shentao/vue-multiselect',
        'shentao/vue-global-events'
      ]
    },
    {
      name: 'Michał Sajnóg',
      city: 'Պոզնան, Լեհաստան',
      languages: ['pl', 'en'],
      github: 'michalsnik',
      twitter: 'michalsnik',
      work: {
        role: 'Senior Frontend Ծրագրավորող / Թիմի Ղեկավար',
        org: 'Netguru',
        orgUrl: 'https://netguru.co/'
      },
      reposOfficial: [
        'eslint-plugin-vue',
        'vue-devtools'
      ],
      reposPersonal: [
        'vue-computed-helpers', 'vue-content-placeholders'
      ]
    },
    {
      name: 'GU Yiling',
      city: 'Շանհայ, Չինաստան',
      languages: ['zh', 'en'],
      work: {
        role: 'Senior ծրագրավորող',
        org: 'Baidu, inc.',
        orgUrl: 'https://www.baidu.com/'
      },
      github: 'Justineo',
      twitter: '_justineo',
      reposOfficial: [
        'vue', 'cn.vuejs.org'
      ],
      reposPersonal: [
        'Justineo/vue-awesome', 'ecomfe/vue-echarts', 'ecomfe/veui'
      ]
    },
    {
      name: 'ULIVZ',
      city: 'Հանչժոու, Չինաստան',
      languages: ['zh', 'en'],
      work: {
        role: 'Senior Frontend Ծրագրավորող',
        org: 'AntFinancial',
        orgUrl: 'https://www.antfin.com'
      },
      github: 'ulivz',
      twitter: '_ulivz',
      reposOfficial: [
        'vuepress'
      ]
    },
    {
      name: 'Phan An',
      title: 'Backend Designer & Process Poet',
      city: 'Մյունխ, Գերմանիա',
      languages: ['vi', 'en'],
      github: 'phanan',
      twitter: 'notphanan',
      work: {
        role: 'Ինժիներական Թիմի Ղեկավար',
        org: 'InterNations',
        orgUrl: 'https://www.internations.org/'
      },
      reposOfficial: [
        'vuejs.org'
      ],
      reposPersonal: [
        'vuequery', 'vue-google-signin-button'
      ],
      links: [
        'https://vi.vuejs.org',
        'https://phanan.net/'
      ]
    },
    {
      name: 'Natalia Tepluhina',
      title: 'Fox Tech Guru',
      city: 'Կիև, Ուկրաինա',
      languages: ['uk', 'ru', 'en'],
      reposOfficial: [
        'vuejs.org',
        'vue-cli'
      ],
      work: {
        role: 'Senior Frontend Ինժիներ',
        org: 'GitLab',
        orgUrl: 'https://gitlab.com/'
      },
      github: 'NataliaTepluhina',
      twitter: 'N_Tepluhina',
    },
    {
      name: 'Yosuke Ota',
      city: 'Կանագավա, Ճապոնիա',
      languages: ['jp'],
      github: 'ota-meshi',
      twitter: 'omoteota',
      work: {
        role: 'Գլխավոր Web Ինժիներ',
        org: 'Future Corporation',
        orgUrl: 'https://www.future.co.jp/'
      },
      reposOfficial: [
        'eslint-plugin-vue'
      ],
    },
    {
      name: 'Ben Hong',
      city: 'Վաշինգտոն, ԿՇ, ԱՄՆ',
      languages: ['en', 'zh'],
      work: {
        role: 'Ծրագրավորողի Փորձի (DX) Ինժիներ',
        org: 'Cypress.io',
      },
      reposOfficial: [
        'vuejs.org',
        'vuepress',
        'vuejs/events'
      ],
      github: 'bencodezen',
      twitter: 'bencodezen',
      links: [
        'https://bencodezen.io/'
      ]
    },
    {
       name: 'Kia King Ishii',
       title: 'The optimist web designer/developer',
       city: 'Կանագավա, Ճապոնիա',
       languages: ['en', 'jp'],
       work: {
         role: 'Տեխնալոգիաների Տաղանդ',
         org: 'Global Brain',
         orgUrl: 'https://globalbrains.com/'
       },
       github: 'kiaking',
       twitter: 'KiaKing85',
       reposOfficial: [
         'vuex'
       ],
       reposPersonal: [
         'vuex-orm/*'
       ]
     }
  ]))

  var emeriti = shuffle([
     {
      name: 'Chris Fritz',
      title: 'Good Word Putter-Togetherer',
      city: 'Դարհեմ, ՀԿ, ԱՄՆ',
      languages: ['en', 'de'],
      github: 'chrisvfritz',
      twitter: 'chrisvfritz',
      work: {
        role: 'Կրթող և Խորհրդատու'
      },
      reposPersonal: [
        'vue-enterprise-boilerplate'
      ]
    },
    {
      name: 'Blake Newman',
      title: 'Performance Specializer & Code Deleter',
      city: 'Լոնդոն, ՄԹ',
      languages: ['en'],
      work: {
        role: 'Ծրագրավորող',
        org: 'Attest',
        orgUrl: 'https://www.askattest.com/'
      },
      github: 'blake-newman',
      twitter: 'blakenewman'
    },
    {
      name: 'kingwl',
      title: 'New Bee',
      city: 'Պեկին, Չինաստան',
      languages: ['zh'],
      work: {
        role: 'Ծրագրավորող',
        org: 'Chaitin',
        orgUrl: 'https://chaitin.cn/'
      },
      github: 'kingwl',
      reposOfficial: [
        'vue'
      ]
    },
    {
      name: 'Alan Song',
      title: 'Regent of Routing',
      city: 'Հանչժոու, Չինաստան',
      languages: ['zh', 'en'],
      work: {
        role: 'Համահիմնադիր',
        org: 'Futurenda',
        orgUrl: 'https://www.futurenda.com/'
      },
      github: 'fnlctrl',
      reposOfficial: [
        'vue-router'
      ]
    },
    {
      name: 'defcc',
      title: 'Details Deity & Bug Surgeon',
      city: 'Չունցին, Չինաստան',
      languages: ['zh', 'en'],
      github: 'defcc',
      work: {
        org: 'zbj.com',
        orgUrl: 'http://www.zbj.com/'
      }
    },
    {
      name: 'gebilaoxiong',
      title: 'Issue Annihilator',
      city: 'Չունցին, Չինաստան',
      languages: ['zh', 'en'],
      github: 'gebilaoxiong',
      work: {
        org: 'zbj.com',
        orgUrl: 'http://www.zbj.com/'
      }
    },
    {
      name: 'Denis Karabaza',
      title: 'Director of Directives (Emoji-Human Hybrid)',
      city: 'Դուբնա, Ռուսաստան',
      languages: ['ru', 'en'],
      github: 'simplesmiler',
      twitter: 'simplesmiler',
      work: {
        role: 'Ծրագրավորող',
        org: 'Neolant',
        orgUrl: 'http://neolant.ru/'
      }
    },
    {
      name: 'Edd Yerburgh',
      title: 'Testatron Alpha 9000',
      city: 'Լոնդոն, ՄԹ',
      languages: ['en'],
      github: 'eddyerburgh',
      twitter: 'EddYerburgh',
      work: {
        role: 'Full Stack Ծրագրավորող'
      },
      reposOfficial: [
        'vue-test-utils'
      ],
      reposPersonal: [
        'avoriaz'
      ],
      links: [
        'https://www.eddyerburgh.me'
      ]
    }
  ])

  var partners = [
    {
      name: 'Maria Lamardo',
      title: 'Front End Engineer at Pendo',
      city: 'Ռոլի, ՀԿ, ԱՄՆ',
      languages: ['en', 'es'],
      work: {
        role: 'Front End Ծրագրավորող',
        org: 'Pendo'
      },
      github: 'mlama007',
      twitter: 'MariaLamardo',
      reposPersonal: [
        'vuejs/events'
      ]
    },
    {
      name: 'Pratik Patel',
      title: 'Organizer of VueConf US',
      city: 'Ատլանտա, ՋԱ, ԱՄՆ',
      languages: ['en'],
      work: {
        role: 'Կազմակերպիչ',
        org: 'VueConf US'
      },
      imageUrl:'https://pbs.twimg.com/profile_images/1541624512/profile-pic-09-11-2011_400x400.png',
      twitter: 'prpatel',
      links: [
        'https://us.vuejs.org/'
      ]
    },
    {
      name: 'Vincent Mayers',
      title: 'Organizer of VueConf US',
      city: 'Ատլանտա, ՋԱ, ԱՄՆ',
      languages: ['en'],
      work: {
        role: 'Կազմակերպիչ',
        org: 'VueConf US'
      },
      imageUrl:'https://pbs.twimg.com/profile_images/916531463905992706/MNvTkO5K_400x400.jpg',
      twitter: 'vincentmayers',
      links: [
        'https://us.vuejs.org/'
      ]
    },
    {
      name: 'Luke Thomas',
      title: 'Creator of Vue.js Amsterdam',
      city: 'Ամստերդամ, Նիդերլանդներ',
      languages: ['nl', 'en', 'de'],
      work: {
        role: 'Ստեղծող',
        org: 'Vue.js Amsterdam'
      },
      imageUrl: 'https://pbs.twimg.com/profile_images/1123492769299877888/aviXE_M5_400x400.jpg',
      twitter: 'lukevscostas',
      linkedin: 'luke-kenneth-thomas-578b3916a',
      links: [
        'https://vuejs.amsterdam'
      ]
    },
    {
      name: 'Jos Gerards',
      title: 'Organizer and Host of Vue.js Amsterdam & Frontend Love',
      city: 'Ամստերդամ, Նիդերլանդներ',
      languages: ['nl', 'en', 'de'],
      work: {
        role: 'Իրադարձությունների ղեկավար',
        org: 'Vue.js Amsterdam'
      },
      imageUrl:'https://pbs.twimg.com/profile_images/1110510517951627269/LDzDyd4N_400x400.jpg',
      twitter: 'josgerards88',
      linkedin: 'josgerards',
      links: [
        'https://vuejs.amsterdam'
      ]
    },
    {
      name: 'Jen Looper',
      title: 'Queen Fox',
      city: 'Բոստոն, ՄԱ, ԱՄՆ',
      languages: ['en', 'fr'],
      work: {
        role: 'Գործադիր տնօրեն',
        org: 'Vue Vixens'
      },
      github: 'jlooper',
      twitter: 'jenlooper',
      links: [
        'https://vuevixens.org/',
        'https://nativescript-vue.org/'
      ]
    },
    {
      name: 'Alex Jover',
      title: 'Vue Components Squeezer',
      city: 'Ալիկանտե, Իսպանիա',
      languages: ['es', 'en'],
      work: {
        role: 'Web, PWA և Կատարման խորհրդատու',
        org: 'Freelance'
      },
      github: 'alexjoverm',
      twitter: 'alexjoverm',
      reposPersonal: [
        'v-runtime-template', 'v-lazy-image', 'vue-testing-series'
      ],
      links: [
        'https://alexjover.com'
      ]
    },
    {
      name: 'Sebastien Chopin',
      title: '#1 Nuxt Brother',
      city: 'Բորդո, Ֆրանսիա',
      languages: ['fr', 'en'],
      github: 'Atinux',
      twitter: 'Atinux',
      work: {
        org: 'NuxtJS',
        orgUrl: 'https://nuxtjs.org'
      },
      reposPersonal: [
        'nuxt/*', 'nuxt-community/*', 'nuxt/vue-meta'
      ]
    },
    {
      name: 'Alexandre Chopin',
      title: '#1 Nuxt Brother',
      city: 'Բորդո, Ֆրանսիա',
      languages: ['fr', 'en'],
      github: 'alexchopin',
      twitter: 'iamnuxt',
      work: {
        org: 'NuxtJS',
        orgUrl: 'https://nuxtjs.org'
      },
      reposPersonal: [
        'nuxt/*', 'nuxt-community/*', 'vue-flexboxgrid'
      ]
    },
    {
      name: 'Khary Sharpe',
      title: 'Viral Newscaster',
      city: 'Քինգսթոն, Ջամայկա',
      languages: ['en'],
      github: 'kharysharpe',
      twitter: 'kharysharpe',
      links: [
        'https://twitter.com/VueJsNews',
        'http://www.kharysharpe.com/'
      ]
    },
    {
      name: 'Pooya Parsa',
      title: 'Nuxtification Modularizer',
      city: 'Թեհրան, Իրան',
      languages: ['fa', 'en'],
      github: 'pi0',
      twitter: '_pi0_',
      work: {
        role: 'Տեխնիկական Խորհրդատու',
        org: 'Fandogh (AUT University)',
        orgUrl: 'https://fandogh.org'
      },
      reposPersonal: [
        'nuxt/*', 'nuxt-community/*', 'bootstrap-vue/*'
      ]
    },
    {
      name: 'Xin Du',
      title: 'Nuxpert',
      city: 'Դուբլին, Իրլանդիա',
      languages: ['zh', 'en'],
      github: 'clarkdo',
      twitter: 'ClarkDu_',
      reposPersonal: [
        'nuxt/*', 'nuxt-community/*'
      ]
    },
    {
      name: 'Yi Yang',
      city: 'Շանհայ, Չինաստան',
      title: 'Interface Elementologist',
      languages: ['zh', 'en'],
      github: 'Leopoldthecoder',
      work: {
        org: 'ele.me',
        orgUrl: 'https://www.ele.me',
      },
      reposPersonal: [
        'elemefe/element', 'elemefe/mint-ui'
      ]
    },
    {
      name: 'Bruno Lesieur',
      title: 'French Community Director',
      city: 'Անեսի, Ֆրանսիա',
      languages: ['fr', 'en'],
      github: 'Haeresis',
      twitter: 'ZetesEthique',
      work: {
        role: 'Համահիմնադիր',
        org: 'Orchard ID',
        orgUrl: 'https://www.orchard-id.com/'
      },
      reposPersonal: [
        'vuejs-fr/*', 'Haeresis/node-atlas-hello-vue'
      ],
      links: [
        'https://node-atlas.js.org/', 'https://blog.lesieur.name/'
      ]
    },
    {
      name: 'ChangJoo Park',
      title: 'Vuenthusiastic Korean Community Organizer',
      city: 'Սեուլ, Հարավային Կորեա',
      languages: ['ko', 'en'],
      github: 'changjoo-park',
      twitter: 'pcjpcj2',
      reposPersonal: [
        'vuejs-kr/kr.vuejs.org', 'ChangJoo-Park/vue-component-generator'
      ],
      links: [
        'https://vuejs-kr.github.io',
        'https://twitter.com/pcjpcj2'
      ]
    },
    {
      name: 'Erick Petrucelli',
      title: 'Perfectionist Chief Translator for Portuguese',
      city: 'Taquaritinga, Brazil',
      languages: ['pt', 'en'],
      github: 'ErickPetru',
      twitter: 'erickpetru',
      work: {
        role: 'Ուսուցիչ',
        org: 'Fatec Taquaritinga',
        orgUrl: 'http://www.fatectq.edu.br/'
      },
      reposPersonal: [
        'vuejs-br/br.vuejs.org', 'ErickPetru/vue-feathers-chat'
      ]
    },
    {
      name: 'Razvan Stoenescu',
      title: 'Deep Space Quasar Creator',
      city: 'Բուխարեստ, Ռումինիա',
      languages: ['ro', 'en'],
      github: 'rstoenescu',
      twitter: 'quasarframework',
      work: {
        role: 'Ծրագրավորող',
        org: 'Quasar Framework',
        orgUrl: 'http://quasar-framework.org/'
      },
      reposPersonal: [
        'quasarframework/quasar', 'quasarframework/quasar-cli', 'quasarframework/quasar-play'
      ]
    },
    {
      name: 'Jilson Thomas',
      title: 'Vue Promoter and VueJobs Guy',
      city: 'Տորոնտո, Կանադա',
      languages: ['en'],
      github: 'JillzTom',
      twitter: 'jilsonthomas',
      work: {
        role: 'Senior Frontend Ծրագրավորող',
        org: 'Nominator',
        orgUrl: 'https://nominator.com/'
      },
      links: [
        'https://vuejobs.com'
      ]
    },
    {
      name: 'Israel Ortuño',
      title: 'VueJobs Buccaneer',
      city: 'Ալիկանտե, Իսպանիա',
      languages: ['es', 'en'],
      github: 'IsraelOrtuno',
      twitter: 'IsraelOrtuno',
      work: {
        role: 'Full Stack Ծրագրավորող',
        org: 'Freelance'
      },
      links: [
        'https://vuejobs.com'
      ]
    },
    {
      name: 'John Leider',
      title: 'Vuetiful Framework Sculptor',
      city: 'Ֆորտ Ուորթ, ՏԽ, ԱՄՆ',
      languages: ['en'],
      github: 'vuetifyjs',
      twitter: 'vuetifyjs',
      work: {
        role: 'Գործադիր տնօրեն',
        org: 'Vuetify LLC',
        orgUrl: 'https://vuetifyjs.com'
      },
      reposPersonal: [
        'vuetifyjs/vuetify'
      ]
    },
    {
      name: 'Grigoriy Beziuk',
      title: 'Translation Gang Leader',
      city: 'Մոսկվա, Ռուսաստան',
      languages: ['ru', 'de', 'en'],
      github: 'gbezyuk',
      work: {
        role: 'Full Stack Ծրագրավորող',
        org: 'Self Employed',
        orgUrl: 'http://gbezyuk.ru'
      },
      reposPersonal: [
        'translation-gang/ru.vuejs.org'
      ]
    },
    {
      name: 'Alexander Sokolov',
      title: 'Russian Translation Sharp Eye',
      city: 'Կրասնոդար, Ռուսաստան',
      languages: ['ru', 'en'],
      github: 'Alex-Sokolov',
      reposPersonal: [
        'translation-gang/ru.vuejs.org'
      ]
    },
    {
      name: 'Anthony Gore',
      title: '',
      city: 'Սիդնեյ, Ավստրալիա',
      languages: ['en'],
      github: 'anthonygore',
      twitter: 'anthonygore',
      work: {
        role: 'Հեղինակ',
        org: 'Vue.js Developers',
        orgUrl: 'https://vuejsdevelopers.com/'
      },
      links: [
        'https://vuejsdevelopers.com'
      ]
    },
    {
      name: 'EGOIST',
      title: 'Build Tool Simplificator',
      city: 'Չենգդու, Չինաստան',
      languages: ['zh', 'en'],
      github: 'egoist',
      twitter: '_egoistlily',
      reposPersonal: [
        'poi', 'ream', 'vue-play'
      ]
    },
    {
      name: 'Alex Kyriakidis',
      title: 'Vueducator Extraordinaire',
      city: 'Սալոնիկ, Հունաստան',
      languages: ['el', 'en'],
      github: 'hootlex',
      twitter: 'hootlex',
      work: {
        role: 'Consultant / Author'
      },
      reposPersonal: [
        'vuejs-paginator', 'vuedo/vuedo', 'the-majesty-of-vuejs-2'
      ],
      links: [
        'https://vuejsfeed.com/', 'https://vueschool.io/'
      ]
    },
    {
      name: 'Rolf Haug',
      title: 'Educator & Consultant',
      city: 'Օսլո, Նորվեգիա',
      languages: ['en', 'no'],
      github: 'rahaug',
      twitter: 'rahaug',
      work: {
        role: 'Դաստիարակ և համահիմնադիր',
        org: 'Vue School',
        orgUrl: 'https://vueschool.io/'
      },
      links: [
        'https://vueschool.io/', 'https://rah.no'
      ]
    },
    {
      name: 'Andrew Tomaka',
      title: 'The Server Server',
      city: 'Իստ Լենսինգ, ՄԻ, ԱՄՆ',
      languages: ['en'],
      github: 'atomaka',
      twitter: 'atomaka',
      reposOfficial: [
        'vuejs/*'
      ],
      work: {
        org: 'Michigan State University',
        orgUrl: 'https://msu.edu/'
      },
      links: [
        'https://atomaka.com/'
      ]
    },
    {
      name: 'Blake Newman',
      title: 'Performance Specializer & Code Deleter',
      city: 'Լոնդոն, ՄԹ',
      languages: ['en'],
      work: {
        role: 'Ծրագրավորող',
        org: 'Attest',
        orgUrl: 'https://www.askattest.com/'
      },
      github: 'blake-newman',
      twitter: 'blakenewman',
      links: [
        'https://vuejs.london'
      ]
    },
    {
      name: 'Filip Rakowski',
      title: 'eCommerce & PWA mastah',
      city: 'Վրոցլավ, Լեհաստան',
      languages: ['pl', 'en'],
      github: 'filrak',
      twitter: 'filrakowski',
      work: {
        role: 'Vue Storefront-ի համահիմնադիր',
        org: 'Divante',
        orgUrl: 'https://divante.co/'
      },
      reposPersonal: [
        'DivanteLtd/vue-storefront', 'DivanteLtd/storefront-ui'
      ],
      links: [
        'https://vuestorefront.io',
        'https://storefrontui.io'
      ]
    },
    {
      name: 'Gregg Pollack',
      title: '',
      city: 'Օրլանդո, Ֆլորիդա, ԱՄՆ',
      languages: ['en'],
      github: 'gregg',
      twitter: 'greggpollack',
      work: {
        role: 'Vue Հրահանգիչ',
        org: 'Vue Mastery',
        orgUrl: 'https://www.vuemastery.com/'
      },
      links: [
        'https://www.vuemastery.com',
        'https://news.vuejs.org/'
      ]
    },
    {
      name: 'Adam Jahr',
      title: '',
      city: 'Օրլանդո, Ֆլորիդա, ԱՄՆ',
      languages: ['en'],
      github: 'atomjar',
      twitter: 'adamjahr',
      work: {
        role: 'Vue Հրահանգիչ',
        org: 'Vue Mastery',
        orgUrl: 'https://www.vuemastery.com/'
      },
      links: [
        'https://www.vuemastery.com',
        'https://news.vuejs.org/'
      ]
    }
  ]

  Vue.component('vuer-profile', {
    template: '#vuer-profile-template',
    props: {
      profile: Object,
      titleVisible: Boolean
    },
    computed: {
      workHtml: function () {
        var work = this.profile.work
        var html = ''
        if (work.orgUrl) {
          html += '<a href="' + work.orgUrl + '" target="_blank" rel="noopener noreferrer">'
          if (work.org) {
            html += work.org
          } else {
            this.minimizeLink(work.orgUrl)
          }
          html += '</a>'
        } else if (work.org) {
          html += work.org
        }
        if (work.role) {
          if (html.length > 0) {
            html = work.role + ' @ ' + html
          } else {
            html = work.role
          }
        }
        return html
      },
      textDistance: function () {
        var distanceInKm = this.profile.distanceInKm || 0
        if (this.$root.useMiles) {
          return roundDistance(kmToMi(distanceInKm)) + ' miles'
        } else {
          return roundDistance(distanceInKm) + ' km'
        }
      },
      languageListHtml: function () {
        var vm = this
        var nav = window.navigator
        if (!vm.profile.languages) return ''
        var preferredLanguageCode = nav.languages
          // The preferred language set in the browser
          ? nav.languages[0]
          : (
              // The system language in IE
              nav.userLanguage ||
              // The language in the current page
              nav.language
            )
        return (
          '<ul><li>' +
          vm.profile.languages.map(function (languageCode, index) {
            var language = languageNameFor[languageCode]
            if (
              languageCode !== 'en' &&
              preferredLanguageCode &&
              languageCode === preferredLanguageCode.slice(0, 2)
            ) {
              return (
                '<span ' +
                  'class="user-match" ' +
                  'title="' +
                    vm.profile.name +
                    ' can give technical talks in your preferred language.' +
                  '"' +
                '\>' + language + '</span>'
              )
            }
            return language
          }).join('</li><li>') +
          '</li></ul>'
        )
      },
      hasSocialLinks: function () {
        return this.profile.github || this.profile.twitter || this.profile.codepen || this.profile.linkedin
      }
    },
    methods: {
      minimizeLink: function (link) {
        return link
          .replace(/^https?:\/\/(www\.)?/, '')
          .replace(/\/$/, '')
          .replace(/^mailto:/, '')
      },
      /**
       * Generate a GitHub URL using a repo and a handle.
       */
      githubUrl: function (handle, repo) {
        if (repo && repo.url) {
          return repo.url
        }
        if (repo && repo.indexOf('/') !== -1) {
          // If the repo name has a slash, it must be an organization repo.
          // In such a case, we discard the (personal) handle.
          return (
            'https://github.com/' +
            repo.replace(/\/\*$/, '')
          )
        }
        return 'https://github.com/' + handle + '/' + (repo || '')
      }
    }
  })

  new Vue({
    el: '#team-members',
    data: {
      team: team,
      teamEmeriti: emeriti,
      partners: shuffle(partners),
      geolocationSupported: false,
      isSorting: false,
      errorGettingLocation: false,
      userPosition: null,
      useMiles: false,
      konami: {
        position: 0,
        code: [38, 38, 40, 40, 37, 39, 37, 39, 66, 65]
      }
    },
    computed: {
      sortedTeam: function () {
        return this.sortVuersByDistance(this.team)
      },
      sortedPartners: function () {
        return this.sortVuersByDistance(this.partners)
      },
      titleVisible: function () {
        return this.konami.code.length === this.konami.position
      }
    },
    created: function () {
      var nav = window.navigator
      if ('geolocation' in nav) {
        this.geolocationSupported = true
        var imperialLanguageCodes = [
          'en-US', 'en-MY', 'en-MM', 'en-BU', 'en-LR', 'my', 'bu'
        ]
        if (imperialLanguageCodes.indexOf(nav.language) !== -1) {
          this.useMiles = true
        }
      }
      document.addEventListener('keydown', this.konamiKeydown)
    },
    beforeDestroy: function () {
      document.removeEventListener('keydown', this.konamiKeydown)
    },
    methods: {
      getUserPosition: function () {
        var vm = this
        var nav = window.navigator
        vm.isSorting = true
        nav.geolocation.getCurrentPosition(
          function (position) {
            vm.userPosition = position
            vm.isSorting = false
          },
          function (error) {
            vm.isSorting = false
            vm.errorGettingLocation = true
          },
          {
            enableHighAccuracy: true
          }
        )
      },
      sortVuersByDistance: function (vuers) {
        var vm = this
        if (!vm.userPosition) return vuers
        var vuersWithDistances = vuers.map(function (vuer) {
          var cityCoords = cityCoordsFor[vuer.city]
          if (cityCoords) {
            return Object.assign({}, vuer, {
              distanceInKm: getDistanceFromLatLonInKm(
                vm.userPosition.coords.latitude,
                vm.userPosition.coords.longitude,
                cityCoords[0],
                cityCoords[1]
              )
            })
          }
          return Object.assign({}, vuer, {
            distanceInKm: null
          })
        })
        vuersWithDistances.sort(function (a, b) {
          if (a.distanceInKm && b.distanceInKm) return a.distanceInKm - b.distanceInKm
          if (a.distanceInKm && !b.distanceInKm) return -1
          if (!a.distanceInKm && b.distanceInKm) return 1
          if (a.name < b.name) return -1
          if (a.name > b.name) return 1
        })
        return vuersWithDistances
      },
      konamiKeydown: function (event) {
        if (this.titleVisible) {
          return
        }

        if (event.keyCode !== this.konami.code[this.konami.position++]) {
          this.konami.position = 0
        }
      }
    }
  })

  /**
  * Shuffles array in place.
  * @param {Array} a items The array containing the items.
  */
  function shuffle (a) {
    a = a.concat([])
    if (window.location.hostname === 'localhost') {
      return a
    }
    var j, x, i
    for (i = a.length; i; i--) {
      j = Math.floor(Math.random() * i)
      x = a[i - 1]
      a[i - 1] = a[j]
      a[j] = x
    }
    return a
  }

  /**
  * Calculates great-circle distances between the two points – that is, the shortest distance over the earth’s surface – using the Haversine formula.
  * @param {Number} lat1 The latitude of the 1st location.
  * @param {Number} lon1 The longitute of the 1st location.
  * @param {Number} lat2 The latitude of the 2nd location.
  * @param {Number} lon2 The longitute of the 2nd location.
  */
  function getDistanceFromLatLonInKm(lat1,lon1,lat2,lon2) {
    var R = 6371 // Radius of the earth in km
    var dLat = deg2rad(lat2-lat1)  // deg2rad below
    var dLon = deg2rad(lon2-lon1)
    var a =
      Math.sin(dLat/2) * Math.sin(dLat/2) +
      Math.cos(deg2rad(lat1)) * Math.cos(deg2rad(lat2)) *
      Math.sin(dLon/2) * Math.sin(dLon/2)
    var c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a))
    var d = R * c // Distance in km
    return d
  }

  function deg2rad(deg) {
    return deg * (Math.PI/180)
  }

  function kmToMi (km) {
    return km * 0.62137
  }

  function roundDistance (num) {
    return Number(Math.ceil(num).toPrecision(2))
  }
})()
</script>
{% endraw %}
