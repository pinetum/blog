+++
categories = []
date = "2018-04-18T20:35:40+08:00"
description = "MQTT x Websocket x Mosquitto"
keywords = []
title = "使用Mosquitto作為MQTT broker並支援websockets連線"

+++


- 安裝 mosquitto

`brew install mosquitto`

- 啟動服務

`brew services start mosquitto`

編輯`/usr/local/etc/mosquitto/mosquitto.conf`

```
# 原本mqtt broker的port
listener 1883
protocol mqtt
# 使用1884作為websockets使用的port
listener 1884
protocol websockets
```

- 重新啟動服務

`brew services restart mosquitto`

