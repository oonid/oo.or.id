---
title: "Telegram setWebhook with curl"
date: 2016-01-03T15:16:33+07:00
publishDate: 2016-01-03T15:16:33+07:00
image: "/images/2016/telegram_logo_512px.png"
url: "/content/telegram-setwebhook-curl"
tags: ["Telegram"]
draft: false
---

{{< figure src="/images/2016/telegram_logo_512px.png" title="Telegram setWebhook with curl" >}}


first, you can check current Webhook information [^1]:

```
curl -v https://api.telegram.org/botTOKEN/getWebhookInfo
```
replace TOKEN with your bot token, keep the "bot" before the TOKEN.

then setWebhook with:
```
curl -v -F "url=https://your.webhook.url" https://api.telegram.org/botTOKEN/setWebhook
```

that's all.

[^1]: https://core.telegram.org/bots/api
