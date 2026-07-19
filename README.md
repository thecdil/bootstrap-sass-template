# bootstrap-sass-template

Basic Jekyll project template containing the full Bootstrap Sass source code and necessary Gemfile dependencies.
This allows you to use Bootstrap's Sass variables to fully customize your build of Bootstrap, following the [Bootstrap source files](https://getbootstrap.com/docs/5.3/getting-started/download/#source-files) approach.

Currently includes Bootstrap 5.3.8 (Sass source in `_sass/`, compiled JS bundle in `assets/lib/`).

## Requirements

- **Ruby** (3.1+) with **Bundler**.
    - Sass is compiled by the [sass-embedded](https://rubygems.org/gems/sass-embedded) gem, which ships its own Dart Sass binary (Ruby Sass and SassC are end-of-life and are *not* used).
- **A JavaScript runtime, i.e. [Node.js](https://nodejs.org/)**, available at build time.
    - Used by the [autoprefixer-rails](https://rubygems.org/gems/autoprefixer-rails) gem to add vendor prefixes to the compiled CSS (Node itself is *not* used to compile anything). Node is preinstalled on GitHub Actions runners and most dev machines. If you can not install Node, add `gem 'mini_racer'` to the Gemfile instead.

*Note:* since this template uses Jekyll 4.x and a custom plugin, it will **not** work with the legacy GitHub Pages automatic build (which is locked to Jekyll 3.x safe mode). Build locally or with a GitHub Actions workflow.

## Use

1. Install dependencies: `bundle install`
2. Develop locally: `bundle exec jekyll serve`
3. Build the site: `bundle exec jekyll build` (output in `_site/`)

## How the CSS build works

1. Jekyll's built-in Sass pipeline compiles `assets/css/main.scss` (using Dart Sass via `sass-embedded`), pulling in the full Bootstrap source from `_sass/` plus the template styles, and outputs a minified `assets/css/main.css`.
2. After the site is written, the `_plugins/autoprefixer.rb` hook runs [Autoprefixer](https://github.com/postcss/autoprefixer) over the generated CSS using [Bootstrap's official browser support targets](https://github.com/twbs/bootstrap/blob/main/.browserslistrc), matching the official compiled Bootstrap builds.

*Note:* Bootstrap's own Sass source triggers deprecation warnings on current Dart Sass versions (an upstream Bootstrap issue). These are silenced via the `sass: quiet_deps: true` option in `_config.yml` — warnings from your own styles will still appear.

## Customize

- **Bootstrap variables:** edit `_sass/_variables.scss` and `_sass/_variables-dark.scss` to customize colors, fonts, spacing, breakpoints, etc.
- **Template styles:** `_sass/template/_template.scss` contains this template's base styles.
- **Your styles:** add tweaks and overrides in `_sass/template/_custom.scss`, which is loaded last.
- **Trim the build:** comment out unused component imports in `_sass/bootstrap.scss` to shrink the CSS output.

## Updating Bootstrap

1. Download the [Bootstrap source files](https://getbootstrap.com/docs/5.3/getting-started/download/#source-files) and replace the contents of `_sass/` with the new `scss/` folder contents.
2. Replace `assets/lib/bootstrap.bundle.min.js` (and `.map`) with the new compiled JS bundle.
