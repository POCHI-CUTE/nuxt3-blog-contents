---
title: Nuxt3の環境構築で詰まった部分(PagesDir、nuxt.config.ts)
tags:
---

Nuxt3 ではディレクトリ構成がかなり Nuxt2 と比べてシンプルでした。  
Nuxt2 に似た構成でやりたいなということで`src`ディレクトリを作って以下に`Pages`ディレクトリなどを作ってアプリ作ろうとしていました。  
Nuxt3 を訳も分からず触ったため何点か詰まった部分があったので備忘録として書きます。

# Pages ディレクトリを作ってファイルを作るだけじゃ反映されない

Nuxt2 で勝手に Pages ディレクトリの下にあればルーティングされていたので脳死で Pages ディレクトリを作ったが反映されなかった。
pages ディレクトリを使う際は、初期にある`app.vue`ファイルに

```js
<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>
```

真ん中の`<NuxtPage/>`を入れることで Pages ディレクトリを読み込んで、使えるようになる。  
`<NuxtLayout>`はこのままだと layouts ディレクトリの default.vue を適用できるようになる。
https://v3.nuxtjs.org/guide/directory-structure/app
https://v3.nuxtjs.org/guide/directory-structure/layouts

# src ディレクトリ下にコードを置くのには config での設定がいる。

`Pages`ディレクトリに書いても全く反映されないので、なにがおかしいのか分からなかったのですが、まず以下の記事を見てとりあえず解決しました。原因は`src`を勝手に作ってそこにコードを置いて行ったので当たり前ですが`nuxt.config.ts`でルートディレクトリを変更する設定がいりました。

```js
export default defineNuxtConfig({ ... rootDir: "src/" })
```

https://qiita.com/teracy164/items/97f35b1550cd2080197b  
ただ config ファイルたちはルートディレクトリの下にあって欲しいなと思っていたところ、以下で設定できそうなことがわかりました。

```js
export default defineNuxtConfig({ ... srcDir: "src/" })
```

srcDir でソースディレクトリの変更を行うことができました。  
rootDir:  
https://v3.nuxtjs.org/api/configuration/nuxt-config#rootdir  
srcDir :  
https://v3.nuxtjs.org/api/configuration/nuxt-config#srcdir
