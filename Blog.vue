<template>
  <main>
    <h1>{{ title }}</h1>
    <h3>{{ description }}</h3>

    <Content/>

    <blog-nav :posts="posts"/>
  </main>
</template>

<script>
import BlogNav from './partials/BlogNav'
// TODO: Get posts (all .md files from /thoughts dir)
export default {
  data() {
    return {
      posts: []
    }
  },
  components: { BlogNav }
}
</script>

<style src="./css/blog.css"></style>
<style src="./css/blog-nav.css"></style>
<style>

</style>
