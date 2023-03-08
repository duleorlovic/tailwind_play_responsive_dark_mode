# Tailwind Play Responsive and Dark mode

You can use this jekyll repo to develop tailwind pages: locally, with live
reload, so you are sure it looks nice on both responsive and dark mode.

It can be deployed to Github Pages using Github Actions.

This repo is hosten on Github

https://github.com/duleorlovic/tailwind_play_responsive_dark_mode

## Preview

Here is a preview how I updated button color only in dark mode

[Screencast from 07.03.2023. 12:01:13.webm](https://user-images.githubusercontent.com/1426092/223404387-c3ee6d53-b84d-4664-a08e-56cbb4886ef9.webm)

<p align="right">(<a href="#top">back to top</a>)</p>

## Local develoment

To work locally you need Ruby and Yarn already installed, and just clone the
repo and start jekyll:

```
git clone git@github.com:duleorlovic/tailwind_play_responsive_dark_mode.git
cd tailwind_play_responsive_dark_mode
bundle
yarn
bundle exec jekyll serve --livereload
```
Open your browser in [http://127.0.0.1:4000/](http://127.0.0.1:4000/) and make
some changes to see that it is updated automatically in browser.

<p align="right">(<a href="#top">back to top</a>)</p>

## Live demo

Live page for this repo it is:

https://duleorlovic.github.io/tailwind_play_responsive_dark_mode/

For live site on `your-user-name.github.io/your-repo-name` you need to [enable
github pages](#github-pages), so when you deploy to github main branch, new
Action will be triggered and your site will be live in a few moments at
github.io.

<p align="right">(<a href="#top">back to top</a>)</p>

## Start from scratch

Install jekyll
```
gem install jekyll
```
generate jekyll blank site

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
Remove `main.scss` and add tailwind directives to css
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

Add some tailwind classes

```
cat >> index.md << HERE_DOC
<div class="text-red-500">
  Text red 500
</div>
HERE_DOC
```
and see in the browser with live reload
```
bundle exece jekyll serve --livereload
```

<p align="right">(<a href="#top">back to top</a>)</p>

## Github Actions

Since `jekyll-postcss` is not whitelisted on [github
pages](https://pages.github.com/versions/) we have to use some machine to
generate the site. To can use Github Actions.
Since it will be run on ubuntu you need to add that platform to Gemfile.lock

```
bundle lock --add-platform x86_64-linux
```

Create configuration for Actions
```
# .github/workflows/github-pages.yml
name: Build and deploy this site to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  github-pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: 3.1
          bundler-cache: true
      - name: Setup Node
        uses: actions/setup-node@v2
        with:
          node-version: '18'
      - run: npm install
      - name: Build site
        uses: limjh16/jekyll-action-ts@v2
        with:
          enable_cache: true
          custom_opts: "--config _config.yml,_config.production.yml"
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./_site
```

After you commit and push to main branch you can see the Actions
https://github.com/duleorlovic/tailwind_play_responsive_dark_mode/actions

<p align="right">(<a href="#top">back to top</a>)</p>

## Github pages

Enabling Github Pages you get url `your-user-name.github.io/your-repo-name` so
update this jekyll production config
```
# _config.production.yml
url: "https://duleorlovic.github.com"
baseurl: "tailwind_play_responsive_dark_mode"
```

Enable Pages in Settings -> Pages , for example for this repo it is
[/settings/pages](../../settings/pages)
Just select the branch `gh-pages` and leave the `Source` to be `Deploy from a
branch`.

Also you need to enable Actions to have write permissions. Select `Read and write permissions` `Workflows have
read and write permissions in the repository for all scopes.`
![image](https://user-images.githubusercontent.com/1426092/223695663-1d7fefd2-5a50-4c91-9084-91526a82b6e1.png)

After next commit your site should be live for example
https://duleorlovic.github.io/tailwind_play_responsive_dark_mode/

<p align="right">(<a href="#top">back to top</a>)</p>

## Authors ✍️

https://github.com/duleorlovic

## 🤝 Contributors

Contributions, issues, and feature requests are greatly appreciated!

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "improvements".

- Fork the Project
- Create your Feature Branch (git checkout -b feature/yourfeaturename)
- Commit your Changes (git commit -m 'Add suggested feature')
- Push to the Branch (git push origin feature/AmazingFeature)
- Open a Pull Request

Feel free to check the [issues page](https://github.com/DeVuDeveloper/jekyll-tailwind./issues/1).

<p align="right">(<a href="#top">back to top</a>)</p>

## 📝 License

This project is licensed by [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Show your support 💪

Give a ⭐️ if you like this project!

<p align="right">(<a href="#top">back to top</a>)</p>
