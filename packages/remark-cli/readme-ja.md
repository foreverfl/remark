# remark-cli

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

**[remark][]**を使用してマークダウンファイルを検査および変更するためのコマンドラインインターフェース。

## 目次

* [これは何ですか？](#これは何ですか)
* [いつ使うべきですか？](#いつ使うべきですか)
* [インストール](#インストール)
* [使用方法](#使用方法)
* [CLI](#cli)
* [例](#例)
  * [例：CLIでマークダウンのチェックとフォーマット](#例cliでマークダウンのチェックとフォーマット)
  * [例：設定ファイル（JSON、YAML、JS）](#例設定ファイルjsonyamljs)
* [互換性](#互換性)
* [セキュリティ](#セキュリティ)
* [貢献](#貢献)
* [スポンサー](#スポンサー)
* [ライセンス](#ライセンス)

## これは何ですか？

このパッケージは、ターミナルやnpmスクリプトなどでマークダウンファイルを検査および変更するために使用できるコマンドラインインターフェース（CLI）です。
このCLIは、マークダウンを構造化データ、特にAST（抽象構文木）として扱うプラグインのエコシステムであるremarkを中心に構築されています。
既存の150以上のプラグインから選択するか、独自のプラグインを作成することができます。

remarkエコシステムについての情報は、[モノレポのREADME][remark]を参照してください。

## いつ使うべきですか？

プロジェクト内のマークダウンファイルをコマンドラインから操作したい場合に、このパッケージを使用できます。
`remark-cli`には多くのオプションがあり、多数のプラグインと組み合わせることができるため、望む操作を行うことができるはずです。
それでも不可能な場合は、スクリプト内で[`remark`][remark-core]自体を手動で使用することができます。

## インストール

このパッケージは[ESMのみ][esm]です。
Node.js（バージョン16以上）では、[npm][]でインストールしてください：

```sh
npm install remark-cli
```

## 使用方法

[`remark-toc`][remark-toc]を使用して`readme.md`に目次を追加します：

```sh
remark readme.md --output --use remark-toc
```

[`remark-preset-lint-markdown-style-guide`][markdown-style-guide]を使用して、マークダウンスタイルガイドに従って現在のディレクトリ内のすべてのマークダウンファイルをリントします。

```sh
remark . --use remark-preset-lint-markdown-style-guide
```

## CLI

`remark-cli`のインターフェースは、そのヘルプページ（`remark --help`）で以下のように説明されています：

```txt
Usage: remark [options] [path | glob ...]

  CLI to process markdown with remark

Options:

      --[no-]color                        レポートでの色の使用を指定（デフォルトではオン）
      --[no-]config                       設定ファイルの検索（デフォルトではオン）
  -e  --ext <extensions>                  拡張子を指定
      --file-path <path>                  処理するパスを指定
  -f  --frail                             警告があれば1で終了
  -h  --help                              使用方法の情報を出力
      --[no-]ignore                       無視ファイルの検索（デフォルトではオン）
  -i  --ignore-path <path>                無視ファイルを指定
      --ignore-path-resolve-from cwd|dir  `ignore-path`のパターンをそのディレクトリまたはcwdから解決
      --ignore-pattern <globs>            無視パターンを指定
      --inspect                           フォーマットされた構文ツリーを出力
  -o  --output [path]                     出力場所を指定
  -q  --quiet                             警告とエラーのみを出力
  -r  --rc-path <path>                    設定ファイルを指定
      --report <reporter>                 レポーターを指定
  -s  --setting <settings>                設定を指定
  -S  --silent                            エラーのみを出力
      --silently-ignore                   無視されたファイルが与えられても失敗しない
      --[no-]stdout                       標準出力への書き込みを指定（デフォルトではオン）
  -t  --tree                              入力と出力を構文ツリーとして指定
      --tree-in                           入力を構文ツリーとして指定
      --tree-out                          構文ツリーを出力
  -u  --use <plugins>                     プラグインを使用
      --verbose                           メッセージの追加情報を報告
  -v  --version                           バージョン番号を出力
  -w  --watch                             変更を監視して再処理

Examples:

  # `input.md`を処理
  $ remark input.md -o output.md

  # パイプ
  $ remark < input.md > output.md

  # 適用可能なすべてのファイルを書き換え
  $ remark . -o
```

これらのオプションについての詳細な情報は、実際の作業を行う[`unified-args`][unified-args]で利用できます。
`remark-cli`は`unified-args`を以下のように事前設定しています：

* `remark-`プラグインを読み込む
* マークダウン拡張子（[`.md`、`.markdown`など][markdown-extensions]）を検索
* [`.remarkignore`ファイル][ignore-file]で見つかったパスを無視
* [`.remarkrc`、`.remarkrc.js`などのファイル][config-file]から設定を読み込む
* [`package.json`ファイルの`remarkConfig`フィールド][config-file]から設定を使用

## 例

### 例：CLIでマークダウンのチェックとフォーマット

この例では、`remark-cli`を使用してマークダウンをチェックおよびフォーマットします。
Node.jsパッケージ内にいることを想定しています。

CLIとプラグインをインストールします：

```sh
npm install remark-cli remark-preset-lint-consistent remark-preset-lint-recommended remark-toc --save-dev
```

...そして、`package.json`にnpmスクリプトを追加します：

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

上記の変更により、`npm run format`で実行できる`format`スクリプトが追加されます。
これはすべてのマークダウンファイル（`.`）でremarkを実行し、それらを書き直します（`--output`）。
CLIの詳細については、`./node_modules/.bin/remark --help`を実行してください。

次に、remarkを設定するために`package.json`に`remarkConfig`を追加します：

```js
  /* … */
  "remarkConfig": {
    "settings": {
      "bullet": "*", // リストアイテムの箇条書きに`*`を使用（デフォルト）
      // 詳細なオプションについては <https://github.com/remarkjs/remark/tree/main/packages/remark-stringify> を参照してください。
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

最後に、npmスクリプトを実行して、プロジェクト内のマークダウンファイルをチェックおよびフォーマットできます：

```sh
npm run format
```

### 例：設定ファイル（JSON、YAML、JS）

前の例では、`remark-cli`が`package.json`ファイル内で設定されていることを確認しました。
設定が比較的短い場合、`package.json`がある場合、そしてコメントが必要ない場合（JSONではコメントが許可されていません）、これは良い場所です。

別のファイルで異なる言語で設定を定義することもできます。
`package.json`の設定を参考に、`.remarkrc.js`に配置できるJavaScriptバージョンを以下に示します：

```js
import remarkPresetLintConsistent from 'remark-preset-lint-consistent'
import remarkPresetLintRecommended from 'remark-preset-lint-recommended'
import remarkToc from 'remark-toc'

const remarkConfig = {
  settings: {
    bullet: '*', // リストアイテムの箇条書きに`*`を使用（デフォルト）
    // 詳細なオプションについては <https://github.com/remarkjs/remark/tree/main/packages/remark-stringify> を参照してください。
  },
  plugins: [
    remarkPresetLintConsistent, // マークダウンの一貫性をチェックします。
    remarkPresetLintRecommended, // いくつかの推奨ルール。
    // `## Contents`に目次を生成します
    [remarkToc, {heading: 'contents'}]
  ]
}

export default remarkConfig
```

これは`.remarkrc.yml`に配置できる同じ設定のYAMLバージョンです：

```yml
settings:
  bullet: "*"
plugins:
  # マークダウンの一貫性をチェックします。
  - remark-preset-lint-consistent
  # いくつかの推奨ルール。
  - remark-preset-lint-recommended
  # `## Contents`に目次を生成します
  - - remark-toc
    - heading: contents
```

`remark-cli`がマークダウンファイルを処理しようとすると、そのファイルが存在するフォルダから始めて、ファイルシステムを上向きに設定ファイルを検索します。
以下のファイル構造を例として考えてみましょう：

```txt
folder/
├─ subfolder/
│  ├─ .remarkrc.json
│  └─ file.md
├─ .remarkrc.js
├─ package.json
└─ readme.md
```

`folder/subfolder/file.md`が処理される場合、最も近い設定ファイルは`folder/subfolder/.remarkrc.json`です。
`folder/readme.md`の場合は`folder/.remarkrc.js`です。

優先順位は以下の通りです。
先に来るものが優先されます（つまり、上記のファイル構造では`folder/.remarkrc.js`が`folder/package.json`よりも優先されます）：

1. `.remarkrc`（JSON）
2. `.remarkrc.cjs`（CJS）
3. `.remarkrc.js`（CJSまたはESM、`package.json`の`type: 'module'`に依存）
4. `.remarkrc.json`（JSON）
5. `.remarkrc.mjs`（ESM）
6. `.remarkrc.yaml`（YAML）
7. `.remarkrc.yml`（YAML）
8. `remarkConfig`フィールドを持つ`package.json`

## 互換性

unified collectiveによってメンテナンスされているプロジェクトは、メンテナンスされているバージョンのNode.jsと互換性があります。

新しいメジャーリリースをカットする際、メンテナンスされていないバージョンのNode.jsのサポートを削除します。
これは、現在のリリースライン、`remark-cli@^12`を、Node.js 16と互換性を保つよう努めていることを意味します。

## セキュリティ

マークダウンはHTMLに変換できるため、HTMLの不適切な使用は[クロスサイトスクリプティング（XSS）][xss]攻撃を引き起こす可能性があるため、remarkの使用は安全でない場合があります。
HTMLに移行する場合、おそらくremarkを**[rehype][]**と組み合わせることになり、その場合は[`rehype-sanitize`][rehype-sanitize]を使用する必要があります。

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

[downloads-badge]: https://img.shields.io/npm/dm/remark-cli.svg

[downloads]: https://www.npmjs.com/package/remark-cli

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

[markdown-extensions]: https://github.com/sindresorhus/markdown-extensions

[rehype]: https://github.com/rehypejs/rehype

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[remark]: https://github.com/remarkjs/remark

[remark-core]: ../remark/

[remark-toc]: https://github.com/remarkjs/remark-toc

[config-file]: https://github.com/unifiedjs/unified-engine#config-files

[ignore-file]: https://github.com/unifiedjs/unified-engine#ignore-files

[unified-args]: https://github.com/unifiedjs/unified-args#cli

[markdown-style-guide]: https://github.com/remarkjs/remark-lint/tree/main/packages/remark-preset-lint-markdown-style-guide

[xss]: https://en.wikipedia.org/wiki/Cross-site_scripting