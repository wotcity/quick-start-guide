# IoT App Frontend：30 分鐘 IoT 前端開發

WoT.City 開始支援 Live Streaming 功能，您將能使用 IoT 裝置進行線上視訊直接。目前 Live Streaming 功能尚在測試階段，同時間僅開放 5 個裝置。預計 6 月初完成系統測試後全面開放。

## Step 1：點撃 Live Streaming 頁面

登入 wotcity.com 後，點撃 *Device Manager* 頁面。系統會為每個裝置配置一個 *Live Video* 功能。請點撃裝置的 *Live Video* 按紐。

![圖 3.1：使用 Live Streaming 功能](https://raw.githubusercontent.com/jollen/wotcity-guide/master/live-streaming/3.1.png)

圖 3.1：使用 Live Streaming 功能

## Step 2：啟動 IoT 裝置視訊串流

以 Raspberry Pi 為例，使用 *ffmpeg* 即可輕鬆進行 live video streaming。將 *ffmpeg* 安裝至 Raspberry Pi 後輸入以下指令：

```
$ ffmpeg -s 640x480 -f video4linux2 -i /dev/video0 -f mpeg1video -b 800k -r 30 http://v.wot.city/object/5550937980d51931b3000009

```

參數說明：

* *-s 640x480* 是視訊尺寸，測試期間僅支援 640x480
* *-f video4linux2* 是輸入格式，這是 Linux 視訊擷取的驅動程式，請勿修改
* *-i /dev/video0* 是 USB Webcam 的裝置檔，將 USB Webcam 連接 Raspberry Pi 後自動產生
* *-f mpeg1video* 表示要以 MPEG1 格式推送視訊流到 WoT.City，請勿修改
* *http://v.wot.city/object/5550937980d51931b3000009* 是 WoT.City 的 video broker 服務，請將後面的數字修改為自已的 *Device Name* 即可

![圖 3.2：測試畫面](https://raw.githubusercontent.com/jollen/wotcity-guide/master/live-streaming/3.2.png)

圖 3.2：測試畫面

## Live Streaming over HTTP

WoT.City 的做法是讓 IoT 裝置透過 HTTP 來推送視訊流，說明如下：

* 易於整合現有技術，例如：*ffmpeg*
* 讓 WoT.City 的 Video Broker 支援 HLS（HTTP Live Streaming）
