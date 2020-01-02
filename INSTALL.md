# 安装配置环境 Install & Configure

## 文件布局 layout of files

* . 说明文件和配置文件
* ./EN 英文版本
* ./zh-CN 中文版本
* ./Images 图片文件

## 环境设置 enviroment setting

```javascript
// 安装 markdown-include
sudo npm install -g markdown-include
```

markdown.cn.json 中文版本配置文件

markdown.en.json 英文版本配置文件

## 合并markdown文件

```javascript
// 运行合并
markdown-include markdown.cn.json
markdown-include markdown.en.json
```

## 用vscode生成pdf文件

在vscode中安装 Markdown pdf插件，利用插件生成pdf。

按下f1键，输入markdown pdf，选择export pdf即可。

## 用pandoc生成pdf

安装pandoc

```javascript
brew install pandoc

cd zh-CN
pandoc -s README.cn.md -o README.cn.docx

cd ../EN
pandoc -s README.en.md -o README.en.docx
```

此时已经有docx文件，可以用WPS处理后导出成pdf。

## 常见索引

SWTC
public chain
RBFT
local image

前言 SWTC 致谢和作者
