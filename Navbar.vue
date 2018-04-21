<template>
  <header class="navbar">
    <router-link to="/" class="home-link">
      <img class="logo"
        v-if="$site.themeConfig.logo"
        :src="$withBase($site.themeConfig.logo)">
    </router-link>
    <div class="links">
      <SearchBox v-if="$site.themeConfig.search !== false"/>
      <NavLinks class="can-hide"/>
    </div>
  </header>
</template>

<script>
import SearchBox from './SearchBox.vue'
import NavLinks from './NavLinks.vue'

export default {
  components: { NavLinks, SearchBox }
}
</script>

<style lang="stylus">
@import './styles/config.styl'

.navbar
  padding 0.7rem 1.5rem
  line-height $navbarHeight - 1.4rem
  position relative
  a, span, img
    display inline-block
  .logo
    height $navbarHeight - 1.4rem
    min-width $navbarHeight - 1.4rem
    margin-right 0.8rem
    vertical-align top
  .site-name
    font-size 1.3rem
    font-weight 600
    color $textColor
    position relative
  .links
    font-size 0.9rem
    position absolute
    right 1.5rem
    top 0.7rem

@media (max-width: $MQMobile)
  .navbar .can-hide
    display none
</style>
