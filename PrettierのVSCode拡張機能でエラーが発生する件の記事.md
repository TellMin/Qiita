<!--
title:   PrettierのVSCode拡張機能でエラーが発生する件の記事
tags:    VSCode, prettier
id:      
private: false
-->

## tl;dr :zap:

`settings.json` に次の一行を追加する

```json
  "prettier.prettierPath": "./node_modules/prettier",
```

## 環境 :cooking:

- `Visual Studio Code - Insiders` : 1.79.0-insider (user setup)
- `Prettier - Code formatter` : v9.13.0
- `WLS` Ubuntu 22.04
- `asdf` v0.11.3
- `nodejs` 20.2.0

## 何が起きたのか :thinking:

上記環境で SvelteKit の開発環境構築を行っていたのだが、Prettierの拡張機能にてエラー。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2566826/fe45a90c-26e4-7eed-b95e-4883d6615314.png)

ログを確認すると

```log
["ERROR" - 9:21:35 PM] Cannot read properties of undefined (reading 'uid')
TypeError: Cannot read properties of undefined (reading 'uid')
    at Object.statSync (/home/tellmin/.vscode-server-insiders/bin/f6be5461f8bc69013a605f5baea834651c6589fb/node_modules/graceful-fs/polyfills.js:313:17)
    at p (/home/tellmin/.vscode-server-insiders/extensions/esbenp.prettier-vscode-9.13.0/dist/extension.js:1:35221)
    at L (/home/tellmin/.vscode-server-insiders/extensions/esbenp.prettier-vscode-9.13.0/dist/extension.js:1:37052)
    at /home/tellmin/.vscode-server-insiders/extensions/esbenp.prettier-vscode-9.13.0/dist/extension.js:1:36677
    at Function.e.exports [as sync] (/home/tellmin/.vscode-server-insiders/extensions/esbenp.prettier-vscode-9.13.0/dist/extension.js:1:36723)
    at t.ModuleResolver.findPkg (/home/tellmin/.vscode-server-insiders/extensions/esbenp.prettier-vscode-9.13.0/dist/extension.js:1:7297)
    at t.ModuleResolver.getPrettierInstance (/home/tellmin/.vscode-server-insiders/extensions/esbenp.prettier-vscode-9.13.0/dist/extension.js:1:3251)
    at t.default.handleActiveTextEditorChanged (/home/tellmin/.vscode-server-insiders/extensions/esbenp.prettier-vscode-9.13.0/dist/extension.js:1:9781)
```

ということで、'uid'というプロパティを読み込もうとしたときに、その親オブジェクトがundefinedであることが原因のようです。

調べてみると、`prettier`のリポジトリに次のようなissueが立てられてました。

- [Broken in vs code insiders - #3000](https://github.com/prettier/prettier-vscode/issues/3000)

そのなかで、次のようなコメントがありました。

> **Dnouv:**
> Well, not sure if it is the correct way to resolve but,
> 
> After changing the prettier path settings to the ones below, it does the trick.
> "prettier.prettierPath": "./node_modules/prettier"
> 
> Thank you!

ということで、`settings.json`に次のような設定を追加することで、エラーが解消されました。

```json
  "prettier.prettierPath": "./node_modules/prettier",
```

## まとめ :memo:

悩んでいた時間に対して、あっさりと解決してしまったので、記事にしてみました。
PrettierプラグインのModuleResolverのエラーのようですが、プロジェクトによっては正しく機能することもあったため、詳しい原因まではわからなかったのが心残りです。

さらに情報が分かり次第追記していきます。
どなたかのお役に立てれば幸いです。