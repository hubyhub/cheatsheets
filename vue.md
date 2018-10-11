# Vue.js


## CLI - Installation

```bash
#https://cli.vuejs.org/guide/installation.html
npm install -g @vue/cli
# Vue CLI requires Node.js version 8.9 or above (8.11.0+ recommended).
```

## Create new project
```bash
 
 vue create hello-world

```
## Start built-in dev Server

The vue-cli-service serve command starts a dev server (based on webpack-dev-server) that comes with Hot-Module-Replacement (HMR) working out of the box.

```bash

npm run serve

```

## Built for production

`vue-cli-service build` produces a production-ready bundle in the dist/ directory, with minification for JS/CSS/HTML and auto vendor chunk splitting for better caching. The chunk manifest is inlined into the HTML.
```
npm run build
```

## Git Hooks
See [https://cli.vuejs.org/guide/cli-service.html](https://cli.vuejs.org/guide/cli-service.html)

## VUE UI
Starts vue GUI for managing your projects, install plugins (vuetify, eslint,...)
```bash
vue ui
```





