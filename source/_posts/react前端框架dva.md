---

title: react前端框架dva
date: 2017/8/29 0:17:59 
tags: react 前端 框架 dva js

---

最近正在看dva框架，发现这是一个很好应用于React的框架。 
众所周知，react的学习成本是较高的，而antd推出的这款框架，大大的降低了react的学习成本。dva简化了 react-redux 的部署的文件复杂度。使其清晰易懂。相信熟悉redux 的人，并不需要了解太多知识就能很快的上手。

<!-- more -->

# Dva的安装

> 首先需要安装的是 dva-cli 。dva-cli 是 dva 的命令行工具，包含 init、new、generate 等功能，目前最重要的功能是可以快速生成项目以及你所需要的代码片段。 

`npm install -g dva-cli `

> 安装完成后，可以通过 dva -v 查看版本，以及 dva -h 查看帮助信息。 

# 创建新应用 

安装完 dva-cli 后，我们用他来创建一个新应用，取名 myDome 。 
`dva new myDome –demo `

> 注意： –demo 用于创建简单的 demo 级项目，正常项目初始化不加要这个参数。然后进入项目目录，并启动。 

`cd myDome `
`npm start `
> 几秒之后，会看到这样的输出： 

`proxy: listened on 8989 livereload: listening on 35729 :package: 173/173 build modules webpack: bundle build is now finished.` 

- (如需关闭 server，请按 Ctrl-C.)

> 在浏览器里打开 http://localhost:8989/ ，正常情况下，你会看到一个 “Yay! Welcome to dva!” 页面。