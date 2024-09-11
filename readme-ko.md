# ![remark][logo]

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

**remark**는 플러그인을 사용하여 마크다운을 변환하는 도구입니다.
이 플러그인들은 마크업을 검사하고 변경할 수 있습니다.
remark를 서버, 클라이언트, CLI, deno 등에서 사용할 수 있습니다.

## 주요 기능 하이라이트

* [x] **[호환성][syntax]**
  — CommonMark에 100%, 플러그인을 통해 GFM 또는 MDX에 100% 호환
* [x] **[AST][syntax-tree]**
  — 콘텐츠 검사 및 변경을 쉽게 만듦
* [x] **[인기][popular]**
  — 세계에서 가장 인기 있는 마크다운 파서
* [x] **[플러그인][plugins]**
  — 150개 이상의 플러그인 중 선택 가능

## 소개

remark는 마크다운을 구조화된 데이터, 특히 AST(추상 구문 트리)로 다루는 플러그인 생태계입니다.
AST는 프로그램이 마크다운을 쉽게 다룰 수 있게 해줍니다.
우리는 이러한 프로그램을 플러그인이라고 부릅니다.
플러그인은 트리를 검사하고 변경합니다.
많은 기존 플러그인을 사용하거나 직접 만들 수 있습니다.

* 마크다운 학습을 위해서는 이 [치트시트와 튜토리얼][cheat]을 참조하세요
* 우리에 대해 더 알아보려면 [`unifiedjs.com`][site]을 참조하세요
* 업데이트는 [Twitter][]를 참조하세요
* 질문이 있다면 [support][]를 참조하세요
* 도움을 주고 싶다면 아래의 [contribute][] 또는 [sponsor][]를 참조하세요

## 목차

* [이것은 무엇인가요?](#이것은-무엇인가요)
* [언제 이것을 사용해야 하나요?](#언제-이것을-사용해야-하나요)
* [플러그인](#플러그인)
* [예제](#예제)
  * [예제: 마크다운을 HTML로 변환하기](#예제-마크다운을-html로-변환하기)
  * [예제: GFM과 frontmatter 지원](#예제-gfm과-frontmatter-지원)
  * [예제: 마크다운 검사하기](#예제-마크다운-검사하기)
  * [예제: CLI에서 마크다운 검사 및 포맷팅하기](#예제-cli에서-마크다운-검사-및-포맷팅하기)
* [구문](#구문)
* [구문 트리](#구문-트리)
* [타입](#타입)
* [호환성](#호환성)
* [보안](#보안)
* [기여하기](#기여하기)
* [후원하기](#후원하기)
* [라이선스](#라이선스)

## 이것은 무엇인가요?

이 프로젝트와 플러그인을 사용하면 다음과 같은 마크다운을:

```markdown
# Hello, *Mercury*!
```

...다음과 같은 HTML로 변환할 수 있습니다:

```html
<h1>Hello, <em>Mercury</em>!</h1>
```

<details><summary>예제 코드 보기</summary>

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

다른 플러그인을 사용하면 다음과 같은 마크다운을:

```markdown
# Hi, Saturn!
```

...다음과 같은 마크다운으로 변환할 수 있습니다:

```markdown
## Hi, Saturn!
```

<details><summary>예제 코드 보기</summary>

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

remark를 다양한 용도로 사용할 수 있습니다.
**[unified][]**는 AST를 사용하여 콘텐츠를 변환하는 핵심 프로젝트입니다.
**remark**는 unified에 마크다운 지원을 추가합니다.
**[mdast][]**는 remark가 사용하는 마크다운 AST입니다.

이 GitHub 저장소는 다음 패키지를 포함하는 모노레포입니다:

* [`remark-parse`][remark-parse]
  — 마크다운을 입력으로 받아 구문 트리(mdast)로 변환하는 플러그인
* [`remark-stringify`][remark-stringify]
  — 구문 트리(mdast)를 받아 마크다운 출력으로 변환하는 플러그인
* [`remark`][remark-core]
  — `unified`, `remark-parse`, `remark-stringify`를 포함하며, 입력과 출력이 모두 마크다운일 때 유용함
* [`remark-cli`][remark-cli]
  — 스크립트에서 마크다운을 검사하고 포맷팅하는 도구인 remark를 위한 CLI

## 언제 이것을 사용해야 하나요?

가지고 있는 입력과 원하는 출력에 따라 remark의 다른 부분을 사용할 수 있습니다.
입력이 마크다운이라면 `unified`와 함께 `remark-parse`를 사용할 수 있습니다.
출력이 마크다운이라면 `unified`와 함께 `remark-stringify`를 사용할 수 있습니다.
입력과 출력이 모두 마크다운이라면 `remark`를 단독으로 사용할 수 있습니다.
프로젝트에서 마크다운 파일을 검사하고 포맷팅하고 싶다면 `remark-cli`를 사용할 수 있습니다.

마크다운을 HTML로 *단순히* 변환하고 싶다면(아마도 몇 가지 확장 기능과 함께), 대신 [`micromark`][micromark]를 추천합니다.

플러그인을 사용하지 않고 구문 트리를 수동으로 다루고 싶다면
[`mdast-util-from-markdown`][mdast-util-from-markdown]과
[`mdast-util-to-markdown`][mdast-util-to-markdown]을 사용할 수 있습니다.

## 플러그인

remark 플러그인은 마크다운을 다룹니다.
몇 가지 인기 있는 예시는 다음과 같습니다:

* [`remark-gfm`][remark-gfm]
  — GFM(GitHub flavored markdown) 지원 추가
* [`remark-lint`][remark-lint]
  — 마크다운을 검사하고 불일치에 대해 경고
* [`remark-toc`][remark-toc]
  — 목차 생성
* [`remark-rehype`][remark-rehype]
  — 마크다운을 HTML로 변환

이 플러그인들은 각각 마크다운 구문 확장, 트리 검사, 트리 변경, 다른 구문 트리로의 변환 등 수행하는 작업과 방식이 상당히 다르기 때문에 예시로 적합합니다.

이미 존재하는 150개 이상의 플러그인 중에서 선택할 수 있습니다.
플러그인을 찾는 좋은 방법 세 가지가 있습니다:

* [`awesome-remark`][awesome-remark]
  — 가장 멋진 프로젝트들을 모은 선정 목록
* [플러그인 목록][list-of-plugins]
  — 모든 플러그인 목록
* [`remark-plugin` 토픽][topic]
  — GitHub에서 태그된 모든 저장소

일부 플러그인은 여기 `@remarkjs` 조직에서 관리되고 있지만, 다른 플러그인들은 다른 곳에서 관리되고 있습니다.
누구나 remark 플러그인을 만들 수 있으므로, 항상 프로젝트에 종속성을 포함시킬지 결정할 때와 마찬가지로 remark 플러그인의 품질도 신중하게 평가해야 합니다.

## 예제

### 예제: 마크다운을 HTML로 변환하기

remark는 마크다운을 위한 생태계입니다.
HTML을 위한 다른 생태계는 [rehype][]입니다.
다음 예제는 [`remark-rehype`][remark-rehype]를 사용하여 두 생태계를 결합해 마크다운을 HTML로 변환합니다:

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

결과:

```html
<h1>Hello, Neptune!</h1>
```

### 예제: GFM과 frontmatter 지원

remark는 기본적으로 CommonMark를 지원합니다.
비표준 마크다운 확장은 플러그인으로 활성화할 수 있습니다.
다음 예제는 GFM(자동 링크 리터럴, 각주, 취소선, 표, 작업 목록)과 frontmatter(YAML) 지원을 추가합니다:

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

결과:

```html
<h1>Hi <del>Mars</del>Venus!</h1>
```

### 예제: 마크다운 검사하기

다음 예제는 마크다운 코드 스타일이 일관되고 권장 모범 사례를 따르는지 확인합니다:

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

결과:

```txt
          warning Missing newline character at end of file final-newline             remark-lint
1:1-1:35  warning Marker style should be `.`               ordered-list-marker-style remark-lint
1:4       warning Incorrect list-item indent: add 1 space  list-item-indent          remark-lint
1:25-1:34 warning Emphasis should use `_` as a marker      emphasis-marker           remark-lint

⚠ 4 warnings
```

### 예제: CLI에서 마크다운 검사 및 포맷팅하기

다음 예제는 터미널에서 사용할 수 있는 remark의 CLI(명령줄 인터페이스)인 `remark-cli`를 사용하여 마크다운을 검사하고 포맷팅합니다.
이 예제는 Node.js 패키지에 있다고 가정합니다.

먼저 CLI와 플러그인을 설치합니다:

```sh
npm install remark-cli remark-preset-lint-consistent remark-preset-lint-recommended remark-toc --save-dev
```

...그런 다음 `package.json`에 npm 스크립트를 추가합니다:

```js
  /* … */
  "scripts": {
    /* … */
    "format": "remark . --output",
    /* … */
  },
  /* … */
```

> 💡 **팁**: `format` 스크립트에 ESLint 등도 추가하세요.

위의 변경 사항은 `npm run format`으로 실행할 수 있는 `format` 스크립트를 추가합니다.
이는 모든 마크다운 파일(`.`)에 대해 remark를 실행하고 다시 작성합니다(`--output`).
CLI에 대한 자세한 정보는 `./node_modules/.bin/remark --help`를 실행하세요.

그런 다음 remark를 구성하기 위해 `package.json`에 `remarkConfig`를 추가합니다:

```js
  /* … */
  "remarkConfig": {
    "settings": {
      "bullet": "*", // 목록 항목 글머리에 `*` 사용 (기본값)
      // 더 많은 옵션은 <https://github.com/remarkjs/remark/tree/main/packages/remark-stringify>를 참조하세요.
    },
    "plugins": [
      "remark-preset-lint-consistent", // 마크다운이 일관되는지 확인합니다.
      "remark-preset-lint-recommended", // 몇 가지 권장 규칙.
      [
        // `## Contents`에 목차를 생성합니다
        "remark-toc",
        {
          "heading": "contents"
        }
      ]
    ]
  },
  /* … */
```

> 👉 **참고**: 위의 예제를 복사/붙여넣기할 때는 주석을 제거해야 합니다. `package.json` 파일에서는 주석이 지원되지 않습니다.

마지막으로, npm 스크립트를 실행하여 프로젝트의 마크다운 파일을 검사하고 포맷팅할 수 있습니다:

```sh
npm run format
```

## 구문

마크다운은 CommonMark에 따라 파싱되고 직렬화됩니다.
다른 플러그인을 사용하여 구문 확장을 지원할 수도 있습니다.

우리는 파싱에 [`micromark`][micromark]를 사용합니다.
마크다운, CommonMark, 확장에 대한 자세한 정보는 해당 문서를 참조하세요.

## 구문 트리

remark에서 사용되는 구문 트리는 [mdast][]입니다.
이는 마크다운 구조를 JSON 객체로 표현합니다.

이 마크다운:

```markdown
## Hello *Pluto*!
```

...는 다음과 같은 트리를 생성합니다(간결성을 위해 위치 정보는 제거됨):

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

## 타입

remark와 unified 프로젝트는 모두 [TypeScript][]로 완전히 타입이 지정되어 있습니다.
mdast의 타입은 [`@types/mdast`][types-mdast]에서 사용할 수 있습니다.

TypeScript가 작동하려면 플러그인의 타입을 지정하는 것이 중요합니다.
예를 들어:

```js
/**
 * @import {Root} from 'mdast'
 * @import {VFile} from 'vfile'
 */

/**
 * @typedef Options
 *   설정.
 * @property {boolean | null | undefined} [someField]
 *   어떤 옵션 (선택사항).
 */

/**
 * 내 플러그인.
 *
 * @param {Options | null | undefined} [options]
 *   설정 (선택사항).
 * @returns
 *   변환.
 */
export function myRemarkPluginAcceptingOptions(options) {
  /**
   * 변환.
   *
   * @param {Root} tree
   *   트리.
   * @param {VFile} file
   *   파일
   * @returns {undefined}
   *   없음.
   */
  return function (tree, file) {
    // 작업 수행.
  }
}
```

## 호환성

unified에서 유지 관리하는 프로젝트는 유지 관리되는 Node.js 버전과 호환됩니다.

새로운 주요 릴리스를 할 때, 우리는 유지 관리되지 않는 Node.js 버전에 대한 지원을 중단합니다.
이는 현재 릴리스 라인을 Node.js 16과 호환되도록 유지하려고 노력한다는 의미입니다.

## 보안

마크다운은 HTML로 변환될 수 있고 HTML의 부적절한 사용은 [크로스 사이트 스크립팅(XSS)][xss] 공격에 노출될 수 있기 때문에, remark의 사용은 안전하지 않을 수 있습니다.
HTML로 변환할 때는 remark를 **[rehype][]**와 결합하게 되는데, 이 경우 [`rehype-sanitize`][rehype-sanitize]를 사용해야 합니다.

remark 플러그인의 사용도 다른 공격에 노출될 수 있습니다.
각 플러그인과 그 사용에 따른 위험을 신중히 평가하세요.

보고서 제출 방법에 대한 정보는 [보안 정책][security]을 참조하세요.

## 기여하기

시작하는 방법은 [`remarkjs/.github`][health]의 [`contributing.md`][contributing]를 참조하세요.
도움을 받는 방법은 [`support.md`][support]를 참조하세요.
커뮤니티 및 기여자들과 대화하려면 [Discussions][chat]에 참여하세요.

이 프로젝트에는 [행동 강령][coc]이 있습니다.
이 저장소, 조직 또는 커뮤니티와 상호 작용함으로써 여러분은 이 조건을 준수하는 데 동의하는 것입니다.

## 후원하기

[OpenCollective][collective]에서 후원하여 이 노력을 지원하고 보답하세요!

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
  <a href="https://opencollective.com/unified"><strong>여기에 당신을 추가하세요!</strong></a>
  <br><br>
</td>
</tr>
</table>

## 라이선스

[MIT](license) © [Titus Wormer](https://wooorm.com)

<!-- 정의 -->

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

[syntax]: #구문

[syntax-tree]: #구문-트리

[plugins]: #플러그인

[contribute]: #기여하기

[sponsor]: #후원하기