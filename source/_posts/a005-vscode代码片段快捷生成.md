---
title: vscode代码片段快捷生成
categories:
  - 前端学习
tags:
  - vscode
date: 2022-07-08 11:17:01
index_img:  https://img2.baidu.com/it/u=3701357552,1623780762&fm=253&app=120&size=w931&n=0&f=JPEG&fmt=auto?sec=1657731600&t=7ffcca6e10ea2d965dd1a28bbd938588
---

## 设置
vscode设置=>用户代码片段

### vue3配置
输入vue回车进入vue.json.code-snippets
```json
{
	"Print to console": {
		"prefix": "vue",
		"body": [
			"<template>",
			"   $2",
			"</template>",
			"\n",
			"<script lang=\"ts\">",
			"export default {",
			"    name: \"\"",
			"}",
			"</script>",
			"<script lang=\"ts\" setup>",
			"import { ref, reactive } from 'vue'",
			"</script>",
			"\n",
			"<style lang=\"less\" scoped>",
			"   $3",
			"</style>",
		],
		"description": "Log output to console"
	}
}
```
### 使用
在vscode中输入vue即可快捷生成指定的代码块