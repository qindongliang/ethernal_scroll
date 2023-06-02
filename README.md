# Ethernal scroll of Random Users

原应用如果滚动加载本次请求无数据的时候，再次滚动就不会生效，是存在问题的，所以该分支优化增加了基于鼠标滚轮的时间监听机制，
如果div的scorll滚动事件没有加载到数据的时候，依然可以手动触发鼠标滚轮或者触摸板的下滑操作去下拉去刷新数据

This is a very simple proof of the concept of Infinite Scroll written in Vue.JS.

As an ethernal source of random data we have used: [Random Data API](https://random-data-api.com/)

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
