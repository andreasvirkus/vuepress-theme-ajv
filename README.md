# vuepress-theme-ajv

A blog friendly custom theme for Vuepress.

## Usage

See [Vuepress](https://vuepress.vuejs.org/guide/getting-started.html) official docs for the Getting Started guide.
After setting up Vuepress, list this theme in your `.vuepress/config.js` file under the `theme` parameter

```json
theme: ajv
```

Set the necessary `themeConfig` options in your `.vuepress/config.js`. Much of the same config params apply
as they do for Vuepress [default theme](https://vuepress.vuejs.org/default-theme-config/). Things excluded by this theme are:
- locales
- homepage
- sidebar (our navbar acts as a sidebar)

You're all set to run `vuepress dev docs`!

## TODO
- Refactor Stylus to native CSS with PostCSS nesting
- Swap vars with CSS custom properties
- Add responsive menu
- Set focus styles only for keyboard events (see [example](https://codepen.io/ajv/pen/dMRwyQ))