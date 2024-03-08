---
title: A Brief Tutorial to Build Homepage
date: 2022-11-11 21:54:11
tags: [Tutorial, Hexo, Homepage, NexT-theme]
categories: 
    - [Tutorial, Homepage, Hexo]
---

## Getting start
### Overview

- [Hexo](https://hexo.io)
    - Hexo is a fast, simple and powerful blog framework. You write posts in Markdown (or other markup languages) and Hexo generates static files with a beautiful theme in seconds. 
    - home
- [NexT](https://theme-next.js.org) 
    - NexT is a high quality elegant theme for Hexo. It is crafted from scratch, with love.

<!-- more -->

#### Requirements
Installing Hexo is quite easy and only requires the following beforehand:
- [Node.js](http://nodejs.org/)
- [Git](http://git-scm.com/)


#### Install Hexo
Once all the requirements are installed, you can install Hexo with npm:
```bash
npm install -g hexo-cli
```

If meet access errors, run the following code installed: 
```bash
sudo npm install --unsafe-perm --verbose -g hexo 
```
### Setup

#### Initialize Hexo

Once Hexo is installed, run the following commands to initialize Hexo in the target `<site-folder>`. 
```bash
cd <github>
hexo init <site-folder>
cd <site-folder>
npm install
```
When using github.io to build your blog, you can replace your `<site-folder>` with `<username>.github.io`. 
- Reminder: please run the code above when there is no folder named as `<site-folder>`. 

Once initialized, here’s what your project folder will look like:
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

#### Install NexT Theme

If you're using Hexo 5.0 or later, the simplest way to install is through `npm`. 
Open your Terminal, change to Hexo <mark>site root directory</mark> and install NexT theme:
```bash
cd <site-folder>
npm install hexo-theme-next
```

#### Config File 
1. Prepare the <mark>NexT config file</mark>
    ```bash
    # Installed through npm
    cp node_modules/hexo-theme-next/_config.yml _config.next.yml 
    ```
2. Edit <mark>Hexo config file</mark>. Open `_config.yml`, and edit
    ```yml
    title: This is title
    subtitle: This is subtitle
    description: This is description
    keywords:
    author: Alice Bob
    language: en # zh-CN
    timezone: ''
    ```
    ```yml
    # URL
    ## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
    url: http://<username>.github.io #http://example.com
    ```
    ```yml
    # Extensions
    ## Plugins: https://hexo.io/plugins/
    ## Themes: https://hexo.io/themes/
    theme: next
    ```
    ```yml
    # Deployment
    ## Docs: https://hexo.io/docs/one-command-deployment
    deploy:
    type: git
    repo: https://github.com/<username>/<username>.github.io
    # example, https://github.com/hexojs/hexojs.github.io
    branch: gh-pages
    ```
3. Install [`hexo-deployer-git`](https://github.com/hexojs/hexo-deployer-git).
    ```bash
    npm install hexo-deployer-git --save
    ```
4. Edit <mark>NexT config file</mark>. Open `_config.next.yml`. 
    1. choose your preferred scheme by deleting the `#` before the scheme and add `#` befor others.
        ```yml
        # Schemes
        # scheme: Muse
        #scheme: Mist
        scheme: Pisces
        #scheme: Gemini
        ```  
    2. You can run the code to preview your website locally.
        ```bash
        hexo server
        ```

#### Setup Deployment
1. Create a repo named `<username>.github.io`, where `<username>` is your username on GitHub. If you have already uploaded to other repo, rename the repo instead.
2. Push the files of your Hexo folder to the default branch of your repository. The default branch is usually **main**, older repository may use **master** branch.
   - The `public/` folder is not (and should not be) uploaded by default, make sure the `.gitignore` file contains `public/` line. 
3. Check what version of Node.js you are using on your local machine with `node --version`. Make a note of the major version (e.g., `v16.y.z`) 
4. Create `.github/workflows/pages.yml` in your repo with the following contents (substituting `16` to the major version of Node.js that you noted in previous step):
```yml
name: Pages

on:
  push:
    branches:
      - main  # default branch

jobs:
  pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js 16.x
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```
5. One-command deployment
    Run
    ```bash
    hexo clean && hexo deploy
    ``` 
6. Once the deployment is finished, the generated pages can be found in the `gh-pages` branch of your repository. 
7. In your GitHub repo’s setting, navigate to **Settings > Pages > Source**. Change the branch to `gh-pages` and save. 
8. Check the webpage at *username.github.io*.


### Customize Your Site
```bash
cd <site-folder> 
```

#### Add an Avatar 
 1.  Create folder `source/uploads`. 
 2. Save your avatar image `avatar.png` in `source/uploads`. 
 3. Edit `_config.next.yml`
    ```yml
    # Sidebar Avatar
    avatar:
      # Replace the default image and set the url here.
      url: /uploads/avatar.png
      # If true, the avatar will be displayed in circle.
      rounded: false
      # If true, the avatar will be rotated with the cursor.
      rotated: false
    ``` 
#### Add `about` menu
If you want to add `about` menu and `about` page, 
1. Run the code
    ```bash
    hexo new page about
    ```
2. Open `source/about/index.md`, and write something you want. 
3. Open `_config.next.yml`, add the `about:` code in `menu:`
    ```yml
    menu: 
      about: /about/ || fa fa-user
    ```

#### Add `tags` menu
If you want to add `tags` menu, 
1. Run the code
    ```bash
    hexo new page tags
    ```
2. Open `source/tags/index.md`, and add `type: tags`
    ```md
    ---
    title: tags
    date: 2022-11-11 19:56:45
    type: tags
    ---
    ```
    You don't need edit this page. 
3. Open `_config.next.yml`, add the `tags:` code in `menu:`
  ```yml
  menu: 
    tags: /tags/ || fa fa-tags
  ```
4. If you want to add tags for other pages, see [Maintain Homepage](#maintain-homepage) for details. 

#### Add `categories` Menu

If you want to add `categories` menu, 
1. Run the code
  ```bash
  hexo new page categories
  ```
2. Open `source/tags/index.md`, and add `type: tags`
    ```md
    ---
    title: categories
    date: 2022-11-11 20:41:43
    type: categories
    ---
    ```
3. Open `_config.next.yml`, add the `categories:` code in `menu:`
    ```yml
    menu: 
      categories: /categories/ || fa fa-th
    ```
4. If you want to add categories for other pages, see [Maintain Homepage](#maintain-homepage) for details.  


#### Create a custom homepage
1. Remove the hexo-generator-index package
```bash
npm remove hexo-generator-index
npm remove hexo-generator-index-pin-top
```
2. You can now put your index page in `/source/index.md`.
```markdown
---
layout: page
title: homepage
permalink: index.html
---
```
3. Open `_config.next.yml`, 
```yml
# index_generator:
#   path: about
#   per_page: 0
#   order_by: -date
```


## Maintain Homepage

### Hexo Command

####  one command deploy
Each time you edit your homepage, and want to deploy, you can run the following code. 
```bash
hexo clean && hexo deploy
```
or 
```bash
hexo clean && hexo d
```

#### preview website
If you want to preview your website locally, you can run
```bash
hexo server
```
or 
```bash
hexo clean && hexo g && hexo s
```

### Writing Articles

#### new article
```bash
hexo new <post-name>
```
Then, you can write your article in the file `source/_posts/<post-name>.md`. 

#### article folding in home page
Add the following code just after what you want to show on the index page in your article `**.md`.
```md
<!-- more -->
```

#### tags
```
tags: [tag1,tag2]
```
or 
```
tags: 
    - tag1
    - tag2
```

#### categories
```
categories: 
    - cate1
    - cate1-1
    - cate1-1-1
```
or multiple catecories: 
```
categories: 
    - [cate1, cate1-1, cate1-1-1]
    - [cate1, cate1-2]
    - [cate2, cate2-1]
    - cate3
```

#### math equation

{% tabs math equation, 2 %}
<!-- tab Prepare -->
1. Open `_config.next.yml`, and edit
    ```yml
    # Math Formulas Render Support
    math:
      # Default (false) will load mathjax / katex script on demand.
      # That is it only render those page which has `mathjax: true` in front-matter.
      # If you set it to true, it will load mathjax / katex script EVERY PAGE.
      every_page: false

      mathjax:
        enable: true
        # Available values: none | ams | all
        tags: none

      katex:
        enable: false
        # See: https://github.com/KaTeX/KaTeX/tree/master/contrib/copy-tex
        copy_tex: false
    ```
    We choose mathjax as our render engine.
    > - For now, NexT provides two rendering engines for displaying Math Equations: [MathJax](https://www.mathjax.org/) and [KaTeX](https://katex.org/).
    >   - MathJax is a JavaScript display engine for mathematics that works in all browsers. It is highly modular on input and output. Use MathML, TeX, and ASCIImath as input and produce HTML+CSS, SVG, or MathML as output.
    >   - [KaTeX is a faster](https://www.intmath.com/cg5/katex-mathjax-comparison.php) math rendering engine compared to MathJax 3. And it could survive without JavaScript. But, for now [KaTeX supports less features](https://github.com/KaTeX/KaTeX/wiki/Things-that-KaTeX-does-not-(yet)-support) than MathJax. Here is a list of [TeX functions supported by KaTeX](https://katex.org/docs/supported.html).
2. Then you need to uninstall the original renderer `hexo-renderer-marked`, and install `hexo-renderer-pandoc`:
    ```bash
    npm un hexo-renderer-marked
    npm i hexo-renderer-pandoc
    ```
3. [pandoc](https://github.com/jgm/pandoc) is required for hexo-renderer-pandoc, here's [how to install pandoc](https://github.com/jgm/pandoc/blob/master/INSTALL.md).
4. Numbering and Referring Equations in MathJax
   1. NexT v6.3.0 has added feature to [automatic equation numbering](https://docs.mathjax.org/en/latest/input/tex/eqnumbers.html) with opportunity to make reference to that equations.
   2. To enable this feature, you need to set `mathjax.tags` to `ams` in `_config.next.yml`.
        ```yml
        math:
          mathjax:
            enable: true
            # Available values: none | ams | all
            tags: ams
        ```
<!-- endtab -->

<!-- tab usage -->
If this is your first time inserting math equations, please see **Prepare** first. 
1. Insert to your `<post-name>.md`
    ``` 
    mathjax: true 
    ```
2. Add your equation by `\(x+y\)` or `\[x+y\]`
<!-- endtab -->
{% endtabs %}

#### images
1. Create a folder named `<post-name>` which is the same as your article `<post-name>.md`
    - or you can edit `_config.yml`
        ```yml
        post_asset_folder: true 
        ```
        Then, each time you `hexo new <post-name>`, there will be a folder named `<post-name>` created at the same time
2. save your `<image-name>.jpg` in folder `./source/_posts/<post-name>/` 
    ```
    ![](<image-name>.jpg)
    ```
    - <mark>Reminder</mark>: using this method, you won't have a correct image preview on your home page. So please insert your image after `<!-- more -->`

#### add tabs
- usage. [(see details)](https://theme-next.js.org/docs/tag-plugins/tabs.html)
    ```
    {% tabs Unique name, [index] %}
    <!-- tab [Tab caption] [@icon] -->
    Any content (support inline tags too).
    <!-- endtab -->
    {% endtabs %}
    ``` 
- examples
  - ex
    ```
    {% tabs First unique name, -1 %}
    <!-- tab -->
    **This is Tab 1.**
    <!-- endtab -->

    <!-- tab -->
    **This is Tab 2.**
    <!-- endtab -->

    <!-- tab haha -->
    **This is Tab 3.**
    <!-- endtab -->
    {% endtabs %}
    ```
    
    {% tabs Third unique name, -1 %}
    <!-- tab -->
    **This is Tab 1.**
    <!-- endtab -->

    <!-- tab -->
    **This is Tab 2.**
    <!-- endtab -->

    <!-- tab haha -->
    **This is Tab 3.**
    <!-- endtab -->
    {% endtabs %}
- 

### Upgrading

#### upgrading NexT
```bash
cd <hexo-site>
npm install hexo-theme-next@latest
```
