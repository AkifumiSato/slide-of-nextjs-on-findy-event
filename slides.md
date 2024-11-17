---
theme: default
# https://unsplash.com/ja/%E5%86%99%E7%9C%9F/%E3%82%B3%E3%83%B3%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%88%E3%81%AE%E5%BB%BA%E7%89%A9%E3%81%AE%E8%9E%BA%E6%97%8B%E9%9A%8E%E6%AE%B5-dj4v4Qdu4H8
background: https://images.unsplash.com/photo-1676407795690-96bae82ef17c?q=80&w=2370&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
highlighter: shiki
lineNumbers: false
title: 「Next.jsの考え方」と「Next.jsのこれから」
info: |
  ## 「Next.jsの考え方」と「Next.jsのこれから」

class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
---

# 「Next.jsの考え方」<br>「Next.jsのこれから」

これまでの考え方、これからの考え方

---

# Profile

- About
  - [akfm_sato(あっきー)](https://x.com/akfm_sato)
  - Next.js
  - Rust
  - テスト設計
  - アジャイル
- Activity
  - https://zenn.dev/akfm
  - [Offers - Next.js v15 アップデート解説イベント](https://offers-jp.connpass.com/event/328878/)
  - [JS Conf 2023](https://main--remarkable-figolla-a694f0.netlify.app/1)
  - [Rust入門本の執筆](https://www.shuwasystem.co.jp/book/9784798067315.html)
  - etc...

---

# Agenda

本日の流れ

1. App Routerを採用すべきかどうか
2. 「Next.jsの考え方」の要点
3. QA
4. Next.jsのこれから

---
layout: section
---

# App Routerを<br>採用すべきかどうか

---

# 日本におけるNext.jsの現状

個人の主観に基づく見解だが、日本においてはこういう状況に思える

- Pages Router
  - 使ってる人は一定以上いる
- App Router
  - 興味がある人も一定以上いる
  - 仕事で使ってる人は少ない
  - ついていけない人も多い
  - 思想が受け入れられない人も一定数いる
- Next.js自体、変化が激しいのでついていくのが大変

---
transition: fade
---

# Q. App Routerを採用すべきかどうか

「App Routerにすべきですか？」「App Router触っといた方がいいですか？」

- 結論: 状況によりけり
  - <span v-mark="{ at: 1, color: 'red', type: 'underline'}">プロダクト特性やチームのスキルセットなど様々な要因を鑑みないとなんとも言えない</span>
    - 多分全ての技術選定に言えること
  - パフォーマンスや開発効率の向上は期待できる
  - 学習コストは求められる理解度は高め

---

# Q. App Routerを採用すべきかどうか

「App Routerにすべきですか？」「App Router触っといた方がいいですか？」

- <span v-mark="{ at: 1, color: 'red', type: 'underline'}">おすすめ: App Routerを学んでおくことはそれなりに価値がある</span>
  - 必要になってから学び始めると大変
  - Next.jsは今日時点で最も大きな影響力を持つフレームワークと考えられる
  - RSCの知識は無駄にはなりにくい
  - Pages Routerが削除される可能性は少ないが、廃れていくのは間違いない

---
layout: fact
---

## 「採用すべき」🤔<br>「学ぶべき」✅

---
layout: section
---

# 「Next.jsの考え方」の要点

---

TBW

- データフェッチ on Server Components
- Streamingの活用
- データフェッチ コロケーション
- Server Actionsを利用したデータ操作
- Static Rendering・Dynamic Rendering・PPR
