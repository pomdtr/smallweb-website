---
# https://vitepress.dev/reference/default-theme-home-page
layout: home
title: Smallweb – Your Internet Folder
head:
  - [
      'meta',
      {
        name: 'og:title',
        content: 'Smallweb – Your internet folder'
      }
    ]
  - [
      'meta',
      {
        property: 'og:description',
        content: 'A personal cloud contained in a single directory.',
      }
    ]
hero:
  name: "Smallweb"
  text: "Your Internet Folder"
  tagline: A personal cloud contained in a single directory.
  image:
    src: /smallweb_vtuber.webp
    alt: Smallweb
  actions:
    - theme: brand
      text: Get Started
      link: /docs/getting-started
    - theme: alt
      text: Open Demo
      link: https://smallweb.club

features:
  - icon: 📂
    title: File-Based Hosting
    details: Smallweb maps subdomains to subfolders in your smallweb directory. Creating a new app is as simple as running mkdir.
  - icon: ⚡
    title: Instant Feedback
    details: No build or deploy step is required. Just save your changes and refresh your browser.
  - icon: 🔐
    title: Secure by Default
    details: Each smallweb app is sandboxed using Deno. By default, it only has access to its own folder.
  - icon: 🚀
    title: Deploy Anywhere
    details: The whole smallweb codebase is compiled to a single binary. You can run it on your local machine, a remote server, or both.
---

