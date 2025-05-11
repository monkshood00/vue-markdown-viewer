# Javaエンジニア3年目がVue.jsでMarkdownビューアを作ってみた話【Vite + Vue3】

## 🎯 はじめに：なぜJava以外を学ぼうと思ったのか

私はJavaエンジニアとして3年目になりますが、バックエンドだけでなく**フロントエンドの技術もキャッチアップしたい**と考えるようになりました。

理由は以下の通りです：

- 業務外でも技術の幅を広げたい
- フロント側の知識があれば、現場でのコミュニケーションや設計にも活かせる
- 個人開発にもチャレンジしたい

そこで今回は、**JavaScript（以下JS）での開発**に挑戦しました。

---

## 🤔 フロントエンドFWの選定

まず、JSで開発するならどのフレームワークを選ぶか？  
以下のように比較して、最終的に**Vue.js**を選びました。

| フレームワーク     | 特徴                         | 評価    |
| ----------- | -------------------------- | ----- |
| React       | 実務や転職に強い。Meta製。やや習得難       | ★★★★☆ |
| Vue.js      | 学習コストが低く、HTMLに近い記法。日本語情報豊富 | ★★★★★ |
| Angular     | Google製。大規模向け。学習コスト高め      | ★★☆☆☆ |
| jQuery      | もう古い。DOM操作が主で今は使われない       | ☆☆☆☆☆ |
| Backbone.js | 完全にレガシー。使う理由がない            | ×     |

### ✅ Vue.jsを選んだ理由

- HTMLに近くて書きやすい
- とりあえず動くものが作りやすい
- 後にReactにも移行できる応用力あり
- 個人開発にピッタリ

---

## 🔧 Vue.jsの構成選定（Vite一択！）

Vueのプロジェクト構築方法もいくつかありますが、今回は以下を選択。

| 方法             | 特徴                     | 対象       |
| -------------- | ---------------------- | -------- |
| Vue CDN版       | HTMLに直接読み込み。お試し用       | 初心者・試すだけ |
| Vue CLI        | Vue2/3対応。やや古い構成        | レガシー向け   |
| **Vite + Vue** | モダン構成、ビルド爆速、ホットリロード    | ✅ 今回はこれ！ |
| Nuxt.js        | VueのSSR/SSGフレームワーク。本格派 | 中～上級者向け  |

---

## 💠 作ったもの：Markdownブログビューア

### ✨ 概要

Markdownとはプレーンテキストで書ける軽量マークアップ言語で、QiitaやGitHubでも使われている書式です。今回のビューアは、そのMarkdownファイルをHTMLに変換して表示するシンプルなWebアプリです。

Markdownで書いた記事を、Vueアプリ上でHTMLとして表示するビューアを作りました。最終的には、**Vercelで無料公開 → Qiita/Zennに成果を投稿**という流れです。

### 📌 機能

- `*.md` ファイルを読み込んでHTML表示（`marked`ライブラリ使用）
- Vite + Vue 3構成
- シンプルなブログUI
- Vercelでデプロイ

### 🔗 公開先

- 🌐 アプリ: [https://vue-markdown-viewer-3fqph5e2f-monkshood00s-projects.vercel.app](https://vue-markdown-viewer-3fqph5e2f-monkshood00s-projects.vercel.app)
- 💻 GitHub: [https://github.com/monkshood00/vue-markdown-viewer](https://github.com/monkshood00/vue-markdown-viewer)

---

## 🧪 実装ポイント

```bash
npm create vite@latest
# フレームワークに Vue を選択
cd プロジェクト名
npm install
npm install marked
```

### 🔍 MarkdownViewer.vue

```vue
<template>
  <div v-html="htmlContent"></div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { marked } from 'marked'

const htmlContent = ref('')

// マークダウンファイルを取得して、HTMLに変換
onMounted(async () => {
  const res = await fetch('/posts/sample.md') // ここでファイルを取得
  const md = await res.text()
  htmlContent.value = marked(md) // markedライブラリを使ってMarkdownをHTMLに変換
})
</script>
```

`MarkdownViewer.vue`では、Vueの`onMounted`フックを使ってマウント時に非同期でMarkdownファイルを取得し、`marked`ライブラリでHTMLに変換しています。

### 🧩 App.vueへの組み込み

```vue
<template>
  <MarkdownViewer />
</template>

<script setup>
import MarkdownViewer from './components/MarkdownViewer.vue'
</script>
```

`App.vue`では、`MarkdownViewer`コンポーネントをインポートして使っています。

---

## 🧑‍💻 `async` と `await` の解説

### `async` と `await` は非同期処理をシンプルに扱うためのキーワードです。

- `async` を関数の前に付けると、その関数は非同期関数として定義され、必ず `Promise` を返します。
- `await` は `async` 関数内で使うことで、非同期処理の完了を待ってから次の処理を実行します。

#### 例:

```javascript
async function fetchData() {
  const response = await fetch('https://api.example.com/data');
  const data = await response.json();
  console.log(data);
}
```

### 非同期処理のエラーハンドリング

```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('エラーが発生しました:', error);
  }
}
```

---

## 🚀 デプロイ：Vercelで一瞬公開

1. GitHubにコードをpush
2. Vercel公式サイトでGitHubと連携
3. 対象リポジトリを選ぶだけで即デプロイ完了！

URLが自動発行され、SSLもついています。

---

## 🎁 まとめ

- Javaエンジニアでも、フロントに挑戦すべき！
- Vueは学習コストが低く、最初の一歩に最適
- Markdownビューアは個人開発にちょうどよい題材
- 成果は必ず「発信」することで価値が上がる！

---

## 🙌 最後に

同じように「JavaだけじゃなくてJSもやりたい」「Vue触ってみたい」という方の参考になれば幸いです。  
ご質問・感想などあればコメントください！

---

## ✅ 補足・参考リンク

- Vue公式: [https://vuejs.org/](https://vuejs.org/)
- Vite公式: [https://vitejs.dev/](https://vitejs.dev/)
- marked: [https://github.com/markedjs/marked](https://github.com/markedjs/marked)
- async/await: [https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/async_function](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/async_function)