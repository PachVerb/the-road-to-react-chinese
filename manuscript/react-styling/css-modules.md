## React 中的 CSS 模块化

CSS Modules 是一种高级的 **CSS-in-CSS** 方法。CSS 文件保持不变，你可以应用 Sass 等 CSS 扩展，但它在 React 组件中的使用会发生变化。要在 `create-react-app` 中启用 CSS 模块，请将 *src/App.css* 文件重命名为 *src/App.module.css*。这个操作是在你的项目目录的命令行中进行的。

{title="Command Line",lang="text"}
~~~~~~~
mv src/App.css src/App.module.css
~~~~~~~

在重命名后的 *src/App.module.css* 中，像之前一样从第一个 CSS 类开始定义：

{title="src/App.module.css",lang="css"}
~~~~~~~
.container {
  height: 100vw;
  padding: 20px;

  background: #83a4d4; /* fallback for old browsers */
  background: linear-gradient(to left, #b6fbff, #83a4d4);

  color: #171212;
}

.headlinePrimary {
  font-size: 48px;
  font-weight: 300;
  letter-spacing: 2px;
}
~~~~~~~

再用相对路径导入 *src/App.module.css* 文件。这一次，把它作为一个 JavaScript 对象导入，对象的名称（这里是`styles`）由你决定：

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';
import axios from 'axios';

# leanpub-start-insert
import styles from './App.module.css';
# leanpub-end-insert
~~~~~~~

不需要将 `className` 定义为一个映射到 CSS 文件的字符串，而是直接从 `styles` 对象中访问 CSS 类，并通过 `JavaScript in JSX` 表达式将其分配给你的元素：

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
# leanpub-start-insert
    <div className={styles.container}>
      <h1 className={styles.headlinePrimary}>My Hacker Stories</h1>
# leanpub-end-insert

      <SearchForm
        searchTerm={searchTerm}
        onSearchInput={handleSearchInput}
        onSearchSubmit={handleSearchSubmit}
      />

      {stories.isError && <p>Something went wrong ...</p>}

      {stories.isLoading ? (
        <p>Loading ...</p>
      ) : (
        <List list={stories.data} onRemoveItem={handleRemoveStory} />
      )}
    </div>
  );
};
~~~~~~~

有各种方法可以通过 `styles` 对象向元素的单个 `className` 属性添加多个 CSS 类。在这里，我们使用的是 `JavaScript` 模板字符串：

{title="src/App.js",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
# leanpub-start-insert
  <div className={styles.item}>
    <span style={{ width: '40%' }}>
# leanpub-end-insert
      <a href={item.url}>{item.title}</a>
    </span>
# leanpub-start-insert
    <span style={{ width: '30%' }}>{item.author}</span>
    <span style={{ width: '10%' }}>{item.num_comments}</span>
    <span style={{ width: '10%' }}>{item.points}</span>
    <span style={{ width: '10%' }}>
# leanpub-end-insert
      <button
        type="button"
        onClick={() => onRemoveItem(item)}
# leanpub-start-insert
        className={`${styles.button} ${styles.buttonSmall}`}
# leanpub-end-insert
      >
        Dismiss
      </button>
    </span>
  </div>
);
~~~~~~~

我们还可以在 JSX 中再次添加内联样式作为更多的动态样式。也可以添加像 Sass 这样的 CSS 扩展来实现 CSS 嵌套等高级功能。不过我们会坚持使用原生 CSS 的功能：

{title="src/App.module.css",lang="css"}
~~~~~~~
.item {
  display: flex;
  align-items: center;
  padding-bottom: 5px;
}

.item > span {
  padding: 0 5px;
  white-space: nowrap;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.item > span > a {
  color: inherit;
}
~~~~~~~

下面是 *src/App.module.css* 文件中按钮 CSS 类：

{title="src/App.module.css",lang="css"}
~~~~~~~
.button {
  background: transparent;
  border: 1px solid #171212;
  padding: 5px;
  cursor: pointer;

  transition: all 0.1s ease-in;
}

.button:hover {
  background: #171212;
  color: #ffffff;
}

.buttonSmall {
  padding: 5px;
}

.buttonLarge {
  padding: 10px;
}
~~~~~~~

这里有一个向伪 BEM 命名惯例的转变，与上一节的 `button_small` 和 `button_large` 形成对比。如果前面的命名约定成立，我们只能用 `styles['button_small']` 来访问样式，由于 JavaScript 对对象下划线的限制，这使得它更加啰嗦。同样的缺点也适用于用破折号（`-`）定义的类。相比之下，现在我们可以使用 `styles.buttonSmall` 来代替（参见：Item组件）:

{title="src/App.js",lang="javascript"}
~~~~~~~
const SearchForm = ({ ... }) => (
# leanpub-start-insert
  <form onSubmit={onSearchSubmit} className={styles.searchForm}>
# leanpub-end-insert
    <InputWithLabel ... >
      <strong>Search:</strong>
    </InputWithLabel>

    <button
      type="submit"
      disabled={!searchTerm}
# leanpub-start-insert
      className={`${styles.button} ${styles.buttonLarge}`}
# leanpub-end-insert
    >
      Submit
    </button>
  </form>
);
~~~~~~~

SearchForm 组件也会接收这些样式。它必须使用字符串插值，通过 JavaScript 的模板字符串在一个元素中使用两个样式。另一种方法是 [classnames](https://github.com/JedWatson/classnames) 库，它作为项目依赖通过命令行安装：

{title="src/App.js",lang="javascript"}
~~~~~~~
import cs from 'classnames';

...

// somewhere in a className attribute
className={cs(styles.button, styles.buttonLarge)}
~~~~~~~

该库也提供了条件样式。最后，继续介绍 InputWithLabel 组件：

{title="src/App.js",lang="javascript"}
~~~~~~~
const InputWithLabel = ({ ... }) => {
  ...

  return (
    <>
# leanpub-start-insert
      <label htmlFor={id} className={styles.label}>
# leanpub-end-insert
        {children}
      </label>
      &nbsp;
      <input
        ref={inputRef}
        id={id}
        type={type}
        value={value}
        onChange={onInputChange}
# leanpub-start-insert
        className={styles.input}
# leanpub-end-insert
      />
    </>
  );
};
~~~~~~~

并在 *src/App.module.css* 文件中完成剩下的样式：

{title="src/App.module.css",lang="css"}
~~~~~~~
.searchForm {
  padding: 10px 0 20px 0;
  display: flex;
  align-items: baseline;
}

.label {
  border-top: 1px solid #171212;
  border-left: 1px solid #171212;
  padding-left: 5px;
  font-size: 24px;
}

.input {
  border: none;
  border-bottom: 1px solid #171212;
  background-color: transparent;

  font-size: 24px;
}
~~~~~~~

与上一节同样的注意事项：其中一些样式，如 `input` 和 `label`，在没有 CSS 模块的全局 *src/index.css* 文件中可能更有效。

同样，CSS Modules 就像任何其他 CSS-in-CSS 方法一样，可以使用 Sass 来实现更高级的 CSS 功能，比如嵌套。CSS 模块的优势在于，每次当一个样式没有被定义时，我们都会在 JavaScript 中收到一个错误。在标准的 CSS 方法中，JavaScript 和 CSS 文件中未匹配的样式可能会被忽略。

### 练习

* 检查 [上一节的源码](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/CSS-Modules-in-React)
* 确认 [上一节之后的变更](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-modern-final...hs/CSS-Modules-in-React?expand=1)
* 了解更多 [create-react-app 中的 CSS Modules](https://create-react-app.dev/docs/adding-a-css-modules-stylesheet)。
