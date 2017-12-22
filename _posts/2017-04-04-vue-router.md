---
layout: post
title: vue router
date: 2017-04-04
categories: vue
tags: grinding
---

12am here. let's go.

steps to add vue-router
---

1. npm i -s vue-router
2. `main.js`:

```
import VueRouter from 'vue-router'
import { routes } from './routes'

Vue.use(VueRouter);

const router = new VueRouter({
  routes
})

new Vue({
  el: '#app',
  router,
  render: h => h(App)
})
```

3. `routes.js`

```
import User from './components/user/User.vue';
import Home from './components/Home.vue';

export const routes = [
    { path: '/', component: Home},
    { path: '/user', component: User}
]
```

4. `App.vue` 

template: add `<router-view></router-view>`
script: add `import Header from './components/Header.vue';`

5. `Header.vue` template:

```
<template>
    <ul class="nav nav-pills">
    <router-link to="/" tag="li" active-class="active" exact><a>Home</a></router-link>
    <router-link to="/user" tag="li" active-class="active"><a>Messages</a></router-link>
    </ul>
</template>
```

that's it!

to do this programatically execute: `this.$router.push({ path: '/' });`

---

2am. finished basic routing knowledge and left routing animations.
