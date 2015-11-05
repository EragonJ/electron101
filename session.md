# Session

## 如何改變預設的下載路徑 ? 

為何要改預設的下載路徑？因為 Electron 預設的下載路徑會指定到你 App 下的 `Downloads` 資料夾，所以如果你不仔細比較的話，你會以為檔案都被存到 `~/Downloads` 下，其實不是！！！這邊超級容易搞混的，我也是仔細比對後才發現 Mac OS 上的 `~/Downloads` 資料夾會有個預設 Icon 在旁邊，然後再搭配 Global Search 才確認了這件事情，所以如果哪天使用者一直抱怨說下載失敗的話，通常都是這個問題造成的。（對於非英語系的我們可能還可以從下圖的「下載項目」還有「Downloads」區分出來，但對其他英語系國家的人就很難分辨出來了！）

![Image](./images/session-path.png)

所以它預設的路徑對使用者（甚至是我們開發者）是相當難理解的，因此建議一定要改變預設的下載路徑，最保險的方法就是都改成 Desktop 就好了。

```javascript
var app = require('app');
var BrowserWindow = require('browser-window');

var win = new BrowserWindow({ width: 800, height: 600 });
win.loadUrl("http://github.com");

var session = win.webContents.session;
session.setDownloadPath(app.getPath('userDesktop'));
```

### 備註

1. 適用於從 `<webview>` 觸發的下載事件。但件事情成立的前提是你的 `<webview>` 必需要沒有使用 partition 這個值 !! 要不然你會發現下載視窗還是會導回原本預設的目錄 ... （這是一個大雷，我卡好久）
2. `session.setDownloadPath()` 在 v0.30.x, v0.31.x 是好的，v0.32.x 開始壞了，v0.33.8 後又修好了（我剛好用到 v0.33.x 壞掉的版本所以試超久 ... ）
3. **強烈建議一定要改這個預設值**

### 補充資料

1. https://github.com/atom/electron/issues/2855
2. https://github.com/atom/electron/issues/2987 
3. https://github.com/atom/electron/pull/3003
4. https://github.com/atom/electron/blob/master/docs/api/session.md#sessionsetdownloadpathpath
