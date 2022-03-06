# 入門 & 環境構築

[公式](https://www.typescriptlang.org/#installation)

## 始めるのに必要なもの

- Node.js

そもそもtypescriptをインストールするのに`npm install`コマンドを実行する
npmはNode.jsをインストールすると利用できる

- TypeScriptコンパイラ

開発で作成したTypeScriptは、最終的にJavaScriptにコンパイルして実行するので、
TypeScriptコンパイラが必要

## インストール

- 基本のインストール

```
$ npm install typescript --save-dev
```

カレントディレクトリ内にインストールされるタイプ
`node_modules/`や`package.json`が作成される
プロジェクトごとにバージョンを分けるなどの時にいいかも

- グローバルインストール

```
# npm install -g typescript
```

/usr/local/~~ にインストールするタイプ
プロジェクト全体で同じバージョンを使いたいときにいいかも
バージョンわざわざ分ける必要ないので、今回はこっちでインストールする

- グローバルはrootじゃないとインストールできない

当たり前やけど/usr/local/~~ にインストールすることになるので、rootじゃないとinstallできない

```shell
[coke@imade-gcp01 typescript-deepdive]$ npm install -g typescript
npm ERR! code EACCES
npm ERR! syscall mkdir
npm ERR! path /usr/local/lib/node_modules/typescript
npm ERR! errno -13
npm ERR! Error: EACCES: permission denied, mkdir '/usr/local/lib/node_modules/typescript'
npm ERR!  [Error: EACCES: permission denied, mkdir '/usr/local/lib/node_modules/typescript'] {
npm ERR!   errno: -13,
npm ERR!   code: 'EACCES',
npm ERR!   syscall: 'mkdir',
npm ERR!   path: '/usr/local/lib/node_modules/typescript'
npm ERR! }
npm ERR! 
npm ERR! The operation was rejected by your operating system.
npm ERR! It is likely you do not have the permissions to access this file as the current user
npm ERR! 
npm ERR! If you believe this might be a permissions issue, please double-check the
npm ERR! permissions of the file and its containing directories, or try running
npm ERR! the command again as root/Administrator.

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/coke/.npm/_logs/2022-03-06T14_16_48_029Z-debug-0.log
```

- 確認

```
$ tsc -v
Version 4.6.2
```