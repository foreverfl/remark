# ![remark][logo]

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

**remark**は、プラグインを使用してマークダウンを変換するツールです。
これらのプラグインはマークアップを検査し、変更することができます。
remarkはサーバー、クライアント、CLI、denoなどで使用できます。

## 主な特徴

* [x] **[準拠][syntax]**
  — CommonMarkに100%、プラグインを使用してGFMやMDXに100%
* [x] **[AST][syntax-tree]**
  — コンテンツの検査と変更が容易
* [x] **[人気][popular]**
  — 世界で最も人気のあるマークダウンパーサー
* [x] **[プラグイン][plugins]**
  — 150以上のプラグインから選択可能

## はじめに

remarkは、マークダウンを構造化データ、特にAST（抽象構文木）として扱うプラグインのエコシステムです。
ASTにより、プログラムがマークダウンを扱うのが容易になります。
そのようなプログラムをプラグインと呼びます。
プラグインはツリーを検査し、変更します。
既存の多くのプラグインを使用することも、独自のプラグインを作成することもできます。

* マークダウンを学ぶには、この[チートシートとチュートリアル][cheat]をご覧ください
* 私たちについての詳細は、[`unifiedjs.com`][site]をご覧ください
* 更新情報については、[Twitter][]をご覧ください
* 質問がある場合は、[support][]をご覧ください
* 貢献するには、以下の[貢献](#貢献)または[スポンサー](#スポンサー)をご覧ください

## 目次

* [これは何ですか？](#これは何ですか)
* [いつ使うべきですか？](#いつ使うべきですか)
* [プラグイン](#プラグイン)
* [例](#例)
  * [例：マークダウンをHTMLに変換する](#例マークダウンをhtmlに変換する)
  * [例：GFMとフロントマターのサポート](#例gfmとフロントマターのサポート)
  * [例：マークダウンのチェック](#例マークダウンのチェック)
  * [例：CLIでマークダウンのチェックとフォーマット](#例cliでマークダウンのチェックとフォーマット)
* [構文](#構文)
* [構文ツリー](#構文ツリー)
* [タイプ](#タイプ)
* [互換性](#互換性)
* [セキュリティ](#セキュリティ)
* [貢献](#貢献)
* [スポンサー](#スポンサー)
* [ライセンス](#ライセンス)

## これは何ですか？

このプロジェクトとプラグインを使用すると、次のようなマークダウンを：

```markdown
# Hello, *Mercury*!
```

...以下のようなHTMLに変換できます：

```html
<h1>Hello, <em>Mercury</em>!</h1>
```

<details><summary>コード例を表示</summary>

```js
import rehypeStringify from 'rehype-stringify'
import remarkParse from 'remark-parse'
import remarkRehype from 'remark-rehype'
import {unified} from 'unified'

const file = await unified()
  .use(remarkParse)
  .use(remarkRehype)
  .use(rehypeStringify)
  .process('# Hello, *Mercury*!')

console.log(String(file)) // => '<h1>Hello, <em>Mercury</em>!</h1>'
```

</details>

別のプラグインを使用すると、次のようなマークダウンを：

```markdown
# Hi, Saturn!
```

...以下のようなマークダウンに変換できます：

```markdown
## Hi, Saturn!
```

<details><summary>コード例を表示</summary>

```js
/**
 * @import {Root} from 'mdast'
 */

import remarkParse from 'remark-parse'
import remarkStringify from 'remark-stringify'
import {unified} from 'unified'
import {visit} from 'unist-util-visit'

const file = await unified()
  .use(remarkParse)
  .use(myRemarkPluginToIncreaseHeadings)
  .use(remarkStringify)
  .process('# Hi, Saturn!')

console.log(String(file)) // => '## Hi, Saturn!'

function myRemarkPluginToIncreaseHeadings() {
  /**
   * @param {Root} tree
   */
  return function (tree) {
    visit(tree, function (node) {
      if (node.type === 'heading') {
        node.depth++
      }
    })
  }
}
```

</details>

remarkは多くの異なる目的に使用できます。
**[unified][]**は、ASTを使用してコンテンツを変換するコアプロジェクトです。
**remark**は、unifiedにマークダウンのサポートを追加します。
**[mdast][]**は、remarkが使用するマークダウンASTです。

このGitHubリポジトリは、以下のパッケージを含むモノレポです：

* [`remark-parse`][remark-parse]
  — マークダウンを入力として受け取り、構文ツリー（mdast）に変換するプラグイン
* [`remark-stringify`][remark-stringify]
  — 構文ツリー（mdast）を受け取り、マークダウンを出力として変換するプラグイン
* [`remark`][remark-core]
  — `unified`、`remark-parse`、`remark-stringify`を組み合わせたもので、入力と出力の両方がマークダウンの場合に便利
* [`remark-cli`][remark-cli]
  — スクリプトでマークダウンを検査およびフォーマットするための`remark`周りのCLI

## いつ使うべきですか？

入力と望む出力に応じて、remarkの異なる部分を使用できます。
入力がマークダウンの場合は、`remark-parse`と`unified`を使用できます。
出力がマークダウンの場合は、`remark-stringify`と`unified`を使用できます。
入力と出力の両方がマークダウンの場合は、`remark`を単独で使用できます。
プロジェクト内のマークダウンファイルを検査およびフォーマットしたい場合は、`remark-cli`を使用できます。

マークダウンをHTMLに変換するだけの場合（おそらくいくつかの拡張機能を使用）、代わりに[`micromark`][micromark]をお勧めします。

プラグインを使用せず、構文ツリーを手動で扱いたい場合は、[`mdast-util-from-markdown`][mdast-util-from-markdown]と[`mdast-util-to-markdown`][mdast-util-to-markdown]を使用できます。

## プラグイン

remarkプラグインはマークダウンを扱います。
いくつかの人気のある例：

* [`remark-gfm`][remark-gfm]
  — GFM（GitHub Flavored Markdown）のサポートを追加
* [`remark-lint`][remark-lint]
  — マークダウンを検査し、不整合について警告
* [`remark-toc`][remark-toc]
  — 目次を生成
* [`remark-rehype`][remark-rehype]
  — マークダウンをHTMLに変換

これらのプラグインは、それぞれマークダウン構文の拡張、ツリーの検査、ツリーの変更、他の構文ツリーへの変換など、行うことと方法が大きく異なるため、例示的です。

既に存在する150以上のプラグインから選択できます。
プラグインを見つけるための3つの良い方法があります：

* [`awesome-remark`][awesome-remark]
  — 最も素晴らしいプロジェクトの選択
* [プラグインのリスト][list-of-plugins]
  — すべてのプラグインのリスト
* [`remark-plugin` トピック][topic]
  — GitHubでタグ付けされたリポジトリ

一部のプラグインは`@remarkjs`組織内で私たちが維持していますが、他のプラグインは他の場所で維持されています。
誰でもremarkプラグインを作成できるので、プロジェクトに依存関係を含めるかどうかを選択する際には、常にremarkプラグインの品質も慎重に評価してください。

## 例

### 例：マークダウンをHTMLに変換する

remarkはマークダウンを中心としたエコシステムです。
HTMLのための別のエコシステムは[rehype][]です。
以下の例では、[`remark-rehype`][remark-rehype]を使用して両方のエコシステムを組み合わせ、マークダウンをHTMLに変換します：

```js
import rehypeSanitize from 'rehype-sanitize'
import rehypeStringify from 'rehype-stringify'
import remarkParse from 'remark-parse'
import remarkRehype from 'remark-rehype'
import {unified} from 'unified'

const file = await unified()
  .use(remarkParse)
  .use(remarkRehype)
  .use(rehypeSanitize)
  .use(rehypeStringify)
  .process('# Hello, Neptune!')

console.log(String(file))
```

結果：

```html
<h1>Hello, Neptune!</h1>
```

### 例：GFMとフロントマターのサポート

remarkはデフォルトでCommonMarkをサポートしています。
非標準のマークダウン拡張機能はプラグインで有効にできます。
以下の例では、GFM（自動リンクリテラル、脚注、取り消し線、テーブル、タスクリスト）とフロントマター（YAML）のサポートを追加します：

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

### 例：マークダウンのチェック

以下の例では、マークダウンのコードスタイルが一貫しており、推奨されるベストプラクティスに従っているかをチェックします：

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

### 例：CLIでマークダウンのチェックとフォーマット

以下の例では、`remark-cli`を使用してマークダウンをチェックおよびフォーマットします。これはターミナルで使用できるremarkのCLI（コマンドラインインターフェース）です。
この例では、Node.jsパッケージ内にいることを前提としています。

まず、CLIとプラグインをインストールします：

```sh
npm install remark-cli remark-preset-lint-consistent remark-preset-lint-recommended remark-toc --save-dev
```

...次に、`package.json`にnpmスクリプトを追加します：

```js
  /* … */
  "scripts": {
    /* … */
    "format": "remark . --output",
    /* … */
  },
  /* … */
```

> 💡 **ヒント**: `format`スクリプトにESLintなども追加してください。

上記の変更により、`format`スクリプトが追加されます。これは`npm run format`で実行できます。
すべてのマークダウンファイル（`.`）で実行し、それらを書き換えます（`--output`）。
CLIの詳細については、`./node_modules/.bin/remark --help`を実行してください。

次に、remarkを設定するために`package.json`に`remarkConfig`を追加します：

```js
  /* … */
  "remarkConfig": {
    "settings": {
      "bullet": "*", // リストアイテムの箇条書きに`*`を使用（デフォルト）
      // その他のオプションについては<https://github.com/remarkjs/remark/tree/main/packages/remark-stringify>をご覧ください。
    },
    "plugins": [
      "remark-preset-lint-consistent", // マークダウンの一貫性をチェックします。
      "remark-preset-lint-recommended", // いくつかの推奨ルール。
      [
        // `## Contents`に目次を生成します
        "remark-toc",
        {
          "heading": "contents"
        }
      ]
    ]
  },
  /* … */
```

> 👉 **注意**: 上記の例をコピー/ペーストする際は、コメントを削除する必要があります。`package.json`ファイルではコメントがサポートされていないためです。

最後に、npmスクリプトを実行してプロジェクト内のマークダウンファイルをチェックおよびフォーマットできます：

```sh
npm run format
```

## 構文

マークダウンはCommonMarkに従ってパースおよびシリアライズされます。
他のプラグインは構文拡張のサポートを追加できます。

パースには[`micromark`][micromark]を使用しています。
マークダウン、CommonMark、および拡張機能の詳細については、そのドキュメントをご覧ください。

## 構文ツリー

remarkで使用される構文ツリーは[mdast][]です。
これはマークダウンの構造をJSONオブジェクトとして表現します。

このマークダウン：

```markdown
## Hello *Pluto*!
```

...は以下のようなツリーを生成します（簡潔にするため位置情報は削除しています）：

```js
{
  type: 'heading',
  depth: 2,
  children: [
    {type: 'text', value: 'Hello '},
    {type: 'emphasis', children: [{type: 'text', value: 'Pluto'}]}
    {type: 'text', value: '!'}
  ]
}
```

## タイプ

remark組織およびunified集団全体は、[TypeScript][]で完全に型付けされています。
mdastの型は[`@types/mdast`][types-mdast]で利用可能です。

TypeScriptが機能するためには、プラグインに型を付けることが重要です。
例えば：

```js
/**
 * @import {Root} from 'mdast'
 * @import {VFile} from 'vfile'
 */

/**
 * @typedef Options
 *   設定。
 * @property {boolean | null | undefined} [someField]
 *   いくつかのオプション（オプション）。
 */

/**
 * 私のプラグイン。
 *
 * @param {Options | null | undefined} [options]
 *   設定（オプション）。
 * @returns
 *   変換。
 */
export function myRemarkPluginAcceptingOptions(options) {
  /**
   * 変換。
   *
   * @param {Root} tree
   *   ツリー。
   * @param {VFile} file
   *   ファイル
   * @returns {undefined}
   *   何も返さない。
   */
  return function (tree, file) {
    // 何かを行う。
  }
}
```

## 互換性

unified集団によって維持されているプロジェクトは、メンテナンスされているNode.jsのバージョンと互換性があります。

新しいメジャーリリースを行う際、メンテナンスされていないバージョンのNode.jsのサポートを終了します。
これは、現在のリリースラインをNode.js 16と互換性を保つよう努めていることを意味します。

## セキュリティ

マークダウンはHTMLに変換でき、HTMLの不適切な使用は[クロスサイトスクリプティング（XSS）][xss]攻撃に対して脆弱になる可能性があるため、remarkの使用は安全でない可能性があります。
HTMLに変換する場合は、remarkと**[rehype][]**を組み合わせて使用し、[`rehype-sanitize`][rehype-sanitize]を使用する必要があります。

remarkプラグインの使用は、他の攻撃にもさらされる可能性があります。
各プラグインとその使用に伴うリスクを慎重に評価してください。

レポートの提出方法については、[セキュリティポリシー][security]をご覧ください。

## 貢献

始め方については、[`remarkjs/.github`][health]の[`contributing.md`][contributing]をご覧ください。
ヘルプを得る方法については[`support.md`][support]をご覧ください。
[Discussions][chat]に参加して、コミュニティや貢献者とチャットしてください。

このプロジェクトには[行動規範][coc]があります。
このリポジトリ、組織、またはコミュニティとやり取りすることで、あなたはその条件に従うことに同意したことになります。

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

[MIT](license) © [Titus Wormer](https://wooorm.com)

<!-- 定義 -->

[logo]: https://raw.githubusercontent.com/remarkjs/remark/1f338e72/logo.svg?sanitize=true

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

[typescript]: https://www.typescriptlang.org

[cheat]: https://commonmark.org/help/

[twitter]: https://twitter.com/unifiedjs

[site]: https://unifiedjs.com

[topic]: https://github.com/topics/remark-plugin

[popular]: https://www.npmtrends.com/remark-parse-vs-marked-vs-micromark-vs-markdown-it

[types-mdast]: https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/mdast

[mdast]: https://github.com/syntax-tree/mdast

[mdast-util-from-markdown]: https://github.com/syntax-tree/mdast-util-from-markdown

[mdast-util-to-markdown]: https://github.com/syntax-tree/mdast-util-to-markdown

[micromark]: https://github.com/micromark/micromark

[rehype]: https://github.com/rehypejs/rehype

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[remark-cli]: packages/remark-cli/

[remark-core]: packages/remark/

[remark-gfm]: https://github.com/remarkjs/remark-gfm

[remark-lint]: https://github.com/remarkjs/remark-lint

[remark-parse]: packages/remark-parse/

[remark-rehype]: https://github.com/remarkjs/remark-rehype

[remark-stringify]: packages/remark-stringify/

[remark-toc]: https://github.com/remarkjs/remark-toc

[awesome-remark]: https://github.com/remarkjs/awesome-remark

[unified]: https://github.com/unifiedjs/unified

[xss]: https://en.wikipedia.org/wiki/Cross-site_scripting

[list-of-plugins]: doc/plugins.md#list-of-plugins

[syntax]: #構文

[syntax-tree]: #構文ツリー

[plugins]: #プラグイン

[contribute]: #貢献

[sponsor]: #スポンサー