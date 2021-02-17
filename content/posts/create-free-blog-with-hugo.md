---
categories : ["golang", "hugo", "blog"]
date : "2021-02-16T00:22:30+08:00"
description : "建立免維護費用的部落格"
keywords : ["hugo", "golang", "blog"]
title : "使用hugo建立完全免費的blog"

---

本文將介紹如何使用hugo搭配gitlab/github pages建立一個免維護費用的部落格!


## hugo簡介與安裝

在CMS(content management system)當中最主流的套裝軟體非Wordpress莫屬，但維護一個wordpress其實是成本昂貴的，你可能可以直接向wordpress.com購買服務或是自行租用虛擬主機架設，也因為wordpress的市占率很高加上是開放原始碼的系統，成為駭客的首要研究目標，所以可能還需要關注是否有漏洞，自己有沒有受影響與需不需要升級，升級有失敗的風險等等。

而CMS還有另一種選擇--靜態網站生成器，著名的有Hugo、Jekyll、Hexo、Octopress等(按github星星數目排序)，其中各有優缺點，雖然相較之下hugo的插件與主題數目可能不多，但是他速度非常快且安裝非常簡單。

Hugo為golang撰寫的程式，支援大部分的作業系統:(macOS (Darwin) for x64, i386, and ARM architectures, Windows, Linux, OpenBSD, FreeBSD)

### 下載與安裝Hugo

可以透過套件管理程式安裝，例如:`sudo apt install hugo`(Ubuntu/debain)`brew install hugo`(macOS)`choco  install hugo`，如此一來就安裝完畢了。

如果沒有套件管理程式也沒關係，可以進入Hugo於[github的release page](https://github.com/gohugoio/hugo/releases)根據自己的作業系統版本選擇並下載，例如windows版請找到[hugo_0.80.0_Windows-64bit.zip](https://github.com/gohugoio/hugo/releases/download/v0.80.0/hugo_0.80.0_Windows-64bit.zip)

解壓縮後將他放到系統PATH路徑底下即可:
- Linux/macOS: `/usr/local/bin`
- Windows: 可以在c槽建立一個hugo資料夾，並將hugo.exe放入最後將`c:\hugo`加入系統的環境變數中

接著就可以測試安裝有沒有成功，開啟Terminal並輸入`hugo version`  (windows請開啟`命令提示字元`)
```
Hugo Static Site Generator v0.80.0-792EF0F4 linux/amd64 BuildDate: 2020-12-31T13:37:58Z
```
如果出現版本資訊就代表安裝成功囉

## 建立網站

首先移動Terminal的路徑到你要存放網站原始碼的路徑，接著我們就可以透過指令`hugo new site mySite`來建立第一個網站，其中`mySite`可以替換成你的網站名稱
```
Congratulations! Your new Hugo site is created in ~/test/mySite.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".
```

出現上面的訊息就代表建立成功了，接著我們先到[https://themes.gohugo.io/](https://themes.gohugo.io/)挑選網站的佈景主題吧。

### 挑選布景主題
### 建立文章
### 啟動本地的伺服器
### 語法簡介
### 自動縮圖
### 浮水印
## 部屬網站
### github pages
### gitlab pages
### 自動化部屬
### 自訂網域(網址)