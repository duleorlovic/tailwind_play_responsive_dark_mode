# Tailwind Play Responsive and Dark mode

Using jekyll locally to do similar to 

```
jekyll new tailwind_play_responsive_dark_mode --blank
cd tailwind_play_responsive_dark_mode
```
Add `.gitignore`, [jekyll-postcss](https://github.com/mhanberg/jekyll-postcss)
gem, enable it in `_config.yml`
```
cat > .gitignore << HERE_DOC
_site/
.sass-cache/
.jekyll-cache/
.jekyll-metadata
node_modules
HERE_DOC

cat > Gemfile << HERE_DOC
source 'https://rubygems.org'

gem 'jekyll'
gem 'jekyll-postcss'
gem 'webrick'
HERE_DOC
bundle


cat >> _config.yml << HERE_DOC
plugins:
  - jekyll-postcss

postcss:
  # disable cache for Tailwind CSS JIT engine so we do not need to restart jekyll
  cache: false
HERE_DOC
```
Install postcss, tailwindcss, autoprefixer cssnano tools and enable them in
`postcss.config.js` and `tailwind.config.js` file
```
yarn add postcss@latest tailwindcss@latest autoprefixer@latest cssnano@latest -D

cat > postcss.config.js << HERE_DOC
module.exports = {
  plugins: [
    require('tailwindcss'),
    require('autoprefixer'),
    ...(process.env.JEKYLL_ENV == 'production'
      ? [require('cssnano')({ preset: 'default' })]
      : [])
  ]
}
HERE_DOC

cat > tailwind.config.js << HERE_DOC
module.exports = {
  content: [
    './_drafts/**/*.html',
    './_includes/**/*.html',
    './_layouts/**/*.html',
    './_posts/*.md',
    './*.md',
    './*.html',
  ],
  theme: {
    theme: {
      extend: {},
    },
  },
  plugins: []
}
HERE_DOC
```
Remove `main.scss` and include tailwind
```
cat > assets/css/main.css << HERE_DOC
---
---

@tailwind base;
@tailwind components;
@tailwind utilities;
HERE_DOC

git rm assets/css/main.scss

git add .
git commit -m"Add tailwindcss"
```

