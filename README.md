# 前端 CRA 项目代码规范

> 结合 Eslint 检查以及 Prettier 插件对 Create-React-App 前端项目代码规范进行检查以及格式化

## create-react-app

使用 CRA 默认模版创建的项目默认安装 eslint， 默认配置如下

```json
"eslintConfig": {
  "extends": [
    "react-app",
    "react-app/jest"
  ]
}
```

其使用的 eslint 体系是 eslint-config-react-app，等同于同时安装以下 eslint 规则

- @typescript-eslint/eslint-plugin
- @typescript-eslint/parser
- babel-eslint
- eslint
- eslint-plugin-flowtype
- eslint-plugin-import
- eslint-plugin-jsx-a11y
- eslint-plugin-react
- eslint-plugin-react-hooks

- eslint-plugin-jest
- eslint-plugin-testing-library

## eslint-config-prettier

eslint 只能进行代码规范检查，无法进行格式化，需要搭配 Prettier，而 eslint-config-prettier 的作用就是关闭 eslint 中一些没必要的但是会和 Prettier 冲突的规则

安装 eslint-config-prettier:

```node
npm install --save-dev eslint-config-prettier
```

然后添加 `"prettier"` 至 `.eslintrc.*` 文件中的 "extends" 数组的最后

```json
{
  "extends": [
    "react-app",
    "react-app/jest",
    "prettier"
  ]
}
```

`"prettier"` 作用等同于安装以下拓展规则:

- [@typescript-eslint/eslint-plugin]
- [@babel/eslint-plugin]
- [eslint-plugin-babel]
- [eslint-plugin-flowtype]
- [eslint-plugin-react]
- [eslint-plugin-standard]
- [eslint-plugin-unicorn]
- [eslint-plugin-vue]

> 注意:  eslint-config-prettier 版本 8.0.0 以下还需要补充 `"prettier/react"`

## 一些特殊规则处理

一些 eslint-config-prettier 关闭的规则在一些特定场景下可能会失效，相关案例以及解决方案详见 [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)

## eslint-plugin-prettier

官方推荐的最简单解决方法

```node
npm install --save-dev eslint-plugin-prettier
npm install --save-dev --save-exact prettier
```

修改 `.eslintrc.*` 为

```json
{
  "extends": [
    "react-app",
    "react-app/jest",
    "plugin:prettier/recommended"
  ]
}
```

等同于

```json
{
  "extends": [
    "react-app",
    "react-app/jest",
    "prettier"
  ],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error",
    "arrow-body-style": "off",
    "prefer-arrow-callback": "off",
    ...
  }
}
```

## Prettier 配置

为了将 Prettier 和 Eslint 的配置按文件分开方便解耦，还需配置 `.eslintrc.*` 中 rules 如下

```json
"rules": {
  "prettier/prettier": ["error", {}, {
    "usePrettierrc": true 
  }]
}
```

这样 eslint 能够读取 `.eslintrc.*` + `.prettierrc.*` 内容，prettier 只读取 `.prettierrc.*` 内容

常用的 `.prettierrc.js` 配置如下

```js
module.exports = {
  trailingComma: 'es5',
  arrowParens: 'always',
  useTabs: false,
  singleQuote: true,
  jsxSingleQuote: false,
  semi: true,
  tabWidth: 2,
  endOfLine: 'auto',
};
```

## 补充

### jsx 检查

[eslint-plugin-jsx-a11y](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y)

```node
npm install eslint-plugin-jsx-a11y --save-dev
```

使用推荐规则

```json
{
  "extends": [
    "plugin:jsx-a11y/recommended"
  ]
}
```

## eslint-config-airbnb

eslint-config-airbnb 是区别于 eslint-config-react-app 的另一套的 airbnb 的 JavaScript 语法规范，包括 ECMAScript 6+ 和 React。安装 eslint-config-airbnb 会同时安装

- eslint
- eslint-plugin-import
- eslint-plugin-react
- eslint-plugin-react-hooks
- eslint-plugin-jsx-a11y

其中 eslint-config-airbnb/hooks 默认不会启用，需要特殊指定

```json
"extends": ["airbnb", "airbnb/hooks"]
```

## 最后

结合 eslint-config-react-app 和 eslint-config-airbnb 语法规范以及一些自定义，同时搭配 Prettier 进行代码格式化的最终配置文件如下

`.eslintrc`

```json
{
  "extends": [
    "react-app",
    "airbnb",
    "plugin:jsx-a11y/recommended",
    "plugin:prettier/recommended",
    "react-app/jest"
  ],
  "plugins": ["jsx-a11y", "prettier"],
  "rules": {
    "prettier/prettier": ["error", {}, { "usePrettierrc": true }],
    "import/no-unresolved": "off",
    "import/prefer-default-export": "off",
    "no-param-reassign": "off",
    "no-unused-vars": "off",
    "no-plusplus": "off",
    "no-debugger": "off",
    "no-underscore-dangle": "off",
    "react/destructuring-assignment": "off",
    "react/no-unescaped-entities": "off",
    "react/jsx-boolean-value": "off",
    "react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"] }],
    "react/jsx-props-no-spreading": "off",
    "react/jsx-one-expression-per-line": "off",
    "react/jsx-wrap-multilines": "off",
    "react/prop-types": "off",
    "react/forbid-prop-types": "off",
    "react/no-array-index-key": "off",
    "react-hooks/exhaustive-deps": "off",
    "jsx-a11y/no-static-element-interactions": "off",
    "jsx-a11y/click-events-have-key-events": "off"
  }
}
```

`.prettierrc.js`

```js
module.exports = {
  trailingComma: 'es5',
  arrowParens: 'always',
  useTabs: false,
  singleQuote: true,
  jsxSingleQuote: false,
  semi: true,
  tabWidth: 2,
  endOfLine: 'auto',
};
```

相关插件安装

```node
npm install eslint-config-airbnb eslint-config-prettier eslint-plugin-jsx-a11y eslint-plugin-prettier prettier
```
