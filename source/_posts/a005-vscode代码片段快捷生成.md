---
title: vscode代码片段快捷生成
categories:
  - 前端学习
tags:
  - vscode
date: 2022-07-08 11:17:01
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