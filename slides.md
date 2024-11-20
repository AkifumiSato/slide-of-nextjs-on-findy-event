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
3. 「Next.jsのこれから」

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
transition: fade
---

# Server Actionsを利用したデータ操作

データ操作はServer Actionsで実装することを基本としましょう。

- App RouterにおいてはServer Actionsで、データ操作を行うことが基本
  - Server ActionsはReact Server Componentsの仕様
  - App Routerにおいては、キャッシュ操作の役割も担っている

---

# Server Actionsを利用したデータ操作

データ操作はServer Actionsで実装することを基本としましょう。

```ts {all|1|7}
"use server";

import { revalidateTag } from "next/cache";

export async function createTodo(formData: FormData) {
  // ...
  revalidateTag("todos");
}
```

---
transition: fade
---

# Server Actionsを利用したデータ操作

データ操作はServer Actionsで実装することを基本としましょう。

```tsx {all|1|3,7}
"use client";

import { createTodo } from "./actions";

export default function CreateTodo() {
  return (
    <form action={createTodo}>
      {/* ... */}
      <button>Create Todo</button>
    </form>
  );
}
```

---
transition: fade
---

# Static Rendering・Dynamic Rendering

従来のSSGやISRに相当するのがStatic Rendering、SSRに相当するのがDynamic Renderingとなります。

| レンダリング                                                                                                                   | タイミング            | Pages Routerとの比較 |
| ------------------------------------------------------------------------------------------------------------------------------ | --------------------- | -------------------- |
| [Static Rendering](https://nextjs.org/docs/app/building-your-application/rendering/server-components#static-rendering-default) | build時やrevalidate後 | SSG・ISR相当         |
| [Dynamic Rendering](https://nextjs.org/docs/app/building-your-application/rendering/server-components#dynamic-rendering)       | ユーザーリクエスト時  | SSR相当              |

App Routerは<span v-mark="{ at: 1, color: 'red', type: 'underline'}">基本がStatic Rendering、特定の関数や設定を含むことでDynamic Rendering</span>となっています。

---
transition: fade
---

# Static Rendering・Dynamic Rendering

従来のSSGやISRに相当するのがStatic Rendering、SSRに相当するのがDynamic Renderingとなります。

[Dynamic Functions](https://nextjs.org/docs/app/building-your-application/rendering/server-components#dynamic-functions)を利用することによるDynamic Renderingの例

```tsx {all|4}
import { cookies } from "next/headers";

export default async function Page() {
  const cookieStore = await cookies(); // `cookies()`はDynamic Functionsと呼ばれるものの1つ
  const sessionId = cookieStore.get("session-id");

  return "...";
}
```

---
transition: fade
---

# Static Rendering・Dynamic Rendering

従来のSSGやISRに相当するのがStatic Rendering、SSRに相当するのがDynamic Renderingとなります。

`no-store`な`fetch()`によるDynamic Renderingの例

```tsx {all|8-10}
// page.tsx
export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;
  const res = await fetch(`https://dummyjson.com/products/${id}`, {
    cache: "no-store",
  });
  const product = await res.json();

  return "...";
}
```

---

# Static Rendering・Dynamic Rendering

従来のSSGやISRに相当するのがStatic Rendering、SSRに相当するのがDynamic Renderingとなります。

[Route Segment Config](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config)によるDynamic Renderingの例

```tsx
// layout.tsx | page.tsx
export const dynamic = "force-dynamic";
```

```tsx
// layout.tsx | page.tsx
export const revalidate = 0; // 1以上でStatic Rendering
```

---

# 続きは「Next.jsの考え方」にて

より詳しく「Next.jsの考え方」を知りたいと言う方は、[Zennで無料公開](https://zenn.dev/akfm/books/nextjs-basic-principle)してるのでぜひご参照ください。

<div class="flex justify-center my-10">
  <img src="https://res.cloudinary.com/zenn/image/fetch/s--K0PTgD12--/c_fill%2Cf_jpg%2Cfl_progressive%2Ch_700%2Cq_90%2Cw_500/https://storage.googleapis.com/zenn-user-upload/book_cover/4027ed2dc8.png" class="h-80">
</div>

---
layout: section
---

# 「Next.jsのこれから」

PPRやdynamicIOなど最新のNext.jsの動向についてお話しします。

---

# 「Next.jsのこれから」

これからのNext.jsを表す2つの大きなコンセプト

- [**PPR**](https://nextjs.org/docs/canary/app/api-reference/config/next-config-js/ppr)
- [**dynamicIO**](https://nextjs.org/docs/canary/app/api-reference/config/next-config-js/dynamicIO)

---
transition: fade
---

# PPR（Partial-Pre Rendering）

2023年のNext Confで発表された、乱立するレンダリングの形を1つにまとめたもの

- 従来、ページ単位でStatic RenderingとDynamic Renderingは選択されていた
- PPRでは、**基本はStatic Renderingとしつつ部分的にDynamic Rendering**にすることができる

<div class="flex justify-center my-10">
  <img src="https://res.cloudinary.com/zenn/image/fetch/s--tbcuP5e3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/6cc068f618fb070762011646.png%3Fsha%3D6d91c621f46a19fe589f47c84ea5199fdb0a7da2" class="h-70">
</div>

---
transition: fade
---

# PPR（Partial-Pre Rendering）

2023年のNext Confで発表された、乱立するレンダリングの形を1つにまとめたもの

<div class="flex space-x-10 my-10">
  <img src="https://res.cloudinary.com/zenn/image/fetch/s--2PGgEyqj--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/978ace959257180f2efac0fe.png%3Fsha%3Dbd99556d92d0b9d182be85899bb4f57e32b1828c" class="w-100">
  <img src="https://res.cloudinary.com/zenn/image/fetch/s--NxRy_1Ox--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/0a8f8fe2399f4776965fce2f.png%3Fsha%3D1c264decef63505ebe6fc5911a59d9fdbddbb901" class="w-100">
</div>

---

# PPR（Partial-Pre Rendering）

2023年のNext Confで発表された、乱立するレンダリングの形を1つにまとめたもの

```tsx {all|5-7}
export default function Page() {
  return (
    <>
      <StaticComponent />
      <Suspense fallback={<>Loading...</>}>
        <DynamicComponent />
      </Suspense>
    </>
  );
}
```

---

# dynamicIO

2024年のNext Confで発表された、Static 1stからDynamic 1stへの転換を目指すコンセプト

TBW

---

TBW

- 付録: Next.jsへの不満
  - Versioningに対する品質が残念
  - issueやPR見てくれない
  - RSCに対するエコシステムが未成熟
    - RTLがSCサポートしてない
    - StorybookのSCサポートが雑
  - セルフホスティングの難易度高すぎる
