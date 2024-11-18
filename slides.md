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

まずApp Router採用に対する、個人の見解をお話しします。

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
    - ※多分全ての技術選定に言えること
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
layout: section
---

# 「Next.jsの考え方」の要点

[Next.jsの考え方](https://zenn.dev/akfm/books/nextjs-basic-principle)の内容を一部抜粋して紹介します。

---

# 「Next.jsの考え方」の要点

より詳しくは、ぜひ[Zenn](https://zenn.dev/akfm/books/nextjs-basic-principle)を参照ください。

- データフェッチ on Server Components
- データフェッチ コロケーション
- Streamingの活用
- Server Actionsを利用したデータ操作
- Static Rendering・Dynamic Rendering・PPR

---
transition: fade
---

# データフェッチ on Server Components

データフェッチはClient Componentsではなく、Server Componentsで行いましょう。

- Reactが従来抱えていた問題の本質は「Reactがサーバーを活用できていないこと」であるとし、再設計されたアーキテクチャが**React Server Components**である
  - React Server Components≠Server Components
- Server Componentsでデータフェッチを行うことで、様々なメリットを享受できる
  - 高速なバックエンドアクセス
  - シンプルでセキュアな実装
  - バンドルサイズの軽減

---

# データフェッチ on Server Components

データフェッチはClient Componentsではなく、Server Componentsで行いましょう。

```tsx {all|2,3}
export async function ProductCard({ id }: { id: string }) {
  const res = await fetch(`https://dummyjson.com/products/${id}`);
  const product: Product = await res.json();

  return (
    <div className={/* ... */}>
      <h2>{product.title}</h2>
      {/* ... */}
    </div>
  );
}
```

---
transition: fade
---

# データフェッチ コロケーション

データフェッチはデータを参照するコンポーネントにコロケーションし、コンポーネントの独立性を高めましょう。

- コロケーションとは？
  - > コードをできるだけ関連性のある場所に配置することを指します。
- データフェッチ コロケーションとは？
  - データフェッチ処理をデータを参照するコンポーネントやその近くで行うこと
  - Props Drilling（バケツリレー）の逆
- データフェッチの管理が破綻しない？
  - Metaの大規模プロダクトにおいても従来より、GraphQLコロケーションを用いたデータフェッチコロケーションが採用されてきた
  - `fetch()`においても破綻しないよう、Next.jsではRequest Memoizationを提供してる

---

# データフェッチ コロケーション

データフェッチはデータを参照するコンポーネントにコロケーションし、コンポーネントの独立性を高めましょう。

```tsx {all|2-6|13,14}
// 従来
export const getServerSideProps = (async () => {
  const res = await fetch("https://dummyjson.com/products/1");
  const product = await res.json();
  return { props: { product } };
}) satisfies GetServerSideProps<ProductProps>;

export default function ProductPage({
  product,
}: InferGetServerSidePropsType<typeof getServerSideProps>) {
  return (
    <Layout>
      {/* 🚨Props Drilling🚨 */}
      <Product product={product} />
    </Layout>
  );
}
```

---
transition: fade
---

# Streamingの活用

特に重いコンポーネントのレンダリングは`<Suspense>`で遅延させて、Streaming SSRにしましょう。

- 重いデータフェッチを伴う場合、該当部分だけClient fetchにするようなパターンが過去よく見られた
  - Zennなどもまさにそう
- App Routerは**Streaming SSR**に対応しているので、`<Suspense>`で該当箇所をStreamingにすることが可能

---
transition: fade
---

# Streamingの活用

特に重いコンポーネントのレンダリングは`<Suspense>`で遅延させて、Streaming SSRにしましょう。

```tsx {all|13-17|6-8}
export default function Page() {
  return (
    <div>
      <h1>Streaming SSR</h1>
      <Clock />
      <Suspense fallback={<>loading...</>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}

async function LazyComponent() {
  await setTimeout(3000);

  return <p>Lazy Component</p>;
}
```

---

# Streamingの活用

特に重いコンポーネントのレンダリングは`<Suspense>`で遅延させて、Streaming SSRにしましょう。

<div class="flex space-x-10 my-10">
  <img src="https://res.cloudinary.com/zenn/image/fetch/s--v5QQf4Qh--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/e63dff72f053c2611819e2a5.png%3Fsha%3D98adb18104055ba50cc5a09179cd40c5f98910d3" class="w-100">
  <img src="https://res.cloudinary.com/zenn/image/fetch/s--ATF7qLgU--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/4ee185577400d6aedfd8cc44.png%3Fsha%3D4578cf7ff142113f571d5641d52159f6da41b786" class="w-100">
</div>

---

# Server Actionsを利用したデータ操作

TBW

---

TBW

- Server Actionsを利用したデータ操作
- Static Rendering・Dynamic Rendering・PPR
