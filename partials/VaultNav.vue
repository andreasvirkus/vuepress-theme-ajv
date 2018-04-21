<template>
  <nav class="vault__menu" role="navigation">
    <ul class="vault__menu-list">
      <li v-for="snippet in vault" class="vault__menu-item">
        <a :href="snippet.path" class="vault__menu-link">{{ snippet.title }}</a> |
        <em>{{ snippet.length }}</em>
      </li>
    </ul>
  </nav>
</template>
