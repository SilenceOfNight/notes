# Markdown

[TOC]

## Sublime Plugins

#### 1. MarkDown Editing
    此插件使得Sublime编辑器支持Markdown语法高亮。支持Github Favored Markdown语法。

#### 2. OmniMarkupPrevie  
    此插件可以实现在浏览器中实时预览。

###### 可以遇到的错误
![OmniMarkupPrevie Error](./images/OmniMarkupPrevie-error.png)
> ##### 解决方案
> 在Sublime Text > Preferences > Package Settings > OmniMarkupPreviewer > Settings - User  中追加以下内容:  
>     {
>         "renderer_options-MarkdownRenderer": {
>          "extensions": ["tables", "fenced_code", "codehilite"]
>         }
>     }

###### 常用快捷键
1. Ctrl + Alt + O : 打开浏览器，并预览Markdown文本。
2. Ctrl + Alt + X : 自动导出HTML文件到Markdown文本所在的目录下。
3. Ctrl + Alt + C : 复制HTML

## 基础语法
[Markdown 语法说明 (简体中文版)](http://www.appinn.com/markdown/)

## 常见问题
###### 如何进行文本换行？
可以在段落末尾追<br\>或者2个空格。
###### 如何设置锚点？
``` html
<!-- 锚点引用 -->
[Title](#title-id)
<!-- 设置锚点 -->
<font id="title-id">Title</font>
```