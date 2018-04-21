<template>
  <div class="theme-container">
    <div class="content">
      <h1>404</h1>
      <blockquote>{{ getMsg() }}</blockquote>

      <!-- 404 credits go to Constantine Apostolou
      https://codepen.io/conmarap/pen/mVMvVv -->
      <div class="code-area">
          <span style="color: #777;font-style:italic;">
              // 404 page not found.
          </span>
          <span>
              <span style="color:#d65562;">
              if
              </span>
              (<span style="color:#4ca8ef;">!</span><span style="font-style: italic;color:#1abc9c;">found</span>)
              {
          </span>
          <span>
              <span style="padding-left: 15px;color:#2796ec">
              <i style="width: 10px;display:inline-block"></i>throw
              </span>
              <span>
              (<span style="color: #a6a61f">"(╯°□°)╯︵ ┻━┻"</span>);
              </span>
              <span style="display:block">}</span>
          </span>
      </div>

      <p>You've sailed into uncharted territories! I'm not sure why you thought this
      page would exist, but I didn't have the coding skills to pull it off.</p>
      <a href="javascript:history.back()">Navigate back</a> or hop to the <router-link to="/">homepage</router-link>.

      <p>
        If you're looking to get in touch, just reach out to me on
        <a href="https://ee.linkedin.com/pub/andreas-johan-virkus/a4/928/22" title="View my LinkedIn profile">LinkedIn</a> or via
        <a href="mailto:write@andreasvirkus.me" title="Shoot me a mail!">e-mail</a>.
      </p>
    </div>
  </div>
</template>

<script>
const msgs = [
  `Why, it's obvious, Watson... There's nothing here.`,
  `How did we get here? Is this a Hangover IV in the making?`,
  `Feelin' mighty adventurous, aren't we?`,
  `That's your standard Four-Oh-Four right there, cap'n`,
  `Looks like we've got some broken links. How about you help me fix them?`
]

export default {
  methods: {
    getMsg () {
      return msgs[Math.floor(Math.random() * msgs.length)]
    }
  }
}
</script>

<style scoped>
  .code-area {
    font-family: monospace;
    font-size: 2em;
  }
  .code-area > span {
    display: block;
  }

  @media screen and (max-width: 45em) {
    .code-area {
      font-size: 5vw;
      min-width: auto;
      width: 95%;
      margin: auto;
      padding: 5px;
      padding-left: 10px;
      line-height: 6.5vw;
    }
  }
</style>