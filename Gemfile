theme: jekyll-theme-chirpy
lang: zh-CN
timezone: Asia/Shanghai

url: "https://liver0377.github.io"
baseurl: ""
avatar: /assets/images/avatar.png

github:
  username: liver0377

social:
  name: liver0377
  links:
    - https://:github.com/liver0377

toc: true

paginate: 10

collections:
  tabs:
    output: true
    sort_by: order

defaults:
  - scope:
      path: ""
      type: posts
    values:
      layout: post
      comments: true
      toc: true
      permalink: /posts/:title/
  - scope:
      path: ""
      type: tabs
    values:
      layout: page
      permalink: /:title/

kramdown:
  footnote_backlink: "&#8617;&#xfe0e;"
  syntax_highlighter: rouge
  syntax_highlighter_opts:
    css_class: highlight
    span:
      line_numbers: false
    block:
      line_numbers: true
      start_line: 1

sass:
  style: compressed

plugins:
  - jekyll-feed
  - jekyll-seo-tag
  - jekyll-sitemap

exclude:
  - Gemfile
  - Gemfile.lock
  - node_modules
  - vendor
  - "*.gem"
  - "*.gemspec"
