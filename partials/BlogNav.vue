<template>
  <nav class="blog__menu" role="navigation">
    <ul class="blog__menu-list">
      <li v-for="(post, i) in posts" :key="i" class="blog__menu-item" :class="{ active: post.path === $router.path }">
        <a :href="'/' + post.path" class="blog__menu-link">
          <div class="blog__menu-title">{{ post.title }}</div>
          <div class="blog__menu-date"><em>{{ post.postDate }}</em></div>
        </a>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: 'blog-nav',
  props: ['posts']
}
</script>

<style>
.blog__menu-list {
    display: flex;
    list-style: none;
    flex-wrap: wrap;
    padding-left: 0;
}

@media screen and (min-width: 35em) {
    .blog__menu-item {
        flex-basis: 48%;
        margin: 5px;
    }
}

.blog__menu-item::before {
    content: none;
}

.blog__menu-link {
    display: block;
    padding: 1em;
    text-decoration: none;
    border-bottom: 0;
}

.blog__menu-link::before,
.blog__menu-link::after {
    content: '';
    display: block;
    position: absolute;
    height: 35px;
    width: 35px;
}

.blog__menu-link::before {
    top: 15px;
    left: 10px;
    border-top: 3px solid #222;
    border-left: 3px solid #222;
}

.blog__menu-link::after {
    bottom: 15px;
    right: 10px;
    border-right: 3px solid transparent;
    border-bottom: 3px solid transparent;
    transition: border-color 600ms cubic-bezier(0.23, 1, 0.32, 1);
}

.blog__menu-link:hover {
    color: inherit;
}

.blog__menu-link:hover::after {
    border-color: #222;
}

.blog__menu-date {
    font-size: 0.7em;
}
</style>
