---
title: React コンポーネントを追加する
---

このガイドは、あなたの Gatsby サイトにサードパーティコンポーネントライブラリーも含む React コンポーネントの追加方法を解説します。

## React コンポーネント

React コンポーネントは、ユーザーインターフェース（UI）を独立した再利用可能な部品として分割するために使えるすでに構築された要素、もしくは要素の集合です。React コンポーネントで書くことができるコンポーネントの種類は多くあるのですが、このガイドでは、関数コンポーネントを使用します。クラスコンポーネントを含む React コンポーネントの書き方について詳細を知りたい場合は、こちらの [React ドキュメント](https://ja.reactjs.org/docs/components-and-props.html)をご覧ください。

コンポーネントは、"props"（プロパティ）として知られる入力を用いてカスタマイズされる機能も提供されています。 Props は、ブーリアンや文字列、オブジェクトのような JavaScript の型になりえます。

例えば、あなたのサイト上のボタンに対してコンポーネントを使えます。これで、毎回異なるラベルやアクションを持つページを越えて、複数回使用できます。

## React コンポーネントをインポートする

Gatsby で React コンポーネントを使うときは、React アプリケーションと同じようにしてインポートして使用できます。ここでは、動作している [Gatsby Link](/docs/gatsby-link/) を例にします。そして、パフォーマンスのための追加機能をもたらします。

```jsx
import React from "react"
import { Link } from "gatsby"

export default () => (
  <div>
    <Link to="/contact/">Contact</Link>
  </div>
)
```

### サードパーティコンポーネントをインポートする

React と同様にして、Gatsby でもサードパーティコンポーネントとライブラリはサポートしています。あなたは、パッケージマネジャ経由でサードパーティコンポーネントやライブラリをインストールできます。私たちは、npm を好んで使用する傾向にあるので、npm を例にしています。

ここでは、サイトにサードパーティコンポーネントを追加する一例を挙げています。

はじめに、パッケージマネジャでコンポーネントやライブラリのパッケージをインストールしなければなりません。パッケージマネジャを組み合わせることはおすすめしません。なので、もしあなたが npm を使用するなら別のものは使用しないことをおすすめします。また逆もまた然りです。もし関連する Gatsby プラグインがある場合は、そのプラグインをインストールして使用する必要があります。

この例では、プラグインとプラグインに依存するライブラリーの両方をインストールします。

```shell
npm install gatsby-plugin-material-ui @material-ui/core
```

パッケージのインストールが完了したら、`gatsby-config.js` のプラグイン配列に必要なプラグインを追加します。

```js:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-material-ui`],
}
```

そして、あなたのページのソースコード上でインポートして使用します。

```jsx:title=index.js
import React from "react"

// わたしの素敵なサードパーティコンポーネントをインポートする
import Button from "@material-ui/core/Button"

export default () => (
  <div>
    <p>これは、わたしの超すごい Gatsby でできたページです!</p>

    {/* わたしの素敵なサードパーティコンポーネントを使用する */}
    <Button variant="contained">素敵なボタン!</Button>
  </div>
)
```

## 気をつけるべきこと

Gatsby は、サーバサイドレンダリング（SSR）を使用してサイトのページを生成しているため、たいていの場合、ブラウザがページをロードする前にあなたが書いた JSX のコードが、コンパイルされます。このため、ある機能は、コンパイルした時に利用できずビルドエラーとなることがあります。

### ブラウザグローバルの使用について

一部のコンポーネントやコードでは、`window`、`document`、`localStorage` のようなブラウザグローバルを参照することがあります。それらのオブジェクトでは、[ビルド](/docs/glossary#build)時点では利用できず、コンパイル時に webpack エラーが発生することもあります。

```text
WebpackError: ReferenceError: window is not defined
```

SSR およびクライアントサイドライブラリをサポートする解決策については、[Create React App から Gatsby への移植](/docs/porting-from-create-react-app-to-gatsby#server-side-rendering-and-browser-apis)に関連するセクションをご覧ください。

#### サードパーティモジュールを直す

一部のパッケージは、`window` もしくは、別のブラウザグローバルが定義されることを期待します。それらのパッケージは、パッチを適用する必要があります。

[HTML ビルドのデバッグ](/docs/debugging-html-builds/#fixing-third-party-modules)でそれらのパッケージにパッチを適用する方法を学べます。

### サーバサイドレンダリングなしのコンポーネント

サーバサイドレンダリングは、ページとコンテンツが Node.js サーバーでビルドされ、ブラウザーに送信されることを意味します。ユーザーに送信される前で、ページが構築されるようなものです。Gatsby では、ビルド時にサーバサイドでレンダリングされます。つまり、ブラウザーに到達するコードは、すでにページとコンテンツをビルドするために実行されています。しかし、これはまだ動的なページを保持できないことを意味します。

一部の React コンポーネントは、デフォルトでサーバサイドレンダリング（SSR）がサポートされていないため、SSR を自分自身で追加する必要があるかもしれません。
