title: grafana 通过自定义API获取数据
author: l1n6yun
tags: 
 - grafana
 - 自定义API
 - infinity插件
categories: []
date: 2025-04-30 14:30:00
---
## 安装infinity插件

在插件页面中搜索 `infinity`，进入 infinity 插件页面点击 `install`,等待安装完成。

![安装infinity插件](/images/pasted-79.png)

## 添加数据源

进入数据源页面，点击 `Add new data source` 按钮，搜索 `infinity`。

![搜索数据源](/images/pasted-80.png)

进入 infinity 数据源页面，输入 数据源 名称点击 `Save & test`

![添加数据源](/images/pasted-81.png)

## 使用数据源

在仪表板中添加可视化，在 `Data source` 选项中选择刚刚新增的数据源。

在 `URL` 中填写你的接口地址。根据要求返回对应的格式即可呈现到面板上。

![使用数据源](/images/pasted-82.png)