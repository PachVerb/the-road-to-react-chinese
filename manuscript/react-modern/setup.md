## 建立一个 React 项目

在本书中，我们将使用 [create-react-app](https://github.com/facebook/create-react-app) 来引导创建你的应用程序。这是 Facebook 在 2016 年针对 React 推出的零配置入门工具包，该工具集成的内容是 Facebook 官方的主观想法，[96% 的 React 用户都会将它推荐给初学者](https://twitter.com/dan_abramov/status/806985854099062785)。使用 *create-react-app* 时，各种工具和配置都发生在后台，而开发者只需将重点放在应用程序的实现上。

安装了 Node 和 npm 之后，使用命令行在项目的专用文件夹中键入以下命令。我们将使用 *hacker-stories* 作为项目名，但你可以选择任何你喜欢的名称：

{title="Command Line",lang="text"}

~~~~~~~
npx create-react-app hacker-stories
~~~~~~~

初始化完成后，进入生成的项目文件夹：

{title="Command Line",lang="text"}

~~~~~~~
cd hacker-stories
~~~~~~~

现在我们可以在编辑器或IDE中打开我们的应用程序。对于 Visual Studio Code，你只需要在命令行输入 `code .` 即可。最后会展示以下的目录结构，或者会有一些变化，取决于不同的 *create-react-app* 版本：

{title="Project Structure",lang="text"}

~~~~~~~
hacker-stories/
--node_modules/
--public/
--src/
----App.css
----App.js
----App.test.js
----index.css
----index.js
----logo.svg
--.gitignore
--package-lock.json
--package.json
--README.md
~~~~~~~

这是一些最重要的文件夹和文件的细分：

* **README.md：** *.md* 拓展名表示该文件是一个 Markdown 文件。Markdown 是一种轻量级的标记语言，具有纯文本格式语法。许多源代码项目都带有 *README*.md 文件，该文件提供了有关该项目的说明和有用信息。当我们将项目推送到 GitHub 之类的平台时，该 *README.md* 文件通常显示有关其存储库中包含的内容的信息。因为您使用 create-react-app 来创建项目，所以 *README.md* 应该和官方的 [create-react-app GitHub 代码库](https://github.com/facebook/create-react-app)相同。

* **node_modules/：** 此文件夹包含通过 npm 安装的所有 node 包。因为我们使用了 create-react-app，因此已经有两个 node 模块被安装了。我们不会去触碰这个文件夹，因为通常会在命令行使用 npm 来安装或卸载 node 包。

* **package.json：** 此文件向您展示了依赖的 node 包列表和项目的其他配置信息。

* **package-lock.json：** 此文件描述了分解之后所有 node 包的版本。我们一般不会触碰这个文件。

* **.gitignore：** 此文件描述了使用 Git 时不应该被添加到你的 Git 代码库的所有文件和文件夹，因为这些文件和文件夹仅位于本地的项目中。*node_modules/* 目录就是一个例子。与其他人共享 *package.json* 文件就已经足够了，其他人可以在没有依赖文件夹的时候通过 `npm install` 来重新安装这些依赖。

* **public/:** 这个文件夹包含了一些开发时的文件，比如 *public/index.html*。在开发时这个索引文件将展示在 *localhost:3000* 或者其他托管的域名下。默认的设置会自动处理 *index.html* 与 *src/* 中其他 JavaScript 文件的关联关系。

首先，你需要的所有文件都位于 *src/* 文件夹下。我们主要关注用于实现 React 组件的 *src/App.js* 文件。它将会用来实现您的应用，但稍后您可能需要将组件拆分到多个文件中，每个文件中会维护一个或多个组件。

另外，您将找到一个用于测试的 *src/App.test.js* 文件，以及一个 *src/index.js* 文件作为 React 世界的入口点。在后续的部分中，您将会深入了解这两个文件。还有一个 *src/index.css* 和一个 *src/App.css* 分别设置了应用的常规样式和组件的样式，这是打开它们时的默认样式。您还会在以后修改它们。

在了解了 React 项目的文件夹和文件结构之后，让我们来看一下一些可用的命令来帮助我们开始。项目的所有特定的命令都能在 *package.json* 中的 *scripts* 属性下找到。它们看起来大概是这样：

{title="package.json",lang="javascript"}

~~~~~~~
{
  ...
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  ...
}
~~~~~~~

这些命令脚本可以在 IDE 集成的终端，或者命令行中通过 `npm run <script>` 命令来执行。其中 `run` 可以在执行 `start` 和 `test` 命令脚本时省略。如下:

{title="Command Line",lang="text"}

~~~~~~~
# 在 http://localhost:3000 运行应用
npm start

# 运行测试
npm test

# 构建用于生产的应用程序
npm run build
~~~~~~~

上面介绍的 npm scripts 中有个叫 `eject` 的命令，它不应该在练习过程中使用。因为这是一个单向操作。一旦执行了 eject 操作，您将无法回滚。本质上仅当你对当前选择的配置不满意或者想更改某些内容时，才使用此命令来访问 create-react-app 中的所有所有构建工具和配置项。在这里，我们将保持默认的配置。

### 练习题

* 通过 React 的 [create-react-app 文档](https://github.com/facebook/create-react-app) 以及 [入门指南](https://create-react-app.dev/docs/getting-started) 了解更多内容。
* 了解更多有关 [create-react-app 中支持的 JavaScript 特性](https://create-react-app.dev/docs/supported-browsers-features)。
* 了解更多有关 [create-react-app 的目录结构](https://create-react-app.dev/docs/folder-structure).
  * 逐个了解你的 React 项目中的文件夹和文件。
* 了解更多有关 [create-react-app 中可用的脚本](https://create-react-app.dev/docs/available-scripts).
  * 在命令行中使用 `npm start` 启动你的 React 应用并在浏览器中检查它。
    * 在命令行中通过按 `Control + C` 退出。
  * 运行 `npm test` 脚本。
  * 运行 `npm run build` 脚本并验证你的项目中是否会添加一个 *build/* 文件夹（之后你可以将其删除）。注意，以后可以通过 build 文件夹来[部署你的应用](https://www.robinwieruch.de/deploy-applications-digital-ocean/)。
* 每当我们在学习过程中更改代码时，请确保在浏览器中检查输出以获得视觉上的反馈。
