# Apache Linkis Website
[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

[English](README.md) | [中文](README_ZH.md)

这是包含 `https://linkis.apache.org` 的所有源代码的存储库。
本指南将指导您如何为Linkis的网站做出贡献。


## 分支 
dev为开发分支，修改请先fork到自己的仓库，然后在dev分支上进行开发修改。
```
master  主分支
dev     开发分支
asf-site    官网正式环境  通过https://linkis.apache.org 访问
asf-staging 官网测试环境  通过https://linkis.staged.apache.org 访问
```


## 1 预览并生成静态文件

本网站是使用node编译的，使用的是Docusaurus框架组件

1. 下载并安装 nodejs(version>12.5.0)
2. 克隆代码到本地 `git clone  git@github.com:apache/incubator-linkis-website.git`
2. 运行 `npm install` 来安装所需的依赖库。
3. 在根目录运行`npm run start`，可以访问http://localhost:3000查看站点英文模式预览
4. 在根目录运行`npm run start-zh`，可以访问http://localhost:3000查看站点的中文模式预览
5. 要生成静态网站资源文件，运行 `npm run build`。构建的静态资源在build目录中。

## 2 目录结构
```html
|-- community //社区
|-- docs     //文档  存方下一个即将发布的版本
|-- download //下载
|-- faq      //Q&A
|-- i18n    
|   `-- zh-CN  //国际化中文
|       |-- code.json
|       |-- docusaurus-plugin-content-docs
|       |-- docusaurus-plugin-content-docs-community
|       |-- docusaurus-plugin-content-docs-download
|       |-- docusaurus-plugin-content-docs-faq
|       `-- docusaurus-theme-classic
|-- resource // 架构/时序/流程图等的原始工程文件
|-- src
|   |-- components
|   |-- css
|   |-- js
|   |-- pages
|   |   |-- home
|   |   |-- index.jsx
|   |   |-- team
|   |   |-- user
|   |   `-- versions
|   |-- styles
|-- static //图片静态资源
|   |-- Images
|   |-- Images-zh
|   |-- faq
|   |-- home
|   `-- img
|-- docusaurus.config.js
|-- versioned_docs //历史版本存档
|   `-- version-1.0.2
|-- versioned_sidebars
|   `-- version-1.0.2-sidebars.json
`-- versions.json

```
下表说明了版本化文件如何映射到其版本和生成的 URL。

| 资源路径                                    | 版本        | 访问URL               |
| --------------------------------------- | -------------- | ----------------- |
| `versioned_docs/version-1.0.1/hello.md` | 1.0.1         | /docs/1.0.1/hello |
| `versioned_docs/version-1.0.2/hello.md` | 1.0.2(latest 当前稳定版本) | /docs/latest/hello  |
| `docs/hello.md`                         | current(下一个将发布的版本 1.0.3)      | /docs/1.0.3/hello  |


当前的版本信息

| 版本                               | 访问路径        | 英文文档路径        | 中文文档路径               |
| ---------------------------------------| -------------- | -------------- | ----------------- |
| 1.0.2|https://linkis.apache.org/docs/latest/xxx(https://linkis.apache.org/zh-CN/docs/latest/xxx)| versioned_docs/version-1.0.2/  |  i18n/zh-CN/docusaurus-plugin-content-docs/version-1.0.2 |
|1.0.3|https://linkis.apache.org/docs/1.0.3/xxx(https://linkis.apache.org/zh-CN/docs/1.0.3/xxx) |  /docs  |i18n/zh-CN/docusaurus-plugin-content-docs/current|




## 3 规范

### 3.1 目录命名规范

全部采用小写方式， 以中下划线分隔，有复数结构时，要采用复数命名法， 缩写不用复数

正例： `scripts / styles / components / images / utils / layouts / demo_styles / demo-scripts / img / doc`

反例： `script / style / demoStyles / imgs / docs`

### 3.2 vue以及静态资源文件命名规范

全部采用小写方式， 以中划线分隔

正例： `render-dom.js / signup.css / index.html / company-logo.png`

反例： `renderDom.js / UserManagement.html`

### 3.3 资源路径 

图片资源统一放在`static/{模块名}`下

css等样式文件放在`src/css`目录下

### 3.4 页面内容修改 
> 除了首页、团队、用户、Docs>All Version 模块页面外，其余页面都能通过底部的'Edit this page'按钮 直接跳转至对应的github的资源修改页

### 3.5 首页页面修改
访问页面  https://linkis.apache.org/
位于 `src/pages/home`

```
├─home
│      config.json 首页中英文的配置  
│      index.less  首页样式
```
### 3.6 团队页面修改
访问页面  https://linkis.apache.org/zh-CN/team
位于 `src/pages/team`
```
├─team
│      config.json
│      index.js
│      index.less
```
### 3.7  用户 列表页面修改
访问页面  https://linkis.apache.org/zh-CN/user/
```
位于 `src/pages/user`
└─versions
        config.json
        data.json
        img.json
        index.js
        index.less
```

### 3.8 version 列表页面修改
访问页面  https://linkis.apache.org/zh-CN/versions/
```
位于 `src/pages/versions`
└─versions
        config.json
        index.jsorchestrator/overview.md
        index.less
```
## 4 新增文档 

![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) md文档建议通过访问官网查看,通过github查看md文档存在图片等静态资源无法正确显示问题

- 英文文档 docs/对应目录为即将发布的下一Next版本，历史存档版本存放在versioned_docs/version-${versionno}目录下。
- 中文文档 放在i18n/zh-CN/docusaurus-plugin-content-docs/对应目录下，current为即将发布的下一个版本.version-${versionno}为历史存档版本。
- 图片资源 放在static/目录下

## 5 如何部署
linkis的官网分为测试环境和正式环境
- 测试环境访问网址 https://linkis.staged.apache.org
- 正式环境访问网址 https://linkis.apache.org

当dev分支代码有更新时，git action会自动执行dev分支的源码build,并将编译的资源结果自动更新至asf-staging分支。
Apache内部机制会将asf-staging分支内容部署至测试环境，所以当git action执行成功后，可以通过访问https://linkis.staged.apache.org 进行验证。
验证无误后，可以将asf-staging分支 merge到asf-site分支，Apache内部机制会将asf-site分支内容部署至正式环境，merge后，正式环境才算更新成功。
    
## 6 其他 
命名规范参考《阿里前端开发规范》