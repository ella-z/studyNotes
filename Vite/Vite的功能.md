# Vite 的功能

### NPM 依赖解析和预构建

- 原生 ES 导入不支持以下这种的裸模块导入：
  ```
   import { someMethod } from 'my-dep'
  ```
- Vite 会检测所有的源文件中是否有此类裸模块导入，并执行一下操作：
  1.  预购建，这一步由 esbuild 执行，主要作用就是为了提升页面重载的速度，并将 CommonJS/UMD 转换为 ESM 格式。
  2.  将裸模块导入语句重写为导入为合法的 URL。 例如:import { someMethod } from 'my-dep' 👉 import { someMethod } from '/node_modules/.vite/my-dep.js?v=f3sf2ebd'
- 主要实现两个目的：
  1. CommonJS 和 UMD 兼容性：开发阶段中，Vite 的开发服务器将所有代码视为原生 ES 模块。因此，Vite 必须先将作为 CommonJS 或 UMD 发布的依赖项转换为 ESM。
  2. 性能：Vite 将有许多内部模块的 ESM 依赖关系转换为单个模块，以提高后续页面加载性能。

#### 自动依赖寻找

- 如果没有找到相应的缓存，Vite 将抓取你的源码，并自动寻找引入的依赖项，并将这些依赖项作为预构建包的入口点。
- 当服务器已经启动之后，如果遇到一个新的依赖关系导入，而这个依赖关系还没有在缓存中，Vite 将重新运行依赖构建进程并重新加载页面。

#### 缓存

- **文件系统缓存**
  - Vite 会将预构建依赖缓存到 node_modules/.vite 。它根据几个源来决定是否需要运行预构建步骤:
    1.  package.json 中的 dependencies 列表。
    2.  包管理器的 lockfile，例如 package-lock.json, yarn.lock，或者 pnpm-lock.yaml。
    3.  可能在 vite.config.js 相关字段中配置过的。
    - 只有当上面的一个步骤发生变化时，才需要重新运行预构建步骤。
  - 如果出于某些原因，你想要强制 Vite 重新绑定依赖，你可以用 --force 命令行选项启动开发服务器，或者手动删除 node_modules/.vite 目录。
- **浏览器缓存**
  - 解析后的依赖请求会以 HTTP 头 max-age=31536000,immutable 强缓存，以提高在开发时的页面重载性能。一旦被缓存，这些请求将永远不会再到达开发服务器。如果想要通过本地编辑来调试依赖项，可以：
    1. 通过浏览器 devtools 的 Network 选项卡暂时禁用缓存。
    2. 重启 Vite dev server，使用 --force 标志重新打包依赖。
    3. 重新载入页面。

### 模块热重载

- Vite 提供了一套原生 ESM 的 HMR API。有需要的框架可以直接利用该 API，无需重新加载页面或删除应用程序状态。

### TypeScript

- Vite 天然支持引入.ts 文件，它仅执行.ts 文件的转译工作(使用 esbuild 将 TypeScript 转换成为 JavaScript)，并不执行任何的类型检查。

### 客户端类型

- Vite 默认的类型定义是写给它的 Node.js API 的。要将其补充到一个 Vite 应用的客户端代码环境中，要添加一个 d.ts 声明文件。

### JSX

- .jsx 和.tsx 文件同样开箱即用。JSX 的转译同样是通过 esbuild。

### 构建生产版本

- 当需要将应用部署到生产环境中，只需运行 vite build 命令。默认情况下，它使用 `<root>/index.html`作为其构建入口点，并生成能够静态部署的应用程序包。

### 浏览器的兼容性

- 默认情况下，vite 是为支持原生 ESM script 标签和支持原生 ESM 动态导入的浏览器服务的。可以通过 build.target 配置项指定构建项目，最低支持 es2015。
- 默认情况下，vite 只处理语法转译，且默认不包含任何 polyfill。

### 公共基础路径

- 如果需要在嵌套的公共路径下部署项目，只需指定 base 配置项，然后所有资源的路径都将据此配置重写。
- 当访问过程中需要使用动态连接的 url 时，可以使用全局注入的 import.meta.env.BASE_URL 变量，它的值为公共基础路径。

### 自定义构建

- 构建过程可以通过多种构建配置项来自定义构建。可以通过 build.rollupOptions 直接调整底层的 Rollup 选项：
  ```
    // vite.config.js
    module.exports = {
      build: {
        rollupOptions: {
          // https://rollupjs.org/guide/en/#big-list-of-options
        }
      }
    }
  ```

### 文件变化时重新编译

- 可以使用 vite build --watch 来启用 rollup 的监听器。或者可以通过 build.watch 调整底层的 WatcherOptions 选项：
  ```
    // vite.config.js
    module.exports = {
      build: {
        watch: {
          // https://rollupjs.org/guide/en/#watch-options
        }
      }
    }
  ```

### 环境变量和模式

#### 环境变量

- Vite 在一个特殊的 import.meta.env 对象上暴露环境变量。这里列出了一些所有情况下都可以使用的内建变量：
  - import.meta.env.MODE: {string} 应用运行的模式。
  - import.meta.env.BASE_URL: {string} 部署应用时的基本 URL。他由 base 配置项决定。
  - import.meta.env.PROD: {boolean} 应用是否运行在生产环境。
  - import.meta.env.DEV: {boolean} 应用是否运行在开发环境 (永远与 import.meta.env.PROD 相反)。

#### 生产环境替换

- 在生产环境中，这些环境变量会在构建时被静态替换，因此，在引用它们的时候要使用完全静态的字符串，动态的 key 将无法生效。

### 还有更多功能看[详情](https://cn.vitejs.dev/guide/features.html)
