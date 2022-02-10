# vscode

## 一.修改快捷键

File >> Preferences >> Keyboard Shprtcuts >>右上角+ 代码模式>>复制以下代码

```json
[
  {
    "key": "ctrl+shift+oem_2",
    "command": "editor.action.blockComment",
    "when": "editorTextFocus && !editorReadonly"
  },
  {
    "key": "shift+alt+a",
    "command": "-editor.action.blockComment",
    "when": "editorTextFocus && !editorReadonly"
  },
  {
    "key": "ctrl+shift+u",
    "command": "-workbench.action.output.toggleOutput",
    "when": "workbench.panel.output.active"
  },
  {
    "key": "ctrl+shift+u",
    "command": "editor.action.transformToUppercase"
  },
  {
    "key": "ctrl+shift+l",
    "command": "-selectAllSearchEditorMatches",
    "when": "inSearchEditor"
  },
  {
    "key": "ctrl+shift+l",
    "command": "-editor.action.selectHighlights",
    "when": "editorFocus"
  },
  {
    "key": "ctrl+shift+l",
    "command": "-addCursorsAtSearchResults",
    "when": "fileMatchOrMatchFocus && searchViewletVisible"
  },
  {
    "key": "ctrl+shift+l",
    "command": "editor.action.transformToLowercase"
  },
  {
    "key": "ctrl+shift+oem_1",
    "command": "-breadcrumbs.focus",
    "when": "breadcrumbsPossible"
  },
  {
    "key": "ctrl+shift+oem_1",
    "command": "editor.action.transformToSnakecase"
  }
]
```

## 二、常用插件

1. Class autocomplete for HTML

> 自动重命名配对的HTML/XML标签(必备)

2. Debugger for Chrome

> 映射vscode上的断点到chrome上，方便调试

3. HTML CSS Support

> 智能提示CSS类名以及id

4. HTML Snippets

> 智能提示HTML标签，以及标签含义

5. JavaScript(ES6) code snippets

> ES6语法智能提示，以及快速输入，不仅仅支持.js，还支持.ts，.jsx，.tsx，.html，.vue，省去了配置其支持各种包含js代码文件的时间

6. open in browser

> vscode不像IDE一样能够直接在浏览器中打开html，而该插件支持快捷键与鼠标右键快速在浏览器中打开html文件，支持自定义打开指定的浏览器，包括：Firefox，Chrome，Opera，IE以及Safari

7. fileheader

> 顶部注释模板，可定义作者、时间等信息，并会自动更新最后修改时间，快捷键ctrl+alt+i在文件开头自动输入作者信息和修改信息等内容

8. Path Intellisense

> 路径自动补全

9. npm Intellisense

> require的包提示

10. Code Runner

> 右键直接跑js代码

11. Auto Rename Tag

> 自动重命名标签

12. Auto Complete Tag

> 自动补全标签

13. Auto Close Tag

> 自动闭合标签

14. Auto Import - ES6, TS, JSX, TSX 

> 自动引入ES6模块

15. Add jsdoc comments

> 给函数添加接口文档风格的注释

16. Bracket Pair Colorizer

> 不同颜色高亮的括号

17. Element UI Snippets

> element UI 的代码块

18. ES7 React/Redux/GraphQL/React-Native snippets

> React全家桶的代码块

19. ESLint

> 语法检查

20. ESLint Chinese Rules

>ESLint的中文规则提示

21. formate: CSS/LESS/SCSS formatter

> 格式化css代码的

22. Image Preview

> 根据路径预览图片

23. Tabnine AI Autocomplete for Javascript, Python, Typescript, PHP, Go, Java, Ruby & more

> 一个非常猛的AI代码智能提示

24. Vue 3 Snippets

> Vue的代码块

25. Vetur


> Vue开发必备插件

26. Webpack Snippets

> Webpack常用代码块

26. React/Redux/react-router Snippets

> React常用代码块

26. React Native Tools

> React Native开发工具

26. Bootstrap 3 Snippets

> BS3的代码块

26. vscode-faker

> 一个提供假数据的插件

## 三、settings.json

```json
{
  // 主题
  "workbench.productIconTheme": "material-product-icons",
  "workbench.colorTheme": "GitHub Dark",
  //保存格式化
  "editor.formatOnSave": true,
  // 配置ESLint和StyleLint开始
  // 在方法括号之间插入空格
  "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
  "css.validate": false,
  "less.validate": false,
  "scss.validate": false,
  "stylelint.configBasedir": "path/vscode-lint",
  // 滚动缩放字体
  "editor.mouseWheelZoom": true,
  // 默认终端
  "terminal.integrated.defaultProfile.windows": "Git Bash",
  // 自动保存
  "files.autoSave": "onFocusChange",
  // codeRunner
  "code-runner.saveAllFilesBeforeRun": true,
  // 信任工作区
  "security.workspace.trust.enabled": false,
  "editor.linkedEditing": true,
  // 不提示liveserver
  "liveServer.settings.donotShowInfoMsg": true,
  "explorer.confirmDelete": false,
  "javascript.updateImportsOnFileMove.enabled": "always",
  "git.confirmSync": false,
  "workbench.startupEditor": "none",
  "editor.suggestSelection": "first",
  "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
  // vetur配置
  "vetur.format.options.tabSize": 2,
  "vetur.format.defaultFormatterOptions": {
    "prettier": {
      "semi": false, //不加分号
      "singleQuote": true //用单引号
    }
  },
  "bracketPairColorizer.depreciation-notice": false,
  // 启用时根据文件内容进行重写
  "editor.detectIndentation": false,
  "editor.tabSize": 2,
  "vetur.format.defaultFormatter.ts": "vscode-typescript",
  // 文件图标
  "workbench.iconTheme": "material-icon-theme",
  // 字符串中自动补全html标签
  "emmet.triggerExpansionOnTab": true,
  "emmet.showAbbreviationSuggestions": true,
  "emmet.showExpandedAbbreviation": "always",
  "emmet.includeLanguages": {
    "javascript": "html"
  },
  "vetur.ignoreProjectWarning": true,
  "editor.fontWeight": "normal",
  // - auto: 仅在超出行长度时才对属性进行换行。
  // - force: 对除第一个属性外的其他每个属性进行换行。
  // - force-aligned: 对除第一个属性外的其他每个属性进行换行，并保持对齐。
  // - force-expand-multiline: 对每个属性进行换行。
  // - aligned-multiple: 当超出折行长度时，将属性进行垂直对齐。
  "html.format.wrapAttributes": "auto",
  "security.workspace.trust.untrustedFiles": "open",
  "workbench.editor.untitled.hint": "hidden",
  "[vue]": {
    "editor.defaultFormatter": "octref.vetur"
  },
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  },
  // 自动删除结尾的; ,  
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "vue",
      "autoFix": true
    },
    "vue"
  ],
  "vetur.format.defaultFormatter.js": "vscode-typescript",
  "explorer.confirmDragAndDrop": false,
  "tabnine.experimentalAutoImports": true,
  "workbench.editor.enablePreview": false,
  // "eslint.autoFixOnSave": true,
  "fileheader.Author": "coderkpc",
  "fileheader.LastModifiedBy": "coderkpc"
}
```

