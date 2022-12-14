---
Author: sync
date: 2022-11-06 22:15 Sunday
tag: obsidian/mkdocs 
---

## 1. 如何创建一个 mkdocs 项目

- 官方文档： <https://squidfunk.github.io/mkdocs-material/>
- 优秀项目： <https://github.com/ObsidianPublisher/obsidian-mkdocs-publisher-template>
- 中文教程（mkdocs 版本太老了）： <https://github.com/Jackiexiao/mkdocs-roamlinks-plugin>
- mkdocs 入门视屏： [Python 版宝藏级静态站点生成器 Material for MkDocs](https://www.bilibili.com/video/BV1nt4y157sR/?spm_id_from=..search-card.all.click&vd_source=ab2f866858f0016a32f6db0daf3438df)

## 2. 修改配置文件

```yaml
# official tutorial 
# mkdocs-material: https://squidfunk.github.io/mkdocs-material/
# mkdocs: https://www.mkdocs.org/user-guide/configuration/

site_name: SYNC

theme:
  name: material
  logo: FigureBed 🌄/source/cat.jpg
  favicon: FigureBed 🌄/source/github.png
  language: zh

  # 调色板
  palette: 
    # Palette toggle for light mode
    - scheme: default
      primary: teal
      accent: light blue
      toggle:
        icon: material/weather-night
        name: Switch to dark mode

    # Palette toggle for dark mode
    - scheme: slate
      primary: indigo
      accent: indigo
      toggle:
        icon: material/weather-sunny
        name: Switch to light mode
  
  # 字体
  font:
    # text: Noto Sans Simplified Chinese
    text: Ubuntu
    code: Ubuntu Mono

  # 导航、目录
  features:
    - navigation.tabs
    - navigation.top
    - toc.follow
    - search.suggest
    - search.highlight

markdown_extensions: 
  - attr_list # https://squidfunk.github.io/mkdocs-material/reference/images/
  - pymdownx.tabbed  # https://squidfunk.github.io/mkdocs-material/reference/content-tabs/
  - nl2br # newline-to-break 
  - toc:
      permalink: '#' # heading anchor 
      slugify: !!python/name:pymdownx.slugs.uslugify # 解决中文标题解析问题
  - admonition
  - codehilite:
      guess_lang: false
      linenums: false
  - footnotes
  - meta
  - def_list
  - pymdownx.arithmatex
  - pymdownx.betterem:
      smart_enable: all
  - pymdownx.caret
  - pymdownx.critic
  - pymdownx.details
  - pymdownx.emoji:
      emoji_index: !!python/name:materialx.emoji.twemoji 
      emoji_generator: !!python/name:materialx.emoji.to_svg
  - pymdownx.inlinehilite
  - pymdownx.magiclink
  - pymdownx.mark
  - pymdownx.smartsymbols
  - pymdownx.superfences
  - pymdownx.tasklist:
      custom_checkbox: true 
  - pymdownx.tilde
  - pymdownx.highlight:
      auto_title: true # 为代码块添加标题
      linenums: true # 行号
  - tables
  - pymdownx.snippets

plugins:
  - search
  - awesome-pages
  - roamlinks
    #- autolinks 
  - exclude:
      glob:
        - "*.tmp"
        - "*.pdf"
        - "*.gz"
      regex:
        - '.*\.(tmp|bin|tar)$'
  

# 脚注
extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/littlecabbage
      name: github of sync
    - icon: fontawesome/solid/paper-plane
      link: mailto:<zengzh1997@126.com>
  generator: false # hide "generte by  mkdocs"

extra_css:
    - https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css
    - assets/css/template/blog.css
    - assets/css/template/grid.css
    - assets/css/template/img-grids-floats.css
    - assets/css/template/tooltipster.bundle.min.css
    - assets/css/template/utils.css
    - assets/css/admonition.css
    - assets/css/custom_attributes.css
    - assets/css/customization.css
extra_javascript:
  - assets/js/mathjax.js
  - assets/js/utils.js
  - https://polyfill.io/v3/polyfill.min.js?features=es6
  - https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js
  - assets/js/tooltipster.bundle.js  

copyright: Copyright &copy; 2022 SYNC

```

## 3. 注意事项

### 3.1 目录

[基于mkdocs-material搭建个人静态博客(含支持的markdown语法)](https://cyent.github.io/markdown-with-mkdocs-material/syntax/headline/)

- 一个`.md` 里只能有一个 `#`，跟着多个 `##`。如果有多个 `#`，则不会自动生产本页目录。
- 如果有 `#`，则使用该标题作为本页正文部分第一行，如果没有 `#`，则为 `mkdocs.yml` 里指定的 pages 名。
- 个人建议 `.md` 从 `##` 开始，不要用 `#`。

### 3.2 高亮

[代码高亮](https://cyent.github.io/markdown-with-mkdocs-material/syntax/highlight_code/#_1)
依赖模块: pymdownx.inlinehilite

使用shebang可以在一行文本里实现代码高亮

```text
`#!python print "Hello, world!"`或`:::python print "Hello, world!"`
```
