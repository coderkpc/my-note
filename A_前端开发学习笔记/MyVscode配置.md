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

27. Template String Converter

> 在字符串中打$自动触发模板字符串

28. Parameter Hints

> 提示函数的参数类型

29. Highlight Matching Tag

> 高亮匹配的标签

30. vue-component

> 自动提示找到的组件，自动引入，自动注册

31. React Style Helper

> 方便您在 JSX 中更快速地编写内联样式，并对 CSS、LESS、SASS 等样式文件提供强大的辅助开发功能。对 React 和 Rax 应用友好。

32. vscode-styled-components

> 在js文件中写样式有智能提示

33. CSS Initial Value

> 提示css属性的初始值

34. A-super-translate

> 翻译插件

## 三、settings.json

```json
{
  // 主题
  "workbench.productIconTheme": "material-product-icons",
  "workbench.colorTheme": "Atom One Dark",
  //保存格式化
  "editor.formatOnSave": true,
  "stylelint.configBasedir": "path/vscode-lint",
  // 滚动缩放字体
  "editor.mouseWheelZoom": true,
  // 默认终端
  "terminal.integrated.defaultProfile.windows": "Command Prompt",
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
  // vetur配置
  "vetur.format.options.tabSize": 2,
  "bracketPairColorizer.depreciation-notice": false,
  // 启用时根据文件内容进行重写
  "editor.detectIndentation": false,
  "editor.tabSize": 2,
  "vetur.format.defaultFormatter.ts": "vscode-typescript",
  // 文件图标
  "workbench.iconTheme": "vscode-great-icons",
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
  "vetur.format.defaultFormatter.js": "vscode-typescript",
  "explorer.confirmDragAndDrop": false,
  "tabnine.experimentalAutoImports": true,
  "workbench.editor.enablePreview": false,
  "files.associations": {
    "*.wxml": "html",
    "*.wxss": "css"
  },
  "remote.SSH.remotePlatform": {
    "150.158.91.191": "linux"
  },
  // 把终端的视图设置成下拉菜单
  "terminal.integrated.tabs.enabled": false,
  "[javascriptreact]": {},
  // updated 2022-03-17 13:21
  // https://github.com/antfu/vscode-file-nesting-config
  "explorer.experimental.fileNesting.enabled": true,
  "explorer.experimental.fileNesting.expand": false,
  "explorer.experimental.fileNesting.patterns": {
    "*.asax": "$(capture).*.cs, $(capture).*.vb",
    "*.ascx": "$(capture).*.cs, $(capture).*.vb",
    "*.ashx": "$(capture).*.cs, $(capture).*.vb",
    "*.aspx": "$(capture).*.cs, $(capture).*.vb",
    "*.c": "$(capture).h",
    "*.cc": "$(capture).hpp, $(capture).h, $(capture).hxx",
    "*.cpp": "$(capture).hpp, $(capture).h, $(capture).hxx",
    "*.csproj": "*.config, *proj.user, appsettings.*, bundleconfig.json",
    "*.cxx": "$(capture).hpp, $(capture).h, $(capture).hxx",
    "*.dart": "$(capture).freezed.dart, $(capture).g.dart",
    "*.ex": "$(capture).html.eex, $(capture).html.heex, $(capture).html.leex",
    "*.js": "$(capture).js.map, $(capture).min.js, $(capture).d.ts",
    "*.jsx": "$(capture).js",
    "*.master": "$(capture).*.cs, $(capture).*.vb",
    "*.module.ts": "$(capture).resolver.ts, $(capture).controller.ts, $(capture).service.ts",
    "*.pubxml": "$(capture).pubxml.user",
    "*.resx": "$(capture).*.resx, $(capture).designer.cs, $(capture).designer.vb",
    "*.ts": "$(capture).js, $(capture).*.ts",
    "*.tsx": "$(capture).ts, $(capture).*.tsx",
    "*.vbproj": "*.config, *proj.user, appsettings.*, bundleconfig.json",
    "*.vue": "$(capture).*.ts, $(capture).*.js",
    ".clang-tidy": ".clang-format",
    ".env": "*.env, .env.*, .envrc, env.d.ts",
    ".gitignore": ".gitattributes, .gitmodules, .gitmessage, .mailmap, .git-blame*",
    "BUILD.bazel": "*.bzl, *.bazel, *.bazelrc, bazel.rc, .bazelignore, .bazelproject, WORKSPACE",
    "CMakeLists.txt": "*.cmake, *.cmake.in, .cmake-format.yaml, CMakePresets.json",
    "artisan": "*.env, .babelrc*, .codecov, .cssnanorc*, .env.*, .envrc, .htmlnanorc*, .mocha*, .postcssrc*, .terserrc*, api-extractor.json, ava.config.*, babel.config.*, cssnano.config.*, cypress.json, env.d.ts, htmlnanorc.*, jasmine.*, jest.config.*, jsconfig.*, karma*, playwright.config.*, postcss.config.*, puppeteer.config.*, server.php, svgo.config.*, tailwind.config.*, tsconfig.*, tsdoc.*, unocss.config.*, vitest.config.*, webpack.config.*, webpack.mix.js, windi.config.*",
    "astro.config.*": "*.env, .babelrc*, .codecov, .cssnanorc*, .env.*, .envrc, .htmlnanorc*, .mocha*, .postcssrc*, .terserrc*, api-extractor.json, ava.config.*, babel.config.*, cssnano.config.*, cypress.json, env.d.ts, htmlnanorc.*, jasmine.*, jest.config.*, jsconfig.*, karma*, playwright.config.*, postcss.config.*, puppeteer.config.*, svgo.config.*, tailwind.config.*, tsconfig.*, tsdoc.*, unocss.config.*, vitest.config.*, webpack.config.*, windi.config.*",
    "cargo.toml": ".clippy.toml, .rustfmt.toml, cargo.lock, clippy.toml, cross.toml, rust-toolchain.toml, rustfmt.toml",
    "composer.json": ".php*.cache, composer.lock, phpunit.xml*, psalm*.xml",
    "default.nix": "shell.nix",
    "dockerfile": ".dockerignore, docker-compose.*, dockerfile*",
    "flake.nix": "flake.lock",
    "gatsby-config.*": "*.env, .babelrc*, .codecov, .cssnanorc*, .env.*, .envrc, .htmlnanorc*, .mocha*, .postcssrc*, .terserrc*, api-extractor.json, ava.config.*, babel.config.*, cssnano.config.*, cypress.json, env.d.ts, gatsby-browser.*, gatsby-node.*, gatsby-ssr.*, gatsby-transformer.*, htmlnanorc.*, jasmine.*, jest.config.*, jsconfig.*, karma*, playwright.config.*, postcss.config.*, puppeteer.config.*, svgo.config.*, tailwind.config.*, tsconfig.*, tsdoc.*, unocss.config.*, vitest.config.*, webpack.config.*, windi.config.*",
    "gemfile": ".ruby-version, gemfile.lock",
    "go.mod": ".air*, go.sum",
    "mix.exs": ".credo.exs, .dialyzer_ignore.exs, .formatter.exs, mix.lock",
    "next.config.*": "*.env, .babelrc*, .codecov, .cssnanorc*, .env.*, .envrc, .htmlnanorc*, .mocha*, .postcssrc*, .terserrc*, api-extractor.json, ava.config.*, babel.config.*, cssnano.config.*, cypress.json, env.d.ts, htmlnanorc.*, jasmine.*, jest.config.*, jsconfig.*, karma*, next-env.d.ts, playwright.config.*, postcss.config.*, puppeteer.config.*, svgo.config.*, tailwind.config.*, tsconfig.*, tsdoc.*, unocss.config.*, vitest.config.*, webpack.config.*, windi.config.*",
    "nuxt.config.*": "*.env, .babelrc*, .codecov, .cssnanorc*, .env.*, .envrc, .htmlnanorc*, .mocha*, .postcssrc*, .terserrc*, api-extractor.json, ava.config.*, babel.config.*, cssnano.config.*, cypress.json, env.d.ts, htmlnanorc.*, jasmine.*, jest.config.*, jsconfig.*, karma*, playwright.config.*, postcss.config.*, puppeteer.config.*, svgo.config.*, tailwind.config.*, tsconfig.*, tsdoc.*, unocss.config.*, vitest.config.*, webpack.config.*, windi.config.*",
    "package.json": ".browserslist*, .circleci*, .codecov, .commitlint*, .editorconfig, .eslint*, .firebase*, .flowconfig, .github*, .gitlab*, .gitpod*, .huskyrc*, .jslint*, .lintstagedrc*, .markdownlint*, .mocha*, .node-version, .nodemon*, .npm*, .nvmrc, .pm2*, .pnp.*, .pnpm*, .prettier*, .releaserc*, .sentry*, .stackblitz*, .styleci*, .stylelint*, .tazerc*, .textlint*, .tool-versions, .travis*, .vscode*, .watchman*, .xo-config*, .yamllint*, .yarnrc*, api-extractor.json, apollo.config.*, appveyor*, ava.config.*, azure-pipelines*, bower.json, build.config.*, commitlint*, crowdin*, cypress.json, dangerfile*, dprint.json, firebase.json, grunt*, gulp*, jasmine.*, jenkins*, jest.config.*, jsconfig.*, karma*, lerna*, lint-staged*, nest-cli.*, netlify*, nodemon*, nx.*, package-lock.json, playwright.config.*, pm2.*, pnpm*, prettier*, pullapprove*, puppeteer.config.*, pyrightconfig.json, renovate*, rollup.config.*, stylelint*, tsconfig.*, tsdoc.*, tslint*, tsup.config.*, turbo*, typedoc*, vercel*, vetur.config.*, vitest.config.*, webpack.config.*, workspace.json, xo.config.*, yarn*",
    "pubspec.yaml": ".metadata, .packages, all_lint_rules.yaml, analysis_options.yaml, build.yaml, pubspec.lock",
    "pyproject.toml": ".pdm.toml, pdm.lock, pyproject.toml",
    "quasar.conf.js": "*.env, .babelrc*, .codecov, .cssnanorc*, .env.*, .envrc, .htmlnanorc*, .mocha*, .postcssrc*, .terserrc*, api-extractor.json, ava.config.*, babel.config.*, cssnano.config.*, cypress.json, env.d.ts, htmlnanorc.*, jasmine.*, jest.config.*, jsconfig.*, karma*, playwright.config.*, postcss.config.*, puppeteer.config.*, quasar.extensions.json, svgo.config.*, tailwind.config.*, tsconfig.*, tsdoc.*, unocss.config.*, vitest.config.*, webpack.config.*, windi.config.*",
    "readme": "authors, backers.md, changelog*, citation*, code_of_conduct.md, codeowners, contributing.md, contributors, copying, credits, governance.md, history.md, license*, maintainers, readme*, security.md, sponsors.md",
    "readme.md": "authors, backers.md, changelog*, citation*, code_of_conduct.md, codeowners, contributing.md, contributors, copying, credits, governance.md, history.md, license*, maintainers, readme*, security.md, sponsors.md",
    "readme.rst": "authors, backers.md, changelog*, citation*, code_of_conduct.md, codeowners, contributing.md, contributors, copying, credits, governance.md, history.md, license*, maintainers, readme*, security.md, sponsors.md",
    "readme.txt": "authors, backers.md, changelog*, citation*, code_of_conduct.md, codeowners, contributing.md, contributors, copying, credits, governance.md, history.md, license*, maintainers, readme*, security.md, sponsors.md",
    "remix.config.*": "*.env, .babelrc*, .codecov, .cssnanorc*, .env.*, .envrc, .htmlnanorc*, .mocha*, .postcssrc*, .terserrc*, api-extractor.json, ava.config.*, babel.config.*, cssnano.config.*, cypress.json, env.d.ts, htmlnanorc.*, jasmine.*, jest.config.*, jsconfig.*, karma*, playwright.config.*, postcss.config.*, puppeteer.config.*, remix.*, svgo.config.*, tailwind.config.*, tsconfig.*, tsdoc.*, unocss.config.*, vitest.config.*, webpack.config.*, windi.config.*",
    "rush.json": ".browserslist*, .circleci*, .codecov, .commitlint*, .editorconfig, .eslint*, .firebase*, .flowconfig, .github*, .gitlab*, .gitpod*, .huskyrc*, .jslint*, .lintstagedrc*, .markdownlint*, .mocha*, .node-version, .nodemon*, .npm*, .nvmrc, .pm2*, .pnp.*, .pnpm*, .prettier*, .releaserc*, .sentry*, .stackblitz*, .styleci*, .stylelint*, .tazerc*, .textlint*, .tool-versions, .travis*, .vscode*, .watchman*, .xo-config*, .yamllint*, .yarnrc*, api-extractor.json, apollo.config.*, appveyor*, ava.config.*, azure-pipelines*, bower.json, build.config.*, commitlint*, crowdin*, cypress.json, dangerfile*, dprint.json, firebase.json, grunt*, gulp*, jasmine.*, jenkins*, jest.config.*, jsconfig.*, karma*, lerna*, lint-staged*, nest-cli.*, netlify*, nodemon*, nx.*, package-lock.json, playwright.config.*, pm2.*, pnpm*, prettier*, pullapprove*, puppeteer.config.*, pyrightconfig.json, renovate*, rollup.config.*, stylelint*, tsconfig.*, tsdoc.*, tslint*, tsup.config.*, turbo*, typedoc*, vercel*, vetur.config.*, vitest.config.*, webpack.config.*, workspace.json, xo.config.*, yarn*",
    "shims.d.ts": "*.d.ts",
    "svelte.config.*": "*.env, .babelrc*, .codecov, .cssnanorc*, .env.*, .envrc, .htmlnanorc*, .mocha*, .postcssrc*, .terserrc*, api-extractor.json, ava.config.*, babel.config.*, cssnano.config.*, cypress.json, env.d.ts, htmlnanorc.*, jasmine.*, jest.config.*, jsconfig.*, karma*, mdsvex.config.js, playwright.config.*, postcss.config.*, puppeteer.config.*, svgo.config.*, tailwind.config.*, tsconfig.*, tsdoc.*, unocss.config.*, vitest.config.*, webpack.config.*, windi.config.*",
    "vite.config.*": "*.env, .babelrc*, .codecov, .cssnanorc*, .env.*, .envrc, .htmlnanorc*, .mocha*, .postcssrc*, .terserrc*, api-extractor.json, ava.config.*, babel.config.*, cssnano.config.*, cypress.json, env.d.ts, htmlnanorc.*, jasmine.*, jest.config.*, jsconfig.*, karma*, playwright.config.*, postcss.config.*, puppeteer.config.*, svgo.config.*, tailwind.config.*, tsconfig.*, tsdoc.*, unocss.config.*, vitest.config.*, webpack.config.*, windi.config.*",
    "vue.config.*": "*.env, .babelrc*, .codecov, .cssnanorc*, .env.*, .envrc, .htmlnanorc*, .mocha*, .postcssrc*, .terserrc*, api-extractor.json, ava.config.*, babel.config.*, cssnano.config.*, cypress.json, env.d.ts, htmlnanorc.*, jasmine.*, jest.config.*, jsconfig.*, karma*, playwright.config.*, postcss.config.*, puppeteer.config.*, svgo.config.*, tailwind.config.*, tsconfig.*, tsdoc.*, unocss.config.*, vitest.config.*, webpack.config.*, windi.config.*"
  },
}
```

