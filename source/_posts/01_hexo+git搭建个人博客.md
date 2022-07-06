---
title: hexo+git搭建个人博客
date: 2022-07-01 14:16:05
tags:
---
**官方文档 [hexo](https://hexo.io/zh-cn/)**


### 配置
1. 新建项目文件夹，安装hexo
```
npm install hexo | npm install hexo -g
```

2. 初始化
```
hexo init
```

3. 安装git插件
```
npm install hexo-deployer-git --save
```

4. 配置_config.yml
```yml
deploy:
  type: git
  repo: git@github.com:xxx/xxx.github.io.git 
  branch: [branch-name]
```

### 部署
1. 清空public文件夹
```
hexo clean                
```

2. 生成public文件夹
```
hexo g  | hexo generate   
```

3. 启动本地服务,默认地址为http://localhost:4000/(可跳过)
```
hexo s  | hexo server 
```

4. 部署站点，在本地生成.deploy_git文件夹，并将编译后的文件上传至 GitHub
```
hexo d  | hexo deploy
```

### 写作
1. 新建md文件
```
hexo new [layout] <title> 
如：hexo new photo 'test'
```
上述指令执行时，Hexo 会尝试在 scaffolds 中寻找photo.md布局，若找到，则根据该布局新建文章；
若未找到或指令中未指定该参数，则使用post.md新建文章。新建文章的名称在_config.yml中配置。


2. layout的指定
>不写默认：以post.md为模板，_posts下生成文件；
>
>draft：以draft.md为模板，_draft文件夹下生成草稿；
>
>page：以page.md为模板，source下生成文件夹，并自带index.md，可以通过xxx.github.io/文件夹名 访问，相当于新建一个页面；
>
>自定义xxx：以xxx.md为模板，_posts下生成文件。


### 主题
[fluid](https://hexo.fluid-dev.com/docs/start/)