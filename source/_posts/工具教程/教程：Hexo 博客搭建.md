# 配置目标
1. 在 Windos 下面使用 Obsidian 书写个人笔记；
2. 笔记备份到 github 上进行同步；
3. 个人博客通过域名进行访问；
# 安装过程
## 安装 Node.js 和 Git
Git 参考: [[教程：git]]

## 安装 Hexo

新建一个文件夹,在这个文件夹右键打开git,并输入以下指令。

```shell
npm install -g hexo-cli
```
## 初始化 Hexo

选择一个适合你的文件夹作为**博客项目的根目录**，在命令行中进入该目录并执行以下命令：

```shell
hexo init hexo
cd hexo
npm install
```
此时文件夹中会出现 Hexo 的默认文件。

##  配置文件
```json
title: '标题'
subtitle: '副标题'
description: '网站描述'
author: 作者
language: zh-Hans
timezone: 'Asia/Shanghai'
```
Hexo 基本操作
[使用 Hexo 搭建个人博客并通过 GitHub部署到vercel - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/681696900)



# 主题

[Themes | Hexo](https://hexo.io/themes/)
比较好看
https://github.com/geektutu/hexo-theme-geektutu

最好看的
[Candinya/Kratos-Rebirth: 一只可爱的hexo主题 m(=•ェ•=)m~🍭 (github.com)](https://github.com/Candinya/Kratos-Rebirth)


考虑一个 obsidian
[Hexo+obsidian+github完美建站教程 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/613429644)
obsidian 加入 hexo 
[Hexo + Obsidian + Git 完美的博客部署与编辑方案 | EsunR-Blog](https://blog.esunr.site/2022/07/e9b42b453d9f.html#3-1-%E5%B0%86-Hexo-%E9%A1%B9%E7%9B%AE%E5%AF%BC%E5%85%A5-Obsidian)

源代码增加主题
```text
git clone -b master https://github.com/Candinya/Kratos-Rebirth.git themes/kratos-rebirth
```


# 启动方式


# 参考连接