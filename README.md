## Vitepress Vuetify Theme

## Install Dependencies

```bash
# install dependencies
$ npm add -D vitepress-vuetify-theme @mdi/font
```

## Use Vuetify Theme
```js [.vitepress/theme/index.js]
// .vitepress/theme/index.js
import  Theme  from 'vitepress-vuetify-theme/theme'
import 'vuetify/styles'

export default {
  extends: Theme,
} satisfies Theme
```

## Config
copy follow to your `config.mts`
```js [.vitepress/config.js]

import { defineConfig } from 'vitepress'

// https://vitepress.dev/reference/site-config
export default defineConfig({
  title: "blog demo",
  description: "a vitepress theme use vuetify.js",
  lang: 'zh-CN',
  head: [],
  appearance: true,
  themeConfig: {
      logo: 'https://cdn.vuetifyjs.com/images/john.jpg',
      author: 'Yang Quan',
      signature: 'When in doubt, use brute force.',
      socialLinks: [
          {icon: 'mdi-github', link: 'https://github.com/YangQuan666'},
          {icon: 'mdi-reddit', link: 'https://reddit.com/user/QuarkYeung'},
          {icon: 'mdi-email', link: 'mailto:quark.yeung@icloud.com'}
      ],
      nav: [
          {
              title: 'game', icon: 'mdi-gamepad-circle-up', items: [
                  {title: 'Chrome Dinosaur', icon: 'mdi-google-downasaur', link: '/game/path1/'},
              ]
          }, {
              title: 'tool', icon: 'mdi-toolbox', items: [
                  {title: 'path1', icon: 'mdi-calendar-today', link: '/tool/path1/'},
              ]
          }
      ],
      outline: 'deep',
      footer:  {
          message: 'Released under the GPL License.',
          copyright: 'Copyright © 2019-present Yang Quan'
      },
      search: {
          provider: 'local'
      }
  },
  lastUpdated: true,
  cleanUrls: true,
  vite: {
      ssr: {
          noExternal: ['vuetify']
      }
  },
  markdown: {
      headers: {level: [2, 3, 4, 5, 6]},
  }
})

```
## License

[GPL-3.0](https://opensource.org/licenses/GPL-3.0)

Copyright (c) 2021-present, YangQuan666