# remark

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

マークダウンからのパース及びマークダウンへのシリアライズをサポートする**[unified][]**プロセッサー。

## 目次

* [これは何ですか？](#これは何ですか)
* [いつ使うべきですか？](#いつ使うべきですか)
* [インストール](#インストール)
* [使用方法](#使用方法)
* [API](#api)
  * [`remark()`](#remark-1)
* [例](#例)
  * [例：マークダウンのチェック](#例マークダウンのチェック)
  * [例：`remark-stringify`にオプションを渡す](#例remark-stringifyにオプションを渡す)
* [構文](#構文)
* [構文木](#構文木)
* [型](#型)
* [互換性](#互換性)
* [セキュリティ](#セキュリティ)
* [貢献](#貢献)
* [スポンサー](#スポンサー)
* [ライセンス](#ライセンス)

## これは何ですか？

このパッケージは、[`remark-parse`][remark-parse]と[`remark-stringify`][remark-stringify]を使用してunifiedとともに、入力としてマークダウンを解析し、出力としてマークダウンをシリアライズするサポートを持つ[unified][]プロセッサーです。

remarkエコシステムについての情報は[モノレポのREADME][remark]を参照してください。

## いつ使うべきですか？

unifiedを使用し、入力としてマークダウンを持ち、出力としてマークダウンを望む場合にこのパッケージを使用できます。
このパッケージは`unified().use(remarkParse).use(remarkStringify)`のショートカットです。
入力がマークダウンでない場合（つまり`remark-parse`が不要な場合）または出力がマークダウンでない場合（`remark-stringify`が不要な場合）、`unified`を直接使用することをお勧めします。

プロジェクト内のマークダウンファイルをコマンドラインで検査およびフォーマットしたい場合は、[`remark-cli`][remark-cli]を使用できます。

## インストール

このパッケージは[ESMのみ][esm]です。
Node.js（バージョン16+）では、[npm][]でインストールしてください：

```sh
npm install remark
```

Denoでは[`esm.sh`][esmsh]を使用：

```js
import {remark} from 'https://esm.sh/remark@15'
```

ブラウザでは[`esm.sh`][esmsh]を使用：

```html
<script type="module">
  import {remark} from 'https://esm.sh/remark@15?bundle'
</script>
```

## 使用方法

以下のような`example.js`モジュールがあるとします：

```js
import {remark} from 'remark'
import remarkToc from 'remark-toc'

const doc = `
# Pluto

Pluto is a dwarf planet in the Kuiper belt.

## Contents

## History

### Discovery

In the 1840s, Urbain Le Verrier used Newtonian mechanics to predict the position of…

### Name and symbol

The name Pluto is for the Roman god of the underworld, from a Greek epithet for Hades…

### Planet X disproved

Once Pluto was found, its faintness and lack of a viewable disc cast doubt…

## Orbit

Pluto's orbital period is about 248 years…
`

const file = await remark()
  .use(remarkToc, {heading: 'contents', tight: true})
  .process(doc)

console.error(String(file))
```

`node example.js`で実行すると、以下のような結果が得られます：

```markdown
# Pluto

Pluto is a dwarf planet in the Kuiper belt.

## Contents

* [History](#history)
  * [Discovery](#discovery)
  * [Name and symbol](#name-and-symbol)
  * [Planet X disproved](#planet-x-disproved)
* [Orbit](#orbit)

## History

### Discovery

In the 1840s, Urbain Le Verrier used Newtonian mechanics to predict the position of…

### Name and symbol

The name Pluto is for the Roman god of the underworld, from a Greek epithet for Hades…

### Planet X disproved

Once Pluto was found, its faintness and lack of a viewable disc cast doubt…

## Orbit

Pluto's orbital period is about 248 years…
```

## API

このパッケージは識別子[`remark`][api-remark]をエクスポートします。
デフォルトエクスポートはありません。

### `remark()`

[`remark-parse`][remark-parse]と[`remark-stringify`][remark-stringify]を既に使用している新しいunifiedプロセッサーを作成します。

`use`でさらにプラグインを追加できます。
詳細については[`unified`][unified]を参照してください。

## 例

### 例：マークダウンのチェック

以下の例では、マークダウンのコードスタイルが一貫しており、いくつかのベストプラクティスに従っているかをチェックします：

```js
import {remark} from 'remark'
import remarkPresetLintConsistent from 'remark-preset-lint-consistent'
import remarkPresetLintRecommended from 'remark-preset-lint-recommended'
import {reporter} from 'vfile-reporter'

const file = await remark()
  .use(remarkPresetLintConsistent)
  .use(remarkPresetLintRecommended)
  .process('1) Hello, _Jupiter_ and *Neptune*!')

console.error(reporter(file))
```

結果：

```txt
          warning Missing newline character at end of file final-newline             remark-lint
1:1-1:35  warning Marker style should be `.`               ordered-list-marker-style remark-lint
1:4       warning Incorrect list-item indent: add 1 space  list-item-indent          remark-lint
1:25-1:34 warning Emphasis should use `_` as a marker      emphasis-marker           remark-lint

⚠ 4 warnings
```

### 例：`remark-stringify`にオプションを渡す

`remark-stringify`を手動で使用する場合、`use`にオプションを渡すことができます。
`remark-stringify`は既に`remark`で使用されているため、それは不可能です。
代わりに、`remark-stringify`のオプションを定義するには、`data`にオプションを渡すことができます：

```js
import {remark} from 'remark'

const doc = `
# Moons of Neptune

1. Naiad
2. Thalassa
3. Despine
4. …
`

const file = await remark()
  .data('settings', {
    bulletOrdered: ')',
    incrementListMarker: false,
    setext: true
  })
  .process(doc)

console.log(String(file))
```

結果：

```markdown
Moons of Neptune
================

1) Naiad
1) Thalassa
1) Despine
1) …
```

## 構文

マークダウンはCommonMarkに従って解析およびシリアライズされます。
他のプラグインは構文拡張のサポートを追加できます。

## 構文木

remarkで使用される構文木は[mdast][]です。

## 型

このパッケージは[TypeScript][]で完全に型付けされています。
追加でエクスポートされる型はありません。

また、`unified`に`Settings`を登録します。
`.data('settings', …)`でオプションを渡す場合は、このパッケージをどこかで型にインポートしてください。これにより、フィールドが登録されます。

```js
/// <reference types="remark" />

import {unified} from 'unified'

// @ts-expect-error: `thisDoesNotExist` は有効なオプションではありません。
unified().data('settings', {thisDoesNotExist: false})
```

## 互換性

unified collectiveによって維持されているプロジェクトは、メンテナンスされているバージョンのNode.jsと互換性があります。

新しいメジャーリリースをカットする際、メンテナンスされていないバージョンのNode.jsのサポートを削除します。
つまり、現在のリリースライン、`remark@^15`は、Node.js 16との互換性を維持しようとしています。

## セキュリティ

マークダウンはHTMLに変換できるため、HTMLの不適切な使用は[クロスサイトスクリプティング（XSS）][xss]攻撃を引き起こす可能性があるため、remarkの使用は安全でない場合があります。
HTMLに移行する場合、remarkを**[rehype][]**と組み合わせる必要があり、その場合は[`rehype-sanitize`][rehype-sanitize]を使用する必要があります。

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

[downloads-badge]: https://img.shields.io/npm/dm/remark.svg

[downloads]: https://www.npmjs.com/package/remark

[size-badge]: https://img.shields.io/bundlejs/size/remark

[size]: https://bundlejs.com/?q=remark

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

[rehype]: https://github.com/rehypejs/rehype

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[remark]: https://github.com/remarkjs/remark

[remark-parse]: ../remark-parse

[remark-stringify]: ../remark-stringify

[remark-cli]: ../remark-cli

[typescript]: https://www.typescriptlang.org

[unified]: https://github.com/unifiedjs/unified

[xss]: https://en.wikipedia.org/wiki/Cross-site_scripting

[api-remark]: #remark-1