<template>
  <nav class="nav-links" v-if="userLinks.length || repoLink">
    <!-- user links -->
    <div
      class="nav-item"
      v-for="item in userLinks"
      :key="item.link">
      <NavLink :item="item"/>
    </div>
    <!-- repo link -->
    <a v-if="repoLink"
      :href="repoLink"
      class="repo-link"
      target="_blank"
      rel="noopener noreferrer">
      {{ repoLabel }}
      <OutboundLink/>
    </a>
  </nav>
</template>

<script>
import OutboundLink from './OutboundLink.vue'
import { isActive, resolveNavLinkItem } from './util'
import NavLink from './NavLink.vue'

export default {
  components: { OutboundLink, NavLink },
  computed: {
    userNav () {
      return this.$site.themeConfig.nav || []
    },
    nav () {
      return this.userNav
    },
    userLinks () {
      return (this.nav || []).map((link => {
        return Object.assign(resolveNavLinkItem(link), {
          items: (link.items || []).map(resolveNavLinkItem)
        })
      }))
    },
    repoLink () {
      const { repo } = this.$site.themeConfig
      if (repo) {
        return /^https?:/.test(repo)
          ? repo
          : `https://github.com/${repo}`
      }
    },
    repoLabel () {
      if (!this.repoLink) return
      if (this.$site.themeConfig.repoLabel) {
        return this.$site.themeConfig.repoLabel
      }

      const repoHost = this.repoLink.match(/^https?:\/\/[^/]+/)[0]
      const platforms = ['GitHub', 'GitLab', 'Bitbucket']
      for (let i = 0; i < platforms.length; i++) {
        const platform = platforms[i]
        if (new RegExp(platform, 'i').test(repoHost)) {
          return platform
        }
      }

      return 'Source'
    },
  },
  methods: {
    isActive
  }
}
</script>

<style lang="stylus">
@import './styles/config.styl'

.nav-links
  display inline-block
  a
    line-height 1.4rem
    color inherit
    &:hover, &.router-link-active
      color $accentColor
  .nav-item
    cursor: pointer
    position relative
    display inline-block
    margin-left 1.5rem
    line-height 2rem
  .repo-link
    margin-left 1.5rem

@media (max-width: $MQMobile)
  .nav-links
    .nav-item, .repo-link
      margin-left 0

@media (min-width: $MQMobile)
  .nav-links a
    &:hover, &.router-link-active
      color $textColor
  .nav-item > a
    &:hover, &.router-link-active
      margin-bottom -2px
      border-bottom 2px solid #42b983
</style>
