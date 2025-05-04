title: 利用 Rust 构建轻量级多端桌面应用：Pake 介绍
author: l1n6yun
tags: 
 - Rust
 - 桌面应用
 - 构建工具
 - 应用打包
categories: []
date: 2024-11-09 11:13:00
---
> Pake 是一个基于 Rust 的工具，它允许开发者轻松构建轻量级的多平台桌面应用。以其小巧的体积和卓越的性能，Pake 成为了许多开发者的首选工具。本文将详细介绍 Pake 的特性、安装方法、使用指南以及如何进行定制开发，并特别强调快捷键的使用。

## Pake 的特性

Pake 的核心特性包括：

- **体积小**：相比传统的 Electron 套壳打包，Pake 的体积小将近 20 倍，大约在 5M 左右。
- **性能优异**：Pake 的底层使用的是 Rust Tauri 框架，相较于 JavaScript 框架，它提供了更轻快的性能体验和更小的内存占用。
- **功能丰富**：Pake 不仅能打包应用，还实现了快捷键透传、沉浸式窗口、拖动、样式改写、去广告等功能，并支持产品的极简风格定制。
- **简单易用**：Pake 被描述为一个简单的小玩具，使用 Tauri 替代了传统的套壳网页打包思路，同时推荐使用 PWA（Progressive Web Apps）。

## 开始使用 Pake

### 安装 Pake CLI

Pake 提供了命令行工具，可以通过 npm 进行安装：

```
npm install -g pake-cli
```

### 命令行一键打包

使用 Pake 进行一键打包非常简单，以下是基本的命令使用示例：

```
pake url [OPTIONS]...
```

例如，如果你想打包 [Weekly](https://weekly.tw93.fun/) 应用，并隐藏标题栏，可以使用以下命令：

```
pake https://weekly.tw93.fun --name Weekly --hide-title-bar
```

### 快捷键说明

Pake 支持快捷键，以提高用户的工作效率。以下是 Pake 支持的快捷键及其功能：

| Mac | Windows/Linux | 功能  |
| --- | --- | --- |
| ⌘ + [ | Ctrl + ← | 返回上一个页面 |
| ⌘ + ] | Ctrl + → | 去下一个页面 |
| ⌘ + ↑ | Ctrl + ↑ | 自动滚动到页面顶部 |
| ⌘ + ↓ | Ctrl + ↓ | 自动滚动到页面底部 |
| ⌘ + r | Ctrl + r | 刷新页面 |
| ⌘ + w | Ctrl + w | 隐藏窗口，非退出 |
| ⌘ + - | Ctrl + - | 缩小页面 |
| ⌘ + + | Ctrl + + | 放大页面 |
| ⌘ + = | Ctrl + = | 放大页面 |
| ⌘ + 0 | Ctrl + 0 | 重置页面缩放 |

## 高级使用

Pake 的代码结构和高级用法可以在其官方文档中找到。以下是一些关键点：

- 修改 src-tauri 目录下的 pake.json 中的 url 和 productName 字段，并同步修改 tauri.config.json 中的 domain 字段。
- 修改 tauri.xxx.conf.json 中的 icon 和 identifier 字段，图标可以从 icons 目录选择，或者从 macOSicons 下载。
- 在 pake.json 中修改窗口属性，如 width/height、fullscreen、resizable 等。
- 适配 Mac 沉浸式头部，可以将 hideTitleBar 设置为 true，并为 Header 元素添加 padding-top 样式。

## 结语

Pake 是一个强大的工具，它让构建轻量级多端桌面应用变得简单快捷。无论是小白用户、开发用户还是折腾用户，都能在 Pake 中找到适合自己的使用方式。希望这篇文章能帮助你更好地了解和使用 Pake，享受构建桌面应用的乐趣。