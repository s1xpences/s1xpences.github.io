---
title: hexo+git
date: 2022-07-01 14:16:05
tags:
layout: hexo+git
---
# 第一步
* 新建项目文件夹
```
npm install hexo
hexo init

hexo c  | hexo clean      删除public文件夹
hexo g  | hexo generate   该命令执行后在hexo站点根目录下生成public文件夹
hexo s  | hexo server     启动本地服务，默认地址为http://localhost:4000/，4000端口
hexo d  | hexo deploy     部署站点，在本地生成.deploy_git文件夹，并将编译后的文件上传至 GitHub
hexo new [layout] <title> 上述指令执行时，Hexo 会尝试在 scaffolds 中寻找photo.md布局，若找到，则根据该布局新建文章；若未找到或指令中未指定该参数，则使用post.md新建文章。新建文章的名称在_config.yml中配置。

npm install hexo-deployer-git --save
deploy:
  type: git
  repo: git@github.com:xxx/xxx.github.io.git github仓库地址
  branch: 分支名 github分支
```
# 主题
https://hexo.fluid-dev.com/