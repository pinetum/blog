+++
categories = ["字型"]
date = "2017-04-27T21:25:06+08:00"
description = "=> >= <= === !== != -->  >-> ~~> <> <| |> 10x10 *** #{ #_( <$ "
keywords = ["字型"]
title = "給程式人專用的等寬連字字型 Firacode"

+++



#### [Firacode 等寬，專門為程式碼所設計的連字字體](https://github.com/tonsky/FiraCode/blob/master/README.md)
![firacode](/image/firacode.png)
![a](https://github.com/tonsky/FiraCode/raw/master/showcases/all_ligatures.png)

#### 等寬字體  vs. 比例字體
![比例字體vs等寬字體](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/Propvsmono.svg/220px-Propvsmono.svg.png)


目前大部分的編輯器與IDE幾乎都支援Ligature font，只要[下載](https://github.com/tonsky/FiraCode/releases)字體安裝到系統中並將編輯器字體調整即可，少數編輯器有較特別的設定方式，可以參考官方的[support list](https://github.com/tonsky/FiraCode/blob/master/README.md#editor-support) & [instructions](https://github.com/tonsky/FiraCode/wiki)

以VS code為例，因為設定部分目前沒有GUI畫面可以使用，需要修改`setting.json`設定檔（透過Code->Perfence->Setting開啟 或是以快捷鍵<kbd>⌘</kbd> + <kbd>,</kbd>）
```json
{
    ...
    "editor.fontFamily": "Fira Code",
    "editor.fontLigatures": true,
    "editor.fontSize": 14,
    ...
}
```
