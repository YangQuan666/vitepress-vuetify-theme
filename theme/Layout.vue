<template>
  <v-responsive class="border rounded">
    <v-app :theme="theme">
      <v-app-bar class="px-3" color="primary" prominent>
        <template v-slot:prepend>
          <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        </template>
        <v-avatar image="https://cdn.vuetifyjs.com/docs/images/logos/vuetify-logo-dark-atom.svg" @click="router.go('/')" variant="elevated" class="ma-2" />
        <v-app-bar-title v-if="!display.mobile.value">{{ site.title }}</v-app-bar-title>
        <Search />
        <v-spacer v-if="!display.mobile.value" />
        <v-progress-linear indeterminate absolute color="secondary" :active="loading"
          :indeterminate="loading"></v-progress-linear>
        <template v-slot:append>
          <v-btn icon="mdi-theme-light-dark" @click="isDark = !isDark"></v-btn>
        </template>
      </v-app-bar>
      <v-navigation-drawer class="bg-primary" v-model="drawer">
        <template v-slot:prepend>
          <div class="mx-auto text-center">
            <v-avatar color="brown" size="200">
              <v-img :alt="themeConfig.author" src="https://cdn.vuetifyjs.com/images/john.jpg"></v-img>
            </v-avatar>
            <h3>{{ themeConfig.author }}</h3>
            <p class="text-caption mt-1">{{ themeConfig.signature }}</p>
            <v-btn variant="text" :icon="item.icon" :href="item.link" v-for="item in themeConfig.socialLinks"></v-btn>
          </div>
        </template>
        <v-divider />
        <v-list nav>
          <v-list-item prepend-icon="mdi-view-dashboard" title="主页" value="home" href="/"
            :active="route.path === site.base">
          </v-list-item>
          <v-list-group :value="nav.title" v-for="nav in themeConfig.nav">
            <template v-slot:activator="{ props }">
              <v-list-item v-bind="props" :prepend-icon="nav.icon" :title="nav.title"></v-list-item>
            </template>
            <v-list-item v-for="({ title, icon, link }, i) in nav.items" :key="i" :append-icon="icon" :title="title"
              :value="title" :active="route.path === link" @click="router.go(link)"></v-list-item>
          </v-list-group>
        </v-list>
      </v-navigation-drawer>

      <v-main class="align-center justify-center">
        <v-container>
          <Timeline v-if="page.frontmatter.layout === 'home'" />
          <Post v-else-if="page.frontmatter.layout === 'post'" />
          <Content v-else />
        </v-container>
      </v-main>

      <v-footer name="footer" class="bg-indigo-lighten-1 d-flex flex-column">
        <div class="pt-0">Released under the GPLv3 License.
          Copyright © 2019-present <strong>{{ themeConfig.author }}</strong></div>
      </v-footer>
    </v-app>
  </v-responsive>
</template>
<script setup>
import { ref, watchPostEffect } from 'vue'
import { useData, useRouter, useRoute } from 'vitepress'
import { useDisplay } from 'vuetify'
import Post from './component/Post.vue'
import Timeline from './component/Timeline.vue'
import Search from './component/Search.vue'

const drawer = ref()
const { site, page, isDark } = useData()
const { themeConfig } = site.value
const router = useRouter()
const route = useRoute()
const loading = ref(false)
const display = useDisplay()
const theme = ref('light')

watchPostEffect(() => {
  theme.value = isDark.value ? 'dark' : 'light'
})

router.onBeforeRouteChange = () => {
  loading.value = true
}
router.onAfterRouteChanged = () => {
  loading.value = false
  if (display.mdAndDown.value) {
    drawer.value = false
  }
}
</script>