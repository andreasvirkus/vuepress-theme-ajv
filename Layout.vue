<template>
  <div class="theme-container"
    :class="pageClasses"
    @touchstart="onTouchStart"
    @touchend="onTouchEnd">

    <Navbar/>

    <div class="custom-layout" v-if="$page.frontmatter.layout">
      <component :is="$page.frontmatter.layout"/>
    </div>

    <Page v-else />
  </div>
</template>

<script>
import Vue from 'vue'
import nprogress from 'nprogress'
import Home from './Home'
import Blog from './Blog'
import Navbar from './Navbar'
import Page from './Page'
import { pathToComponentName, getTitle, getLang } from '@app/util'

export default {
  components: { Home, Blog, Page, Navbar },

  computed: {
    pageClasses() {
      const userPageClass = this.$page.frontmatter.pageClass
      return [
        {
          'no-navbar': !this.showNavbar,
        },
        userPageClass
      ]
    }
  },

  created () {
    if (this.$ssrContext) {
      this.$ssrContext.title = getTitle(this.$title, this.$page)
      this.$ssrContext.lang = this.$lang
      this.$ssrContext.description = this.$page.description || this.$description
    }
  },

  mounted () {
    // update title / meta tags
    this.currentMetaTags = []
    const updateMeta = () => {
      console.log('Document:', document);
      console.log('Title:', document.title);

      document.title = getTitle(this.$title, this.$page)
      document.documentElement.lang = this.$lang
      const meta = [
        {
          name: 'description',
          content: this.$description
        },
        ...(this.$page.frontmatter.meta || [])
      ]
      this.currentMetaTags = updateMetaTags(meta, this.currentMetaTags)
    }
    this.$watch('$page', updateMeta)
    updateMeta()

    // configure progress bar
    nprogress.configure({ showSpinner: false })

    this.$router.beforeEach((to, from, next) => {
      if (to.path !== from.path && !Vue.component(pathToComponentName(to.path))) {
        nprogress.start()
      }
      next()
    })

    this.$router.afterEach(() => {
      nprogress.done()
    })
  },

  beforeDestroy () {
    updateMetaTags(null, this.currentMetaTags)
  },

  methods: {
    // side swipe
    onTouchStart (e) {
      this.touchStart = {
        x: e.changedTouches[0].clientX,
        y: e.changedTouches[0].clientY
      }
    },
    onTouchEnd (e) {
      const dx = e.changedTouches[0].clientX - this.touchStart.x
      const dy = e.changedTouches[0].clientY - this.touchStart.y
      if (Math.abs(dx) > Math.abs(dy) && Math.abs(dx) > 40) {
        if (dx > 0 && this.touchStart.x <= 80) {
          // this.toggleSidebar(true)
        } else {
          // this.toggleSidebar(false)
        }
      }
    }
  }
}

function updateMetaTags (meta, current) {
  if (current) {
    current.forEach(c => {
      document.head.removeChild(c)
    })
  }
  if (meta) {
    return meta.map(m => {
      const tag = document.createElement('meta')
      Object.keys(m).forEach(key => {
        tag.setAttribute(key, m[key])
      })
      document.head.appendChild(tag)
      return tag
    })
  }
}
</script>

<style src="prismjs/themes/prism-tomorrow.css"></style>
<style src="./styles/theme.styl" lang="stylus"></style>
