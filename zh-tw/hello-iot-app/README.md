# IoT App Frontend：30 分鐘 IoT 前端開發

將 ARM mbed 裝置「封裝」成 web app 只要簡單的 7 個步驟，而且不用寫程式。請閱讀 [Physical to App：30 分鐘物聯網上手、Code-less](http://wotcity.com/guide/hello-world/zh-tw) 教學文件，了解「裝置到 IoT App」的做法。

本文延續 [Physical to App：30 分鐘物聯網上手、Code-less](http://wotcity.com/guide/hello-world/zh-tw) 步驟 7 的教學，指導創客客製化 IoT App 的 frontend。

## Step 1：安裝 .CITY Starter Kit

1. 請登入 [Github](https://github.com/)
2. Fork [.CITY Starter Kit](https://github.com/wotcity/dotcity-starter-kit)：撃點該頁面上方的 *Fork* 按紐，將 .CITY Starter Kit 複製一份至自已的 Github 帳號下
3. 將專案 clone 至本地電腦。例如：

```
$ git clone https://github.com/jollen/dotcity-starter-kit.git
```

![圖 2.1：Fork .CITY Starter Kit](https://raw.githubusercontent.com/jollen/wotcity-guide/master/hello-iot-app/2.1.png)

圖 2.1：Fork .CITY Starter Kit

以上步驟需要有基本的 Github 操作經驗。

### 安裝開發環境

至 [Nodejs.org](http://nodejs.org/) 安裝 Node 執行環境。再依下列步驟安裝 .CITY Starter Kit 開發環境：

1. 切換至專案目錄：`$ cd dotcity-starter-kit`
2. 安裝 Gulp 命令工具：`$ npm install --global gulp`
3. 安裝 Bower 命令工具：`$ npm install --global bower`
4. 安裝 Node 相依套件：`$ npm install`
5. 安裝 Frontend 程式庫：`$ bower install`

請注意，使用 Windows 作業系統者，需要另行安裝 [Git for Windows](https://msysgit.github.io/)。

## Step 2：修改 ARM mbed 程式碼

ARM mbed 裝置以 JSON 文件推送感測器數據，可根據需要自行修改 JSON 文件內容。以下是一個修改範例：

1. 根據 [Physical to App：30 分鐘物聯網上手、Code-less](http://wotcity.com/guide/hello-world/zh-tw) 的說明，建立自已的 ARM mbed 專案
2. 找到 JSON 的格式化程式碼實作，並進行修改

根據 [Physical to App：30 分鐘物聯網上手、Code-less](http://wotcity.com/guide/hello-world/zh-tw) 為例，找到以下程式片斷：

```
        sprintf( data , "{ \"temperature\": %f }",temperature);
```

溫度數值原本是放在 *temperature* 裡，可以根據需要調整。例如：修改為 *temp*：

```
        sprintf( data , "{ \"temp\": %f }",temperature);
```

裝置推送的 JSON 文件，必須與 Frontend 的 Data Model 與 Template 互相對應。

## Step 3：修改 Frontend

開啟 `src/index.js` 文件，找到 Data Model 的定義：

```
app.Container = Backbone.Model.extend({
  ...
  defaults: {
    name: 'test',
    data: '',
    cid: 0
  },
  ...
});
```

將 Step 2 的 *temp* 加入 Data Model：

```
app.Container = Backbone.Model.extend({
  ...
  defaults: {
    name: 'test',
    data: '',
    cid: 0,
    temp: 0
  },
  ...
});
```
.CITY Starter Kit 範例包含一個顯示溫度的圖表（Chart），圖表的 Y 軸數據，經回 Data Model 裡的 *getY()* 函數回傳：

```
  // Y-Axis getter
  getY: function() {
    return this.get('temperature');
  }
```

這部份也要做相對應的修改：

```
  // Y-Axis getter
  getY: function() {
    return this.get('temp');
  }
```

開啟 `index.html` 文件，找到 Template 的定義：

```
    <script type="text/template" id="tmpl-status">
      <p class="lead"><%= data %></p>
    </script>
```

Template 裡的語法：*<%= data %>*，是引用變數值的寫法。Template 裡的變數名稱，就是 JSON 文件的 key：必須 ARM mbed 裝置的 JSON 文件一致。以上述範例來說，需修改如下：

```
    <script type="text/template" id="tmpl-status">
      <p class="lead"><%= temp %></p>
    </script>
```

以本文的範例來看，ARM mbed 裝置所推送給 WoT.City 的 JSON 文件如下：

```
{ "temp": 25 }
```

UI 的部份請自由發揮，例如：

```
    <script type="text/template" id="tmpl-status">
      <h1><%= temp %></h1>
    </script>
```

完成後進行程式碼的編譯（Browserify）。

## Step 4：編譯與測試

執行：

```
$ gulp apps
```

進行編譯。完成後，使用瀏覽器開放 `index.html` 文件即可。WoT.City 提供一個測試用的 device name：*testman*。瀏覽器開啟 `index.html` 後，再加上 frontend routing 的參數：

```
index.html#testman
```

當然可以指定想使用的 device name，例如：

```
index.html#5550937980d51931b3000009
```

Device name 的建立請參考 [Physical to App：30 分鐘物聯網上手、Code-less](http://wotcity.com/guide/hello-world/zh-tw) 的教學。


![圖 2.2：執行結果](https://raw.githubusercontent.com/jollen/wotcity-guide/master/hello-iot-app/2.2.png)

圖 2.2：執行結果

## Step 5：出版套件

完成 IoT App 開發後，就可以將 IoT App 套件「出版」至 WoT.City 平臺。出版至 WoT.City 後，系統 會自動套件設定為「App Theme」。出版成 App Theme 後，就可以使用自已的 App Theme 來建立 IoT App，而且還可以公開分享。

出版前，請開啟 *package.json* 檔案，修改幾個主要的套件資訊：

* 將 `name` 欄位修改為自已想要的套件名稱，例如：*jollen-starter-kit*。
* 修改 `version` 欄位，自訂套件的版本編號
* 修改 `preview` 欄位，指定畫面預覽圖檔。請幫自已的 App Theme 擷取一張畫面圖檔，檔案大小為 460x600
* 修改 `description` 欄位，簡單介紹 App Theme 的主要特色
* 根據需要修改 *README.md* 的內容（非必要）

第一次出版套件至 WoT.City 時，必須先登錄使用者資訊：

```
$ npm adduser --registry http://npm.wotcity.com
```

接著執行以下命令出版套件至 WoT.City：

```
$ npm set registry http://npm.wotcity.com
$ npm publish
$ npm set registry http://registry.npmjs.org
```

上述指令將 npm 伺服器修改為 `http://npm.wotcity.com`，出版套件後，再修改回 npm 官方的主機。

## Build IoT App

登入 WoT.City 後，至 [Build IoT App](http://wotcity.com/account/liveapp) 頁面即可找到你出版與分享的 App Them。
