# remark-stringify

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

マークダウンへのシリアライズをサポートする**[remark][]**プラグイン。

## 目次

* [これは何ですか？](#これは何ですか)
* [いつ使うべきですか？](#いつ使うべきですか)
* [インストール](#インストール)
* [使用方法](#使用方法)
* [API](#api)
  * [`unified().use(remarkStringify[, options])`](#unifieduseremarkstringify-options)
  * [`Options`](#options)
* [構文](#構文)
* [構文木](#構文木)
* [型](#型)
* [互換性](#互換性)
* [セキュリティ](#セキュリティ)
* [貢献](#貢献)
* [スポンサー](#スポンサー)
* [ライセンス](#ライセンス)

## これは何ですか？

このパッケージは、構文木を入力として受け取り、シリアライズされたマークダウンに変換する方法を定義する[unified][]（[remark][]）プラグインです。
使用すると、最終結果としてマークダウンがシリアライズされます。

remarkエコシステムについての情報は[モノレポのREADME][remark]を参照してください。

## いつ使うべきですか？

このプラグインは、unifiedにマークダウンのシリアライズ機能を追加します。
マークダウンの解析も必要な場合は、代わりに[`remark`][remark-core]を使用できます。これはunified、[`remark-parse`][remark-parse]、およびこのプラグインを組み合わせたものです。

プラグインを使用せず、構文木にアクセスできる場合は、このプラグイン内で使用されている[`mdast-util-to-markdown`][mdast-util-to-markdown]を直接使用できます。
remarkは、これらの内部を抽象化することで、コンテンツの変換を容易にすることに焦点を当てています。

このプラグインを他のプラグインと組み合わせて、構文拡張機能を追加できます。
深く統合されている注目すべき例として、
[`remark-gfm`][remark-gfm]、
[`remark-mdx`][remark-mdx]、
[`remark-frontmatter`][remark-frontmatter]、
[`remark-math`][remark-math]、
[`remark-directive`][remark-directive]があります。
また、`remark-stringify`の前に他の[remarkプラグイン][remark-plugin]を使用することもできます。

## インストール

このパッケージは[ESMのみ][esm]です。
Node.js（バージョン16+）では、[npm][]でインストールしてください：

```sh
npm install remark-stringify
```

Denoでは[`esm.sh`][esmsh]を使用：

```js
import remarkStringify from 'https://esm.sh/remark-stringify@11'
```

ブラウザでは[`esm.sh`][esmsh]を使用：

```html
<script type="module">
  import remarkStringify from 'https://esm.sh/remark-stringify@11?bundle'
</script>
```

## 使用方法

以下のような`example.js`モジュールがあるとします：

```js
import rehypeParse from 'rehype-parse'
import rehypeRemark from 'rehype-remark'
import remarkStringify from 'remark-stringify'
import {unified} from 'unified'

const doc = `
<h1>Uranus</h1>
<p><b>Uranus</b> is the seventh
<a href="/wiki/Planet" title="Planet">planet</a> from the Sun and is a gaseous
cyan <a href="/wiki/Ice_giant" title="Ice giant">ice giant</a>.</p>
`

const file = await unified()
  .use(rehypeParse)
  .use(rehypeRemark)
  .use(remarkStringify)
  .process(doc)

console.log(String(file))
```

`node example.js`を実行すると、以下のような結果が得られます：

```markdown
# Uranus

**Uranus** is the seventh [planet](/wiki/Planet "Planet") from the Sun and is a gaseous cyan [ice giant](/wiki/Ice_giant "Ice giant").
```

## API

このパッケージは識別子をエクスポートしません。
デフォルトエクスポートは[`remarkStringify`][api-remark-stringify]です。

### `unified().use(remarkStringify[, options])`

マークダウンへのシリアライズ機能を追加します。

###### パラメータ

* `options` ([`Options`][api-options], オプション)
  — 設定

###### 戻り値

なし（`undefined`）。

### `Options`

設定（TypeScript型）。

###### フィールド

* `bullet` (`'*'`、`'+'`、または`'-'`、デフォルト：`'*'`)
  — 順不同リストの項目の箇条書きに使用するマーカー
* `bulletOther` (`'*'`、`'+'`、または`'-'`、デフォルト：`bullet`が`'*'`の場合は`'-'`、それ以外は`'*'`)
  — 主な箇条書きが機能しない特定のケースで使用するマーカー；`bullet`と同じにはできません
* `bulletOrdered` (`'.'`または`')'`、デフォルト：`'.'`)
  — 順序付きリストの項目の箇条書きに使用するマーカー
* `closeAtx` (`boolean`、デフォルト：`false`)
  — ATX見出しの開始シーケンスと同じ数のナンバー記号（`#`）を末尾に追加します
* `emphasis` (`'*'`または`'_'`、デフォルト：`'*'`)
  — 強調に使用するマーカー
* `fence` (``'`'``または`'~'`、デフォルト：``'`'``)
  — フェンスド・コードに使用するマーカー
* `fences` (`boolean`、デフォルト：`true`)
  — 常にフェンスド・コードを使用します；`false`の場合、言語が定義されている場合、コードが空の場合、または空白行で始まるか終わる場合にフェンスド・コードを使用します
* `handlers` (`Handlers`、オプション)
  — 特定のノードを処理します；
  詳細は[`mdast-util-to-markdown`][mdast-util-to-markdown]を参照してください
* `incrementListMarker` (`boolean`、デフォルト：`true`)
  — 順序付きリスト項目のカウンターを増加させます
* `join` (`Array<Join>`、オプション)
  — ブロックの結合方法；
  詳細は[`mdast-util-to-markdown`][mdast-util-to-markdown]を参照してください
* `listItemIndent` (`'mixed'`、`'one'`、または`'tab'`、デフォルト：`'one'`)
  — リスト項目の内容のインデント方法；
  箇条書きのサイズにスペース1つを加えた(`'one'`)、タブストップ(`'tab'`)、または項目と親リストに応じて：`'mixed'`は項目とリストがタイトな場合は`'one'`を、そうでない場合は`'tab'`を使用します
* `quote` (`'"'`または`"'"`、デフォルト：`'"'`)
  — タイトルに使用するマーカー
* `resourceLink` (`boolean`、デフォルト：`false`)
  — 常にリソースリンク（`[text](url)`）を使用します；
  `false`の場合、可能な場合は自動リンク（`<https://example.com>`）を使用します
* `rule` (`'*'`、`'-'`、または`'_'`、デフォルト：`'*'`)
  — テーマ区切り線に使用するマーカー
* `ruleRepetition` (`number`、デフォルト：`3`、最小：`3`)
  — テーマ区切り線に使用するマーカーの数
* `ruleSpaces` (`boolean`、デフォルト：`false`)
  — テーマ区切り線のマーカー間にスペースを追加します
* `setext` (`boolean`、デフォルト：`false`)
  — 可能な場合はSetextタイプの見出しを使用します；
  `true`の場合、空でないランク1または2の見出しにSetextタイプの見出し（`heading\n=======`）を使用します
* `strong` (`'*'`または`'_'`、デフォルト：`'*'`)
  — 強調に使用するマーカー
* `tightDefinitions` (`boolean`、デフォルト：`false`)
  — 定義を空白行なしで結合します
* `unsafe` (`Array<Unsafe>`、オプション)
  — 文字が出現できない場合を定義するスキーマ；
  詳細は[`mdast-util-to-markdown`][mdast-util-to-markdown]を参照してください

<!-- 注：`extensions`は意図的にサポート/文書化されていません。 -->

## 構文

マークダウンはCommonMarkに従ってシリアライズされますが、ほとんどのマークダウンパーサーで動作する方法でフォーマットするように注意が払われています。
他のプラグインは構文拡張機能のサポートを追加できます。

## 構文木

remarkで使用される構文木は[mdast][]です。

## 型

このパッケージは[TypeScript][]で完全に型付けされています。
追加の型[`Options`][api-options]をエクスポートします。

また、`unified`に`Settings`を登録します。
`.data('settings', …)`でオプションを渡す場合は、このパッケージをどこかで型にインポートしてください。これにより、フィールドが登録されます。

```js
/// <reference types="remark-stringify" />

import {unified} from 'unified'

// @ts-expect-error: `thisDoesNotExist`は有効なオプションではありません。
unified().data('settings', {thisDoesNotExist: false})
```

## 互換性

unified collectiveによって維持されているプロジェクトは、メンテナンスされているバージョンのNode.jsと互換性があります。

新しいメジャーリリースをカットする際、メンテナンスされていないバージョンのNode.jsのサポートを削除します。
これは、現在のリリースライン、`remark-stringify@^11`を、Node.js 16と互換性を保つよう努めていることを意味します。

## セキュリティ

`remark-stringify`の使用は安全です。

remarkプラグインの使用は攻撃を受ける可能性があります。
各プラグインとその使用に関連するリスクを慎重に評価してください。

レポートの提出方法については、[セキュリティポリシー][security]を参照してください。

## 貢献

[`remarkjs/.github`][health]の[`contributing.md`][contributing]で、始め方を確認してください。
[`support.md`][support]でヘルプを得る方法を確認してください。
[Discussions][chat]に参加して、コミュニティや貢献者とチャットしてください。

このプロジェクトには[行動規範][coc]があります。
このリポジトリ、組織、またはコミュニティと対話することで、その条件に従うことに同意したものとみなされます。

## スポンサー

この取り組みを支援し、[OpenCollective][collective]でスポンサーになることで還元してください！

<table>
<tr valign="middle">
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://vercel.com">Vercel</a><br><br>
  <a href="https://vercel.com"><img src="https://avatars1.githubusercontent.com/u/14985020?s=256&v=4" width="128"></a>
</td>
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://motif.land">Motif</a><br><br>
  <a href="https://motif.land"><img src="https://avatars1.githubusercontent.com/u/74457950?s=256&v=4" width="128"></a>
</td>
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://www.hashicorp.com">HashiCorp</a><br><br>
  <a href="https://www.hashicorp.com"><img src="https://avatars1.githubusercontent.com/u/761456?s=256&v=4" width="128"></a>
</td>
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://www.gitbook.com">GitBook</a><br><br>
  <a href="https://www.gitbook.com"><img src="https://avatars1.githubusercontent.com/u/7111340?s=256&v=4" width="128"></a>
</td>
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://www.gatsbyjs.org">Gatsby</a><br><br>
  <a href="https://www.gatsbyjs.org"><img src="https://avatars1.githubusercontent.com/u/12551863?s=256&v=4" width="128"></a>
</td>
</tr>
<tr valign="middle">
</tr>
<tr valign="middle">
<td width="20%" align="center" rowspan="2" colspan="2">
  <a href="https://www.netlify.com">Netlify</a><br><br>
  <!--OC has a sharper image-->
  <a href="https://www.netlify.com"><img src="https://images.opencollective.com/netlify/4087de2/logo/256.png" width="128"></a>
</td>
<td width="10%" align="center">
  <a href="https://www.coinbase.com">Coinbase</a><br><br>
  <a href="https://www.coinbase.com"><img src="https://avatars1.githubusercontent.com/u/1885080?s=256&v=4" width="64"></a>
</td>
<td width="10%" align="center">
  <a href="https://themeisle.com">ThemeIsle</a><br><br>
  <a href="https://themeisle.com"><img src="https://avatars1.githubusercontent.com/u/58979018?s=128&v=4" width="64"></a>
</td>
<td width="10%" align="center">
  <a href="https://expo.io">Expo</a><br><br>
  <a href="https://expo.io"><img src="https://avatars1.githubusercontent.com/u/12504344?s=128&v=4" width="64"></a>
</td>
<td width="10%" align="center">
  <a href="https://boostnote.io">Boost Note</a><br><br>
  <a href="https://boostnote.io"><img src="https://images.opencollective.com/boosthub/6318083/logo/128.png" width="64"></a>
</td>
<td width="10%" align="center">
  <a href="https://markdown.space">Markdown Space</a><br><br>
  <a href="https://markdown.space"><img src="https://images.opencollective.com/markdown-space/e1038ed/logo/128.png" width="64"></a>
</td>
<td width="10%" align="center">
  <a href="https://www.holloway.com">Holloway</a><br><br>
  <a href="https://www.holloway.com"><img src="https://avatars1.githubusercontent.com/u/35904294?s=128&v=4" width="64"></a>
</td>
<td width="10%"></td>
<td width="10%"></td>
</tr>
<tr valign="middle">
<td width="100%" align="center" colspan="8">
  <br>
  <a href="https://opencollective.com/unified"><strong>あなたも？</strong></a>
  <br><br>
</td>
</tr>
</table>

## ライセンス

[MIT][license] © [Titus Wormer][author]

<!-- 定義 -->

[build-badge]: https://github.com/remarkjs/remark/workflows/main/badge.svg

[build]: https://github.com/remarkjs/remark/actions

[coverage-badge]: https://img.shields.io/codecov/c/github/remarkjs/remark.svg

[coverage]: https://codecov.io/github/remarkjs/remark

[downloads-badge]: https://img.shields.io/npm/dm/remark-stringify.svg

[downloads]: https://www.npmjs.com/package/remark-stringify

[size-badge]: https://img.shields.io/bundlejs/size/remark-stringify

[size]: https://bundlejs.com/?q=remark-stringify

[sponsors-badge]: https://opencollective.com/unified/sponsors/badge.svg

[backers-badge]: https://opencollective.com/unified/backers/badge.svg

[collective]: https://opencollective.com/unified

[chat-badge]: https://img.shields.io/badge/chat-discussions-success.svg

[chat]: https://github.com/remarkjs/remark/discussions

[security]: https://github.com/remarkjs/.github/blob/main/security.md

[health]: https://github.com/remarkjs/.github

[contributing]: https://github.com/remarkjs/.github/blob/main/contributing.md

[support]: https://github.com/remarkjs/.github/blob/main/support.md

[coc]: https://github.com/remarkjs/.github/blob/main/code-of-conduct.md

[license]: https://github.com/remarkjs/remark/blob/main/license

[author]: https://wooorm.com

[npm]: https://docs.npmjs.com/cli/install

[esm]: https://gist.github.com/sindresorhus/a39789f98801d908bbc7ff3ecc99d99c

[esmsh]: https://esm.sh

[mdast]: https://github.com/syntax-tree/mdast

[mdast-util-to-markdown]: https://github.com/syntax-tree/mdast-util-to-markdown

[remark]: https://github.com/remarkjs/remark

[remark-core]: ../remark/

[remark-directive]: https://github.com/remarkjs/remark-directive

[remark-frontmatter]: https://github.com/remarkjs/remark-frontmatter

[remark-gfm]: https://github.com/remarkjs/remark-gfm

[remark-math]: https://github.com/remarkjs/remark-math

[remark-mdx]: https://github.com/mdx-js/mdx/tree/main/packages/remark-mdx

[remark-parse]: ../remark-parse/

[remark-plugin]: https://github.com/remarkjs/remark#plugin

[typescript]: https://www.typescriptlang.org

[unified]: https://github.com/unifiedjs/unified

[api-options]: #options

[api-remark-stringify]: #unifieduseremarkstringify-options