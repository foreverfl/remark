# remark-parse

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

マークダウンからのパース機能を追加する**[remark][]**プラグイン。

## 目次

* [これは何ですか？](#これは何ですか)
* [いつ使うべきですか？](#いつ使うべきですか)
* [インストール](#インストール)
* [使用方法](#使用方法)
* [API](#api)
  * [`unified().use(remarkParse)`](#unifieduseremarkparse)
* [例](#例)
  * [例：GFMとフロントマターのサポート](#例gfmとフロントマターのサポート)
  * [例：マークダウンをmanページに変換する](#例マークダウンをmanページに変換する)
* [構文](#構文)
* [構文木](#構文木)
* [型](#型)
* [互換性](#互換性)
* [セキュリティ](#セキュリティ)
* [貢献](#貢献)
* [スポンサー](#スポンサー)
* [ライセンス](#ライセンス)

## これは何ですか？

このパッケージは、マークダウンを入力として受け取り、構文木に変換する方法を定義する[unified][]（[remark][]）プラグインです。

remarkエコシステムについての情報は[モノレポのREADME][remark]を参照してください。

## いつ使うべきですか？

このプラグインは、unifiedにマークダウンのパース機能を追加します。
マークダウンのシリアライズも必要な場合は、代わりに[`remark`][remark-core]を使用できます。これはunified、このプラグイン、および[`remark-stringify`][remark-stringify]を組み合わせたものです。

マークダウンをHTMLに変換するだけ（おそらくいくつかの拡張機能を使用）の場合は、代わりに[`micromark`][micromark]をお勧めします。
プラグインを使用せず、構文木にアクセスしたい場合は、[`mdast-util-from-markdown`][mdast-util-from-markdown]を直接使用できます。
remarkは、これらの内部を抽象化することで、コンテンツの変換を容易にすることに焦点を当てています。

このプラグインを他のプラグインと組み合わせて、構文拡張機能を追加できます。
深く統合されている注目すべき例として、
[`remark-gfm`][remark-gfm]、
[`remark-mdx`][remark-mdx]、
[`remark-frontmatter`][remark-frontmatter]、
[`remark-math`][remark-math]、
[`remark-directive`][remark-directive]があります。
また、`remark-parse`の後に他の[remarkプラグイン][remark-plugin]を使用することもできます。

## インストール

このパッケージは[ESMのみ][esm]です。
Node.js（バージョン16+）では、[npm][]でインストールしてください：

```sh
npm install remark-parse
```

Denoでは[`esm.sh`][esmsh]を使用：

```js
import remarkParse from 'https://esm.sh/remark-parse@11'
```

ブラウザでは[`esm.sh`][esmsh]を使用：

```html
<script type="module">
  import remarkParse from 'https://esm.sh/remark-parse@11?bundle'
</script>
```

## 使用方法

以下のような`example.js`モジュールがあるとします：

```js
import rehypeStringify from 'rehype-stringify'
import remarkGfm from 'remark-gfm'
import remarkParse from 'remark-parse'
import remarkRehype from 'remark-rehype'
import {unified} from 'unified'

const doc = `
# Mercury

**Mercury** is the first planet from the [Sun](https://en.wikipedia.org/wiki/Sun)
and the smallest planet in the Solar System.
`

const file = await unified()
  .use(remarkParse)
  .use(remarkGfm)
  .use(remarkRehype)
  .use(rehypeStringify)
  .process(doc)

console.log(String(file))
```

`node example.js`を実行すると、以下のような結果が得られます：

```html
<h1>Mercury</h1>
<p><strong>Mercury</strong> is the first planet from the <a href="https://en.wikipedia.org/wiki/Sun">Sun</a>
and the smallest planet in the Solar System.</p>
```

## API

このパッケージは識別子をエクスポートしません。
デフォルトエクスポートは[`remarkParse`][api-remark-parse]です。

### `unified().use(remarkParse)`

マークダウンからのパース機能を追加します。

###### パラメータ

パラメータはありません。

###### 戻り値

なし（`undefined`）。

## 例

### 例：GFMとフロントマターのサポート

デフォルトではCommonMarkをサポートしています。
非標準のマークダウン拡張機能はプラグインで有効にできます。

この例では、GFM機能（自動リンクリテラル、脚注、取り消し線、テーブル、タスクリスト）とフロントマター（YAML）をサポートする方法を示しています：

```js
import rehypeStringify from 'rehype-stringify'
import remarkFrontmatter from 'remark-frontmatter'
import remarkGfm from 'remark-gfm'
import remarkParse from 'remark-parse'
import remarkRehype from 'remark-rehype'
import {unified} from 'unified'

const doc = `---
layout: solar-system
---

# Hi ~~Mars~~Venus!
`

const file = await unified()
  .use(remarkParse)
  .use(remarkFrontmatter)
  .use(remarkGfm)
  .use(remarkRehype)
  .use(rehypeStringify)
  .process(doc)

console.log(String(file))
```

結果：

```html
<h1>Hi <del>Mars</del>Venus!</h1>
```

### 例：マークダウンをmanページに変換する

manページ（マニュアルページの略）は、CLIを文書化する方法です（例：ターミナルで`man git-log`と入力します）。
これらは、roffと呼ばれる古いマークアップ形式を使用しています。
remarkプラグインの[`remark-man`][remark-man]は、roffとしてシリアライズできます。

この例では、unifiedと`remark-parse`および`remark-man`を使用して、マークダウンをmanページに変換する方法を示しています：

```js
import remarkMan from 'remark-man'
import remarkParse from 'remark-parse'
import {unified} from 'unified'

const doc = `
# titan(7) -- largest moon of saturn

Titan is the largest moon…
`

const file = await unified().use(remarkParse).use(remarkMan).process(doc)

console.log(String(file))
```

結果：

```roff
.TH "TITAN" "7" "September 2023" "" ""
.SH "NAME"
\fBtitan\fR - largest moon of saturn
.P
Titan is the largest moon…
```

## 構文

マークダウンはCommonMarkに従って解析されます。
他のプラグインは構文拡張機能のサポートを追加できます。
マークダウンの拡張に興味がある場合、[micromarkのREADMEに詳細な情報があります][micromark-extend]。

## 構文木

remarkで使用される構文木は[mdast][]です。

## 型

このパッケージは[TypeScript][]で完全に型付けされています。
追加の型`Options`（現在は空）をエクスポートします。

## 互換性

unified collectiveによって維持されているプロジェクトは、メンテナンスされているバージョンのNode.jsと互換性があります。

新しいメジャーリリースをカットする際、メンテナンスされていないバージョンのNode.jsのサポートを削除します。
これは、現在のリリースライン、`remark-parse@^11`を、Node.js 16と互換性を保つよう努めていることを意味します。

## セキュリティ

マークダウンはHTMLに変換できるため、HTMLの不適切な使用は[クロスサイトスクリプティング（XSS）][xss]攻撃を引き起こす可能性があるため、remarkの使用は安全でない場合があります。
HTMLに移行する場合、remarkを**[rehype][]**と組み合わせることになり、その場合は[`rehype-sanitize`][rehype-sanitize]を使用する必要があります。

remarkプラグインの使用も他の攻撃を引き起こす可能性があります。
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

[downloads-badge]: https://img.shields.io/npm/dm/remark-parse.svg

[downloads]: https://www.npmjs.com/package/remark-parse

[size-badge]: https://img.shields.io/bundlejs/size/remark-parse

[size]: https://bundlejs.com/?q=remark-parse

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

[mdast-util-from-markdown]: https://github.com/syntax-tree/mdast-util-from-markdown

[micromark]: https://github.com/micromark/micromark

[micromark-extend]: https://github.com/micromark/micromark#extensions

[rehype]: https://github.com/rehypejs/rehype

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[remark]: https://github.com/remarkjs/remark

[remark-core]: ../remark/

[remark-directive]: https://github.com/remarkjs/remark-directive

[remark-frontmatter]: https://github.com/remarkjs/remark-frontmatter

[remark-gfm]: https://github.com/remarkjs/remark-gfm

[remark-mdx]: https://github.com/mdx-js/mdx/tree/main/packages/remark-mdx

[remark-man]: https://github.com/remarkjs/remark-man

[remark-math]: https://github.com/remarkjs/remark-math

[remark-plugin]: https://github.com/remarkjs/remark#plugin

[remark-stringify]: ../remark-stringify/

[typescript]: https://www.typescriptlang.org

[unified]: https://github.com/unifiedjs/unified

[xss]: https://en.wikipedia.org/wiki/Cross-site_scripting

[api-remark-parse]: #unifieduseremarkparse