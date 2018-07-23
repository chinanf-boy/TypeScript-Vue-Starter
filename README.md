### 校对✅

<details>

<summary> ⏰ 2018 7.22 开始 </summary>


**1/1**

- ⏰ 2018 7.23 结束 ^_^

</details>

---

## 目录
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [TypeScript Vue Starter](#typescript-vue-starter)
- [初始化您的项目](#%E5%88%9D%E5%A7%8B%E5%8C%96%E6%82%A8%E7%9A%84%E9%A1%B9%E7%9B%AE)
- [初始化该项目](#%E5%88%9D%E5%A7%8B%E5%8C%96%E8%AF%A5%E9%A1%B9%E7%9B%AE)
- [安装我们的依赖项](#%E5%AE%89%E8%A3%85%E6%88%91%E4%BB%AC%E7%9A%84%E4%BE%9D%E8%B5%96%E9%A1%B9)
- [添加 TypeScript 配置文件](#%E6%B7%BB%E5%8A%A0-typescript-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
- [添加 Webpack](#%E6%B7%BB%E5%8A%A0-webpack)
- [添加构建脚本](#%E6%B7%BB%E5%8A%A0%E6%9E%84%E5%BB%BA%E8%84%9A%E6%9C%AC)
- [创建一个基本项目](#%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%9F%BA%E6%9C%AC%E9%A1%B9%E7%9B%AE)
- [添加组件](#%E6%B7%BB%E5%8A%A0%E7%BB%84%E4%BB%B6)
- [单个文件组件](#%E5%8D%95%E4%B8%AA%E6%96%87%E4%BB%B6%E7%BB%84%E4%BB%B6)
- [使用 装饰器-decorators 定义组件](#%E4%BD%BF%E7%94%A8-%E8%A3%85%E9%A5%B0%E5%99%A8-decorators-%E5%AE%9A%E4%B9%89%E7%BB%84%E4%BB%B6)
- [接下来是什么?](#%E6%8E%A5%E4%B8%8B%E6%9D%A5%E6%98%AF%E4%BB%80%E4%B9%88)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# TypeScript Vue Starter

本快速入门指南,将教您如何让 TypeScript和[Vue](https://vuejs.org)一起工作. 本指南非常灵活,不管你位于本指南的哪个步骤, 都可以让你把 TypeScript集成到 现有的Vue项目中. 

# 初始化您的项目

让我们创建一个新项目包. 

```sh
mkdir typescript-vue-tutorial
cd typescript-vue-tutorial
```

接下来,我们将通过以下架构,构建项目: 

```txt
typescript-vue-tutorial/
├─ dist/
└─ src/
   └─ components/
```

TypeScript文件将从您的`src`文件夹开始,运行T ypeScript编译器,然后运行 webpack,最后`dist`文件夹有了个`bundle.js`. 我们写的任何组件都会进入`src/components`文件夹. 

让我们来继续吧: 

```shell
mkdir src
cd src
mkdir components
cd ..
```

Webpack最终将生成我们的`dist`目录. 

# 初始化该项目

现在我们将把这个文件夹变成一个 npm包裹. 

```shell
npm init
```

您将获得一系列提示. 除 入口点{entry point} 外,您都可以使用默认值. 您可以随时返回,并更改这些内容在`package.json` - 已为您生成的文件. 

# 安装我们的依赖项

确保安装 TypeScript,Webpack,Vue和必要的加载器. 

```sh
npm install --save-dev typescript webpack ts-loader css-loader vue vue-loader vue-template-compiler
```

Webpack 是一个将 代码和可选的所有依赖项 捆绑在单个`.js`文件的工具. 虽然您不需要使用像 Webpack或Browserify 这样的捆绑器,但这些工具将允许我们使用`.vue`文件 - 我们稍后会介绍. 

我们不需要[加入 `.d.ts`「声明文件」 文件](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html),但如果我们使用的是 未发送声明文件 的软件包,我们需要安装相应的软件包`@types/`包. [了解更多 - 声明文件{官方}](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html). 

# 添加 TypeScript 配置文件

您需要将 TypeScript文件 放在一起 - 您要编写的代码以及任何必要的 声明文件. 

要做到这一点,你需要创建一个`tsconfig.json`,其中包含 输入文件列表 以及 所有编译设置. 在项目根目录中创建一个名为`tsconfig.json`的新文件,并填写以下内容: 

```json
{
    "compilerOptions": {
        "outDir": "./built/",
        "sourceMap": true,
        "strict": true,
        "noImplicitReturns": true,
        "module": "es2015",
        "moduleResolution": "node",
        "target": "es5"
    },
    "include": [
        "./src/**/*"
    ]
}
```

请注意`strict`是设置为true. 至少,TypeScript的`noImplicitThis`需要打开,来影响 Vue的声明文件,但是`strict`给了我们更多选项1 (比如`noImplicitAny`和`strictNullChecks`) . 我们强烈建议您使用TypeScript 更 严格-strict 的模式选项以获得更好的体验. 

# 添加 Webpack

我们需要添加一个`webpack.config.js`捆绑我们的应用程序. 

```js
var path = require('path')
var webpack = require('webpack')

module.exports = {
  entry: './src/index.ts',
  output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: '/dist/',
    filename: 'build.js'
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          loaders: {
            // 由于 sass-loader（很奇怪）将 SCSS 作为其默认的解析模式，
            // 我们映射
            // “scss”和“sass”语言值位于 配置的右边。
            // 其他预处理器应该开箱即用，没有像这样的加载器配置。
            'scss': 'vue-style-loader!css-loader!sass-loader',
            'sass': 'vue-style-loader!css-loader!sass-loader?indentedSyntax',
          }
          // 其他 vue-loader 选项就在这里
        }
      },
      {
        test: /\.tsx?$/,
        loader: 'ts-loader',
        exclude: /node_modules/,
        options: {
          appendTsSuffixTo: [/\.vue$/],
        }
      },
      {
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'file-loader',
        options: {
          name: '[name].[ext]?[hash]'
        }
      }
    ]
  },
  resolve: {
    extensions: ['.ts', '.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    }
  },
  devServer: {
    historyApiFallback: true,
    noInfo: true
  },
  performance: {
    hints: false
  },
  devtool: '#eval-source-map'
}

if (process.env.NODE_ENV === 'production') {
  module.exports.devtool = '#source-map'
  // http://vue-loader.vuejs.org/en/workflow/production.html
  module.exports.plugins = (module.exports.plugins || []).concat([
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      sourceMap: true,
      compress: {
        warnings: false
      }
    }),
    new webpack.LoaderOptionsPlugin({
      minimize: true
    })
  ])
}
```

# 添加构建脚本

打开你的`package.json`,并添加一个`build`来运行Webpack. 你的`"scripts"`字段应该看起来像这样: 

```json
"scripts": {
    "build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

一旦我们添加了一个入口文件,我们就可以通过运行,来构建

```sh
npm run build
```

并通过运行下面的,观察更改

```sh
npm run build -- --watch
```

# 创建一个基本项目

让我们创建一个最简单的 Vue和TypeScript 示例 来尝试一下. 首先,创建文件`./src/index.ts`: 

```ts
// src/index.ts

import Vue from "vue";

let v = new Vue({
    el: "#app",
    template: `
    <div>
        <div>Hello {{name}}!</div>
        Name: <input v-model="name" type="text">
    </div>`,
    data: {
        name: "World"
    }
});
```

我们来检查一切`<>`是否正确连线. 在您的根目录中创建一个`index.html`,包含以下内容: 

```html
<!doctype html>
<html>
<head></head>

<body>
    <div id="app"></div>
</body>
<script src="./dist/build.js"></script>

</html>
```

现在运行`npm run build`,并在浏览器中输入,打开你的`index.html`文件. 

你应该看到一些文字说`Hello World!`. 在此之下,您将看到一个文本框. 如果更改文本框的内容,您将注意到在两者之间是同步的文本. 

恭喜!你已经完全搞定了 TypeScript和Vue! 

# 添加组件

正如您刚才所见,Vue有一个非常简单的界面,可用于完成简单任务. 当我们的页面只需要在两个元素之间传递一些数据时,它只花了很少的代码. 

对于更复杂的任务,Vue非常灵活,因为它支持整顿您的应用程序变成*组件*. [Components](https://vuejs.org/v2/guide/components.html)对于将实体如何显示给用户的问题分开是有用的. [了解更多 vue组件.](https://vuejs.org/v2/guide/components.html)

可以通过以下方式 声明Vue组件: 

```ts
// src/components/Hello.ts

import Vue from "vue";

export default Vue.extend({
    template: `
        <div>
            <div>Hello {{name}}{{exclamationMarks}}</div>
            <button @click="decrement">-</button>
            <button @click="increment">+</button>
        </div>
    `,
    props: ['name', 'initialEnthusiasm'],
    data() {
        return {
            enthusiasm: this.initialEnthusiasm,
        }
    },
    methods: {
        increment() { this.enthusiasm++; },
        decrement() {
            if (this.enthusiasm > 1) {
                this.enthusiasm--;
            }
        },
    },
    computed: {
        exclamationMarks(): string {
            return Array(this.enthusiasm + 1).join('!');
        }
    }
});
```

该组件有 两个按钮 和 一些文本. 渲染时,它需要初始值`name`和`initialEnthusiasm`
这是我们想要显示的感叹号的数量. 
当我们击中`+`按钮,它在文本的末尾添加一个感叹号. 同样,当我们击中时`-`按钮,它会删除一个感叹号,
除非我们只是一个感叹号. 

我们的 根 Vue实例 可以按 如下方式使用它: 

```ts
// src/index.ts

import Vue from "vue";
import HelloComponent from "./components/Hello";

let v = new Vue({
    el: "#app",
    template: `
    <div>
        Name: <input v-model="name" type="text">
        <hello-component :name="name" :initialEnthusiasm="5" />
    </div>
    `,
    data: { name: "World" },
    components: {
        HelloComponent
    }
});
```

但是,我们会注意到它的使用相当普遍[Vue 的*单文件组件*](https://vuejs.org/v2/guide/single-file-components.html). 让我们尝试将上述内容写成SFC. 

# 单个文件组件

使用 Webpack或Browserify 时,Vue有插件[vue-loader](https://github.com/vuejs/vue-loader)和[vueify](https://www.npmjs.com/package/vueify)
它允许您在类似 HTML的文件 中创作组件. 这些文件以`.vue`扩展结尾,是单个文件组件. 

这里需要搞些事情,才能使用`.vue`与TypeScript 的文件,但幸运的是我们已经在那里完成一半了. 我们在获得 开发依赖项-{dev dependencies}时 已经安装了 vue-loader. 我们还指定了`appendTsSuffixTo: [/\.vue$/]`,同时我们的`webpack.config.js`文件中`ts-loader`选项,它允许 TypeScript 处理从 单个文件组件 中提取的代码. 

我们要做的另一件事是告诉 TypeScript,`.vue`文件在导入时是什么样子滴. 我们这样做是为了`vue-shims.d.ts`文件: 

```ts
// src/vue-shims.d.ts

declare module "*.vue" {
    import Vue from "vue";
    export default Vue;
}
```

我们不需要在任何地方导入此文件. 它被 TypeScript自动包含,并告诉它 任何 `.vue`导入的内容都具有与 Vue构造函数 本身相同的形状. 

还剩下什么? 编辑体验! TypeScript为我们提供的最佳功能之一,是它完善的编辑器支持. 为了用好`.vue`文件,我们建议使用[Visual Studio Code](https://code.visualstudio.com/)和随着[Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur)的Vue插件. 

现在,让我们写一个SFC!

```html
<!-- src/components/Hello.vue -->

<template>
    <div>
        <div class="greeting">Hello {{name}}{{exclamationMarks}}</div>
        <button @click="decrement">-</button>
        <button @click="increment">+</button>
    </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
    props: ['name', 'initialEnthusiasm'],
    data() {
        return {
            enthusiasm: this.initialEnthusiasm,
        }
    },
    methods: {
        increment() { this.enthusiasm++; },
        decrement() {
            if (this.enthusiasm > 1) {
                this.enthusiasm--;
            }
        },
    },
    computed: {
        exclamationMarks(): string {
            return Array(this.enthusiasm + 1).join('!');
        }
    }
});
</script>

<style>
.greeting {
    font-size: 20px;
}
</style>
```

然后让我们在 根实例 导入它: 

```ts
// src/index.ts

import Vue from "vue";
import HelloComponent from "./components/Hello.vue";

let v = new Vue({
    el: "#app",
    template: `
    <div>
        Name: <input v-model="name" type="text">
        <hello-component :name="name" :initialEnthusiasm="5" />
    </div>
    `,
    data: { name: "World" },
    components: {
        HelloComponent
    }
});
```

请注意我们的单文件组件的一些事项: 

-   我们不得不写`<script lang="ts">`来使用TypeScript. 
-   我们不得不在`index.ts`导入组件`.vue`扩展. 
-   我们能够将 CSS 隔离到我们的组件中`<style>`标签,我们无法在`.ts`组件做到. 
-   我们默认导出的是`Vue.extend` (而不是 Vue 本身) . 如果你不写`Vue.extend`,Vetur 会让它看起来像是正常工作,但是在构建项目时会出现错误. 

试试运行`npm run build`,并打开`index.html`看到结果!

# 使用 装饰器-decorators 定义组件

组件也可以使用[decorators](https://www.typescriptlang.org/docs/handbook/decorators.html)定义. 在另外两个包, ([vue-class-component](https://github.com/vuejs/vue-class-component)和[vue-property-decorator](https://github.com/kaorun343/vue-property-decorator))的帮助下 ,我们的组件可以通过以下方式重写: 

```ts
import { Vue, Component, Prop } from "vue-property-decorator";

@Component
export default class HelloDecorator extends Vue {
    @Prop() name!: string;
    @Prop() initialEnthusiasm!: number;

    enthusiasm = this.initialEnthusiasm;

    increment() {
        this.enthusiasm++;
    }
    decrement() {
        if (this.enthusiasm > 1) {
            this.enthusiasm--;
        }
    }

    get exclamationMarks(): string {
        return Array(this.enthusiasm + 1).join('!');
    }
}
```

已经不是使用`Vue.extend`来定义我们的组件,我们创建了一个类来扩展和装饰`Vue`,用于`vue-class-component`包的`@Component`装饰器 (从中重新导出 `vue-property-decorator`包) . 

通过使用 实例变量 加 前缀`@Prop() - {来自`vue-property-decorator`包的装饰器}`来定义属性. 

因为`--strictPropertyInitialization`选项打开,我们需要告诉TypeScript , 有关 Vue 将通过 添加一个`!` 来初始化我们的属性 给他们. 这告诉 TypeScript "嘿,放松,别人会给这个属性分配一个值. "

常规实例变量,例如在我们的示例中的`enthusiasm`,自动用于 绑定到 template 的数据,就像它们已经在 template 中定义的`data`字段一样. 请注意,所有变量必须设置为 除`undefined`以外的值 来使绑定工作. 

同样,方法如`increment`被视为写在`methods`字段,并自动为 template 提供. 

最后,`计算-computed`属性如`exclamationMarks`简单地写成`get`存取. 

# 接下来是什么?

您可以[从 Github 中 clone 这个项目来试试](https://github.com/DanielRosenwasser/typescript-vue-tutorial). 

一旦你觉得你已经掌握了它,你可以试试更复杂的[TodoMVC风格 应用来自TypeScript 和 Vue](https://github.com/DanielRosenwasser/typescript-vue-todomvc). 这个TodoMVC风格 的 样本 具有路由功能[vue-router](https://github.com/vuejs/vue-router)以便您的应用程序可以根据当前URL 显示不同的视图. 

您可能还想了解一下[Vuex](https://github.com/vuejs/vuex),如果你正在寻找[Redux](http://redux.js.org/)式的状态管理的话. 
