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

#### 3. Monokai Extended & Markdown Extended
    Sublime诸多主题默认不支持Markdown语法的高亮显示，Monokai Extended & Markdown Extended可以很好的解决此问题。
###### 步骤
1. pcip **Monokai Extended**
2. pcip **Markdown Extended**
3. 点击右下角切**换文档格式**（Markdown Extended）。或者`Set Syntax: Markdown Extended`(Ctrl+Shift+P ssm)

#### 4. Table Editor
    辅助书写表格。自适应，自动对齐。书写表格时，使用Tab键。
###### 步骤
1. pcip Table Editor
2. Table Editor: Set table syntax 'EmacsOrgMode' for current view(Ctrl+Shift+P stso)

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