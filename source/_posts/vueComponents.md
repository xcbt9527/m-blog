---
title: vue组件使用学习
tags:
  - vue
description: vue组件使用 vue组件写法 vue引入组件 vue使用组件 
abbrlink: ca90b71e
date: 2019-01-22 16:44:44
---
  在日常的开发中，经常会碰到很多可复用性的`div`来进行重复的开发，进行各种`copy`，项目大起来以后，就很难进行维护了。如果需要更改一个样式。可能会影响到其他页面的使用。所以我们就经常会用到我们的`组件`开发了。
  <!-- more -->
### 全局引入`组件`
  - 在`src/components`下新建一个`header`文件夹并新建`index.vue`、`index.scss`、`index.js`文件
      ```

      <!-- src/components/header/index.vue -->
      <template>
      <header>
        {{title}}
      </header>
      </template>
      <!-- 引入scss -->
      <!-- 如果是组件，建议都使用 scoped 来避免其他样式渲染更改 -->
      <style lang="scss" rel="stylesheet/scss" scoped>
        @import './index.scss';
      </style>
      <!-- 引入js -->
      <script type="text/babel">
        import vm from './vm';
        export default vm;
      </script>

      <!-- src/components/header/index.scss -->
        header {
          height: 100px;
          width: 100px;
          margin: auto;
          display: flex;
          align-items: center;
          justify-content: center;
        }
        <!-- src/components/header/index.js -->
        export default {
          name: 'header-view',
          data() {
            return {};
          },
          props: {
            value: {
              type: String,
              default: '这是头部',
            },
          },
          watch: {},
          mounted() {},
          methods: {},
        };

      ```
  - 在`src/components`下新建一个`components.js`用于管理全局组件的引入
    ```

    <!-- src/components/components.js -->
      import headerView from './header';
      export default (Vue)=>{
        <!-- 此处的 headerView.name 是获取 src/components/header/index.js里面的name,第二个是需要引入的组件 -->
          Vue.component(headerView.name,headerView);
        <!-- 也可以直接指定引入组件的名字 -->
        <!-- Vue.component("header-view",headerView); -->
      }

    ```
  - 在`main.js`里面使用组件
    ```
      <!-- src/main.js -->
        import components from "./components/components.js";
        Vue.use(components);
    ```
  - 使用组件
    ```

      <header-view title="这是组件的使用"></header-view>

    ```
### 局部引入`组件`
  - 在需要引入组件文件夹下面新建一个文件夹 `components` 然后新建需要的组件，如上述新建`header` 组件一样分别新建,如在`index`下需要使用局部组件
    ```

      src/components/index/components/article/article.vue
      src/components/index/components/article/article.js
      src/components/index/components/article/article.scss

    ```
  - 引入组件
    在`src/components/index/components/index/index.js`引入组件并挂载
    ```

      import articleView from './components/article/index';
    <!-- 然后使用组件钩子函数引入组件 -->
    export default {
      name: 'index',
      data() {
        return {};
      },
      components: {
        articleView,
      },
      mounted() {},
      methods: {}
    }
    ```
  - 使用组件
    ```

      <!-- 在 src/components/index/components/index/index.vue 里使用 组件 -->
      <!-- 同一组件可在页面引入多个 -->
      <article-view title="我是组件"></view>
      <article-view title="我是组件1"></view>
      <article-view title="我是组件2"></view>
      
    ```