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

# 「Next.jsの考え方」と<br>「Next.jsのこれから」

これまでの考え方、これからの考え方

---

# akfm's Profile

佐藤 昭文、[akfm_sato](https://x.com/akfm_sato)、あっきー

- Interest
  - Next.js
  - Rust
  - テスト設計
  - アジャイル
- Activity
  - https://zenn.dev/akfm
    - [Next.jsの考え方](https://zenn.dev/akfm/books/nextjs-basic-principle)
  - [Offers - Next.js v15 アップデート解説イベント](https://offers-jp.connpass.com/event/328878/)
  - [JS Conf 2023](https://main--remarkable-figolla-a694f0.netlify.app/1)
  - [Rust入門本の執筆](https://www.shuwasystem.co.jp/book/9784798067315.html)
  - etc...

---

# Agenda

本日の流れ

1. Next.js（App Router）にすべきかどうか
2. 「Next.jsの考え方」の要点
3. QA
4. Next.jsのこれから

---
layout: section
---

# Next.js（App Router）<br>にすべきかどうか

---

# 日本におけるNext.jsの現状

個人の主観に基づく見解だが、日本においてはこういう状況に思える

- Pages Routerを使ってる人は一定以上いる
- App Routerに...
  - 興味がある人も一定以上いる
  - 仕事で使ってる人は少ない
  - ついていけない人も多い
  - 思想が受け入れられない人も一定数いる
- よく大きな変化があるからついていくのが大変

---

# Q. Next.js（App Router）にすべきかどうか？

筆者の周りでもよく聞く疑問に対する個人的な見解

- 結論: 状況によりけり
  - プロダクト特性やチームのスキルセットなど様々な要因を鑑みないとなんとも言えない
- 意見1: 移行するメリットはある
  - Pages Routerが削除される可能性は少ないが、廃れていくのは間違いない
  - パフォーマンスや開発効率の向上は大きく期待できる
  - 学習コストが低いとは言えない
  - バグや困難な問題はそれなりに起きえる
- 意見2: 勉強して損になりにくい
  - RSCの知識は無駄にはなりにくい
  - 未だ非常に大きな勢力を持ったフレームワークである

---

TBW
