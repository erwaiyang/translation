# Webpack — 令人困惑的部分

> This article is written by rajaraodv on  [medium](https://medium.com/@rajaraodv/webpack-the-confusing-parts-58712f8fcad9#.f7pdxr8fe).
> The traditional Chinese version is translated by Erwai, [with the permit from the author](https://medium.com/@erwai/many-thanks-rajaraodv-21bb42c93678#.slma5xuwi).

Webpack是React+Redux應用程式中最主要的模組打包器(module bundler)。我認為其他的前端框架，如Angular 2，在這些日子以來也大量使用Webpack。

當我第一次看到某個Webpack的設定檔，那簡直就是外星👽來的，非常令人困惑😱。在摸索了這東西一陣子之後，我現在認為，因為Webpack有著自己獨特的語法與新思維，這可能讓我們在一開始使用它時感到困惑。附帶一提，Webpack的這些思維也是讓它如此受歡迎的原因。

__因為在開始使用它時會感到困惑，我想我會寫一些文章，希望能讓其他人更容易上手，開始使用它強大的功能。這篇文章是第一篇：安裝。__

## Webpack的核心思想

以下兩項是Webpack的核心思想：

1. __所有東西都是模組__ — 就像JS檔案可以作為“模組”，其他的東西（CSS，圖片，HTML）也可以當作模組。也就是說，你可以 __require(“myJSfile.js”) 或 require(“myCSSfile.css”) __。這意味著，我們可以將任何檔案切割成更小、可以管理的片段（pieces），然後重複利用它們或是做別的用途。

2. __只在需要的時間載入你需要的東西（Load only “what” you need and “when” you need）__ — 典型的模組打包器將所有模組打包然後產生一個很大的單一檔案 “bundle.js”。__但是在許多實際情況中，這“bundle.js”可能有10MB～15MB之大，等它載入完成可能要幾千幾百年！__因此Webpack有著許多能__切割你的程式碼__、並產生多個“bundle”檔案的功能。__它同樣能非同步載入整個應用程式的各個部分__，讓你能夠只在需要的時間載入你需要的東西。

OK，讓我們來看看這些令人困惑的部分吧。

## 1. 開發模式vs產品模式（Development Vs Production）

第一個要注意的部分是，Webpack__有一大堆功能__。有些“只適用於開發模式”，有些則是“只適用於產品模式”，還有一些“在兩種情況都能使用”。

> 註：你可以放大圖片，閱讀上面的文字。

![]()

> 通常來說，大部分的專案會使用非常多的功能，以至於他們需要兩個很大的Webpack設定檔案。

你將會在__package.json__裡面寫scripts來產生bundles：

```JavaScript
“scripts”: {
 //npm run build to build production bundles
 //npm run build：產生產品用的bundles
 “build”: “webpack --config webpack.config.prod.js”,
 //npm run dev to generate development bundles and run dev. server
 //npm run dev：產生開發用的bundles然後運行dev. server
 “dev”: “webpack-dev-server”
}
```

## 2. webpack CLI Vs webpack-dev-server

關於模組打包器Webpack，有一點很重要--它提供了兩種介面：
1. Webpack CLI tool（Webpack命令列工具） — 預設介面（是Webpack本身的其中一部份，與Webpack一起安裝）

2. webpack-dev-server tool （Webpack開發用server工具）— 一個Node.js伺服器（你需要單獨安裝它）

### Webpack CLI (在產品模式時很有用)
這個工具能經由命令列取得參數（options），也從設定檔中取得參數， (預設為： webpack.config.js)再傳遞給Webpack進行打包工作。

> 你或許是從CLI開始學習Webpack，但將來，你幾乎只使用它來生成產品。

#### 用途：
```JavaScript
OPTION 1:
//Install it globally
//全域安裝它
npm install webpack --g
//Use it at the terminal
//在命令列中使用它
$ webpack //<--Generates bundle using webpack.config.js
//使用設定檔webpack.config.js生成bundles

OPTION 2 :
//Install it locally & add it to package.json
//本地安裝它＆寫入package.json
npm install webpack --save
//Add it to package.json's script
//將其增加到package.json的script部分
“scripts”: {
 “build”: “webpack --config webpack.config.prod.js -p”,
 ...
 }
//Use it by running the following:
//用下列命令來執行它
"npm run build"
```

### Webpack-dev-server (在開發模式中很有用)
這是一個在port 8080上執行的Express node.js server。這個server在內部會呼叫Webpack。這樣做的好處是它提供了一些額外的功能，像是重新整理瀏覽器， i.e.“Live Reloading”，以及/或替換方才被更動的模組，i.e.“Hot Module Replacement” (HMR)。

#### 用途：
```JavaScript
OPTION 1:
//Install it globally
//全域安裝它
npm install webpack-dev-server --save
//Use it at the terminal
//在命令列中使用它
$ webpack-dev-server --inline --hot
OPTION 2:
// Add it to package.json's script
//將它增加到package.json的script部分

“scripts”: {
 “start”: “webpack-dev-server --inline --hot”,
 ...
 }
// Use it by running
//在命令列中執行
$ npm start
Open browser at:
http://localhost:8080
```

### Webpack Vs webpack-dev-server options

It’s worth noting that some of the options like “inline” and “hot” are webpack-dev-server only options. Where as some others like “hide-modules” are CLI only options.
