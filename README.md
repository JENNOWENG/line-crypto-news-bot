# line-crypto-news-bot
# 建立專案資料夾
mkdir line-crypto-news-bot
cd line-crypto-news-bot

# 初始化專案
npm init -y

# 安裝所需套件
npm install express line-bot-sdk rss-parser
const express = require('express');
const line = require('@line/bot-sdk');
const app = express();

const config = {
  channelAccessToken: '你的Channel Access Token',
  channelSecret: '你的Channel Secret',
};

const client = new line.Client(config);

app.post('/webhook', line.middleware(config), (req, res) => {
  Promise.all(req.body.events.map(handleEvent))
    .then((result) => res.json(result));
});

function handleEvent(event) {
  if (event.type !== 'message' || event.message.type !== 'text') {
    return Promise.resolve(null);
  }

  return client.replyMessage(event.replyToken, {
    type: 'text',
    text: 'Hi, 加密新聞請點擊圖文選單！',
  });
}

const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`Listening on ${port}`));
