---
categories : ["golang", "hugo"]
date : "2017-04-23T00:22:30+08:00"
description : "Hugo := markdown files->>static html web site"
keywords : ["hugo", "golang"]
title : "使用hugo建立靜態的blog"

---
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
- 使用[Markdown](http://markdown.tw)，對於一般使用者有一定的門檻
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
想要潮一點，請到[Hugo Themes Site](https://themes.gohugo.io)挑一個順眼的主題來安裝，基本上主題的簡介都會有安裝的流程說明，將主題下載後放到hugo目錄下的`themes`目錄，並根據所選主題修改config.toml檔案。
```
...
# 主題
theme = "hyde-x"
...
```

#### 顯示圖片
![hugo blog](/image/creake-hugo-blog.png)
將圖片檔放置在`static/image/`資料夾中(於static下，資料夾路徑可自訂)
加入`![hugo blog](/image/creake-hugo-blog.png)`即可顯示圖片

#### 如何自動化上傳到github上

1.將git設定好
```bash
# 初始化 git
git init
# 新建 gh-pages branch，用來存放產生的靜態html檔案
git checkout -b gh-pages
# 新建 blog branch，用來存放編輯的檔案
git checkout -b blog
git add -A
# 先於github開好repository並加入遠端列表中
git remote add origin https://github.com/user/repo.git
```

2.寫你的文章吧！

3.寫完文章好只要執行這個自動化的script，可以將部落格上傳到github上

```bash
#!/bin/zsh
cd ~/path/to/hugosite
git checkout blog
hugo
git add -A
git commit -m "update content"
rm -rf /tmp/blog
cp -r public/ /tmp/blog
git checkout gh-pages
cp -rf /tmp/blog/* ./
rm -rf /tmp/blog
git add -A
git commit -m "update content"
git push --all origin
git checkout blog
```

網站網址就會是 `github使用者名稱.github.io/建立的repo名稱`

#### 如果要使用自訂網域
1.github->設定->Custom domain-->將自己的domain name加上去

或是

建立一個`CNAME`檔案，並將網域名稱寫在裡面

例如：
```
blog.qtlin.tw
```

2.接著去設定你的DNS代管新增兩筆`A`records

--> 192.30.252.153

--> 192.30.252.154

關於github page的資訊可以參考[官方文件](https://help.github.com/categories/customizing-github-pages/)