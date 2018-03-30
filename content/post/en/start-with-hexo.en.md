---
author: "Alex Yu"
date: 2018-03-19
title: "Start with Hexo"
tags: ["Hexo", "Next"]
categories: ["Development"]
---

# Why I start with Hexo instead of Jekyll or Hugo

At the beginning, I just want to create my blog with GitHub page without other complicated setup. At first, Jekyll is the first result came out from the search. I knew Jekyll has many pros and to me, its biggest pro is that

> Jekyll has the largest community among other static generators that has provided a plethora of great tutorials, open source themes, and plugins.

But its cons also hesitate me to use it. If I want to maintain my blog in a long-term, then I do want the build speed to be fast. And if I want to use Jekyll, I have to install Ruby on Rail then. In my PC setup, I only have <span class="NodeJS">Node.js</span>, so I don’t want to install another package manager if I just want to create a simple blog. ~~Similar reason to why I didn’t choose Hugo~~, although Hugo's build speed is way fast than Jekyll.

# How to install Hexo

make sure you have npm installed, and simply type

    npm install -g hexo

Before install the theme, needs to create the blog folder. After hexo is installed, type

    hexo init blog
Then enter that folder and isntall the package

    cd blog
    npm install
After installation, we can run the server by

    hexo server

# How to install Next Theme

in the blog path /blog, type

    git clone https://github.com/theme-next/hexo-theme-next themes/next


## Add Disqus comment

In `blog/_config.yml`, add your Disqus shartname

    # Disqus
    disqus_shortname: your-disqus-id

Then in `themes/next/_config.yml` find Disqus section

    # Disqus
    disqus:
    enable: true
    shortname: your-disqus-id
    count: true
    lazyload: false

# Credits
* https://www.techiediaries.com/jekyll-hugo-hexo/
* https://hexo.io/
* https://github.com/theme-next/hexo-theme-next
* https://oawan.me/2016/easy-hexo-easy-blog/