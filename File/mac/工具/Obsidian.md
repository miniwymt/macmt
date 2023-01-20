---
title : obsidian
time : 2023-01-11 周三
tags : 
- markdown
- obplug
- 
---

## 文件结构

```sh
# 相关配置都在.obsidian目录下,方便迁移
.obsidian
├── plugins
│   ├── cm-editor-syntax-highlight-obsidian
│   ├── dataview
│   ├── obsidian-excalidraw-plugin
│   ├── obsidian-icon-folder
│   │   └── icons
│   │       ├── font-awesome-brands
│   │       ├── font-awesome-regular
│   │       ├── font-awesome-solid
│   │       ├── icon-brew
│   │       ├── remix-icons
│   │       └── simple-icons
│   ├── obsidian-image-auto-upload-plugin
│   ├── obsidian-pangu
│   ├── oz-image-plugin
│   └── table-editor-obsidian
└── themes
    └── Blue Topaz
```

## ob技巧

## 模版

适用于结构相同内容不同的文件

#### 日记

1. 日记是一种特殊的模板,在核心组件-日记-设置中进行配置

	- 日期格式设置YYYYMMDD_ddd
	- 日记存放目录Diary
	- 日记模板存放的位置Templates目录下新建一个模板

#### 其它模板

1. 自定义各种常用模版

	- 核心插件-模版中设置模板存放位置,然后才可以在新文件中插入模板	

	- 在Templates模板目录下新建会议记录模板	

	- 使用双大阔号占位符{{title}},自动填充文件标题	

	- 在官网帮助文件中学习更多的占位符,如{{date:YY-MM-DD}}{{time}}等

#### 快捷键

1. cmd + p 打开命令面板

## 双链

1. 出链到其它Markdown文件

		[[MarkDown]]

2. 出链到Markdown文件中的片段

		[[MarkDown#代码]]

3. 出链到Markdown文件中的非标题片段

		[[MarkDown#^165794]]

4. 出链到Markdown文件中的片段后创建别名

		[[MarkDown#^9477e5|别名]]

5. 出链到Markdown文件中的片段直接在当前文件中呈现

		 ![[MarkDown#代码]]

6. 链接到excalidraw并呈现

		![[CleanShot 2022-12-03 at 02.12.57@2x.png|200]]

7. 图片链接

	[![CodeQL](https://github.com/FlorianWoelki/obsidian-icon-folder/actions/workflows/codeql-analysis.yml/badge.svg)](https://www.baidu.com)

## 插件

#### 插件的调用方式

1. 界面调用

2. 命令面板调用

3. 设置调用

4.菜单调用

5.语法调用

#### 推荐插件

1. DataView

	-  在文件开头使用yaml语句定义的Metadata字段

	-  obsidian文件属性

	-  使用dataview查询语句进行查询

| 关键字                           | 作用                                                           | 示例                                                    |
| -------------------------------- |:-------------------------------------------------------------- | ------------------------------------------------------- |
| list table task field as myfield | 3种呈现方式                                                    | table file.ctime as time,file.size as size              |
| from                             | 标签使用#号,目录使用双引号,双中括号link,outgoing(双中括号link) | from "File"                                             |
| where                            | 筛选，支持and与or                                              | where file.size > 500 or contains(file.name,双引号文本) |
| sort                             | 排序,asc升序,desc降序                                          | sort file.size desc                                     |
```dataview
table file.ctime as ctime,file.size as size,tags
from "File"
where file.size > 500 and contains(file.name,"双链")
sort file.size desc
```

