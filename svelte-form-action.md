<!--
title:   SvelteKitのForm Actionでオブジェクトを送信する
tags:    Svelte,SvelteKit,TypeScript
private: false
-->

# FormAction

svelteKitの FormActionは、フォームの送信を制御するためのコンポーネントです。

ただし、通常のstring型のバインドでは、単なる文字列しか送信できません。
この記事ではForm Actionを使ってオブジェクトを送信する方法を紹介します。

## はじめに

Form Actionは、フォームの送信を制御するためのコンポーネントです。

いかに実装例を示します

```html
<script lang="ts">
    import type { ActionData } from './$types';
    export let form: ActionData;
    let message: string = '';
</script>

<form method="POST">
    <button type="submit">
        send
    </button>
    <input type="text" bind:value={message} />
</form>
```

上記の実装で、+page.server.tsに記述されたform actionを実行することができます。

## オブジェクトを送信する

objectを送信するには、以下のように実装します。

```html
<script lang="ts">
    import type { ActionData } from './$types';
    export let form: ActionData;
    let item: myObject = {
        message: 'Hello',
        person: 'james'
    };

    let jsonItem = JSON.stringify(item);
    type myObject = {
        message: string,
        person: string
    };
</script>

<form method="POST">
    <button type="submit">
        send
    </button>
    <input type="text" bind:value={jsonItem} />
</form>
```

このようにJSON文字列として送信することで、オブジェクトを送信することができます。

## まとめ

Form Actionは、フォームの送信を制御するためのコンポーネントです。

ただし、通常のstring型のバインドでは、単なる文字列しか送信できません。
この記事ではForm Actionを使ってオブジェクトを送信する方法を紹介しました。

## 参考

- [FormAction](https://kit.svelte.dev/docs/form-actions)
