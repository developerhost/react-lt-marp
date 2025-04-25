---
title: 個人開発を集客から始めた話
author: 橋田至
marp: true
theme: gaia
---

## 個人開発、してますか？

- 個人開発は男のロマン！

---

## 個人開発の悩み

- 作っても誰にも使われない問題
- そもそも誰にも見られない。知られない

---

## なら集客から始めよう！

- とりあえず自己紹介サイトを作ってみることに

---

## 私の自己紹介サイトを React, Vite, Vercel で作成！

- 普通のポートフォリオサイトでは面白くないので、ドラクエ風に！
- サイト →https://my-dq-portfolio.vercel.app/
- GitHub→https://github.com/developerhost/my-dq-portfolio
  - 現在 19 スター獲得！
- Zenn にて技術記事を公開！

その結果...

---

## 現在、累計 2.5 万 PV, 4000 ユーザー獲得！

![test](./img/1.png)

↑

ここ拍手！

---

## 工夫した箇所

- RSS を用いて毎日更新された記事を取得

- src/rss/rss-parser.mjs

```js
import { writeFileSync } from "fs";

import Parser from "rss-parser";
const parser = new Parser();

(async () => {
  const rssFeed = {
    zenn: {
      label: "Zenn",
      url: "https://zenn.dev/dirtyman/feed",
      favicon: "https://zenn.dev/images/logo-transparent.png",
    },
    qiita: {
      label: "Qiita",
      url: "https://qiita.com/app_js/feed",
      favicon:
        "https://cdn.qiita.com/assets/favicons/public/production-c620d3e403342b1022967ba5e3db1aaa.ico",
    },
    note: {
      label: "Note",
      url: "https://note.com/dall_develop/rss",
      favicon:
        "https://assets.st-note.com/poc-image/manual/note-common-images/production/svg/production.ico",
    },
  };

  const allArticles = [];

  for (const [site, info] of Object.entries(rssFeed)) {
    try {
      const feed = await parser.parseURL(info.url);
      const articles = feed.items.map((item) => ({
        title: item.title || "",
        url: item.link || "",
        date: item.isoDate || "",
        thumbnail: item.enclosure?.url || "",
        favicon: info.favicon,
        site,
      }));
      allArticles.push(...articles);
    } catch (error) {
      console.error(`Error fetching feed for ${site}:`, error.message);
    }
  }

  writeFileSync("src/rss/data.json", JSON.stringify(allArticles, null, 2));
})();
```

---

## react-simple-typewriter を用いてドラクエ風のチャット UI を実現！

- src/components/ChatMessage.tsx

```tsx
import { Typewriter } from "react-simple-typewriter";

interface ChatMessageProps {
  message: string;
  typeSpeed?: number;
}

export const ChatMessage = ({ message, typeSpeed = 50 }: ChatMessageProps) => {
  return (
    <div className="z-20" style={{ whiteSpace: "pre-line" }}>
      <Typewriter
        cursor
        cursorStyle="_"
        delaySpeed={1000}
        key={message}
        typeSpeed={typeSpeed}
        words={[message]}
      />
    </div>
  );
};

export default ChatMessage;
```

---

## 集客がある程度出来たので、現在作りたかったサービスを開発中！

リリースしたらみんな、インストールして使ってみてね！

---

##

---

##

---

---

---

## 自己紹介
