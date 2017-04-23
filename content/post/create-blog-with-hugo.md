+++
categories = ["golang", "hugo"]
date = "2017-04-23T00:22:30+08:00"
description = "Hugo := markdown files->>static html web site"
keywords = ["hugo", "golang"]
title = "使用hugo建立一個靜態的blog"

+++
Hugo := markdown files->>static html web site
## <a href="https://gohugo.io" target="_blank">Hugo</a>

- 快速+現代的網站引擎
- 跨平台
- 使用[golang](https://golang.org)撰寫
- **[開源](https://github.com/spf13/hugo)**而且**免費**
- 根據自己的作業系統[下載](https://github.com/spf13/hugo/releases)編譯好的可執行檔

優點：

- 據說很快！（等文章多了以後再回來研究是有多快, 呵呵）
- 搭配github page使用無限流量，省去維護主機的成本
- 不用擔心部落格供應商倒閉
- 功能不夠可以自己[fork](https://git-scm.com/book/zh-tw/v2/GitHub-參與一個專案)增加或修改
- 比較不用擔心網站被打下來（？）
- 官方有很完整的[user guide](https://gohugo.io/overview/quickstart/)可以參考

缺點：

- 寫完文章後要自己把靜態網頁上傳到主機上（例如push到github上）
- 無法**輕易**的對圖片做排版
- 使用[Markdown](http://markdown.tw)，對於一般使用者有門檻([Blackfriday](https://github.com/russross/blackfriday))
- 需要自己找放置靜態網頁的地方
- 
~~網站掛了不能找客服嘴砲~~

#### 安裝
使用brew可以快速的在macOS上安裝
```bash
$ brew update && brew install hugo
```
#### 基本指令
~~懶人就只要記這些啦ob'_'ov~~
```
# 建立新的網站
$ hugo new site bookshelf 
# 建立新頁面
$ hugo new about.md
# 建立新文章（部落格使用時）
$ hugo new post/hello-world.md 
# 建立Server可以使用瀏覽器預覽，於修改markdown檔案時會自動重新整理網頁
$ hugo server  
# 產生靜態html檔案到public資料夾內
$ hugo  
```
#### Themes
想要潮一點，請到[Hugo Themes Site](https://themes.gohugo.io)挑一個順眼的主題來安裝。
基本上主題的簡介都會有安裝的流程說明，將主題下載後放到hugo目錄下的`themes`目錄，並根據所選主題修改config.toml檔案。
```
...
# 主題
theme = "hyde-x"
...
```

#### 關於圖片
![hugo blog](/image/creake-hugo-blog.png)
目前尚未找到簡單的方法來靠左或靠又顯示的方式......