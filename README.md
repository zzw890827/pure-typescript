# TypeScript

- Version: 4.1.3
- [公式サイト](https://www.typescriptlang.org/)

## 事前知識

- （必須）JavaScript
- （望ましい）ES6以上の知識

## レクチャーの概要

1. 入門編
   - [Definition](./01.Definitaion/README.md): TypeScriptの紹介及び前提知識
   - [Types](./02.Types/README.md): TypeScriptのタイプの種類の紹介
   - [Interface](./03.Interface/README.md): TypeScriptが用意した注釈ツール
   - [OOP](./04.OOP/README.md): TypeScriptの特徴のオブジェクト指向
2. 応用編
   - [Generics](./05.Generics/README.md): APIやFWなどを開発時によく使われている型引数
   - [Enum](./06.Enum/README.md): JavaScriptでは利用できない列挙型
   - [Modularization](./07.Modularization/README.md): モジュール化のあるべき姿
   - [Guard](./08.Guard/README.md): ユニオンタイプを使用時に変数の保護機能
   - [DTS](./09.DTS/README.md): 型定義ファイルの書き方
   - [Decorator](./10.Decorator/README.md): デコレータの書き方及び使用方法（作成中）

## サンプルコードの実行方法

サンプルコードの実行方法はいくつかがあります。
大前提として`Node.js`が必須です。

Node.jsを[ダウンロード](https://nodejs.org/ja/)してインストールすること

### グローバルts-nodeを用いて実行

1. `TypeScript`をグローバルインストールします

   ```bash
   npm i typescript -g
   ```

2. `ts-node`をグローバルインストールします

   ```bash
   npm i ts-node -g
   ```

3. 動作確認: 下記の`index.ts`を作成しましょう

   ```typescript
   // index.ts
   const message: string = 'Hello TypeScript.'
   console.log(message);
   ```

4. 実行

   ```bash
   ts-node index.ts
   ```

5. `index.ts`の内容をサンプルコードに上書きをして動作確認することができます

### TypeScriptネイティブHMR統合環境

- [場所](./11.Playground/node)

### Parcelを使用したHMR統合環境

- [場所](./11.Playground/parcel)

## 実践の概要

>作成中、しばらくお待ちください。
