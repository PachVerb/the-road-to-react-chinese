## React Props

我们现在把 `list` 变量作为当前应用的一个全局变量。我们在 App 组件内直接从全局作用域使用它，在 List 组件内同样这样使用。如果你只有一个变量的话这种方式是可行的，但这无法扩展为在不同文件的多个组件内使用多个变量。

通过使用所谓的 props ，我们可以把变量作为信息从一个组件传给另一个组件。在使用 props 之前，我们要先把 list 从全局作用域移至 App 组件内部，并按它的实际领域重命名。

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
# leanpub-start-insert
  const stories = [
    {
      title: 'React',
      url: 'https://reactjs.org/',
      author: 'Jordan Walke',
      num_comments: 3,
      points: 4,
      objectID: 0,
    },
    {
      title: 'Redux',
      url: 'https://redux.js.org/',
      author: 'Dan Abramov, Andrew Clark',
      num_comments: 2,
      points: 5,
      objectID: 1,
    },
  ];
# leanpub-end-insert

  const handleChange = event => { ... };

  return ( ... );
};
~~~~~~~

接下来我们将使用 **React props** 来把这个数组传递给 List 组件：

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

  const handleChange = event => { ... };

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange={handleChange} />

      <hr />

# leanpub-start-insert
      <List list={stories} />
# leanpub-end-insert
    </div>
  );
};
~~~~~~~

这个变量在 App 组件中被称为 `stories` ，并且我们把它以这个名字传给了 List 组件。然而在 List 组件实例化时，它被赋值给了 `list` 属性。我们在 List 组件的函数签名中通过 `props` 对象中的 `list` 访问它。

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const List = props =>
  props.list.map(item => (
# leanpub-end-insert
    <div key={item.objectID}>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  ));
~~~~~~~

通过这样操作，我们已经避免了 list/stories 变量在 App 组件内污染全局作用域。因为 `stories` 不是在 App 组件被直接使用的，而是在它的其中一个子组件内，我们把它作为 props 传给了 List 组件。然后我们可以通过函数签名的第一个参数 `props` 来访问它。

### 练习

* 检查[上一节的源码](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/React-Props)。
* 确认[上一节之后的变更](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Handler-Function-in-JSX...hs/React-Props?expand=1)。
* 阅读更多关于[如何向 React 组件传递 props](https://www.robinwieruch.de/react-pass-props-to-component)。
