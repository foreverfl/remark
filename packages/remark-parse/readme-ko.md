# remark-parse

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]

마크다운 파싱을 지원하기 위한 **[remark][]** 플러그인입니다.

## 목차

* [이것은 무엇인가요?](#이것은-무엇인가요)
* [언제 이것을 사용해야 하나요?](#언제-이것을-사용해야-하나요)
* [설치](#설치)
* [사용](#사용)
* [API](#api)
  * [`unified().use(remarkParse)`](#unifieduseremarkparse)
* [예제](#예제)
  * [예제: GFM과 프론트매터 지원](#예제-gfm과-프론트매터-지원)
  * [예제: 마크다운을 man 페이지로 변환](#예제-마크다운을-man-페이지로-변환)
* [구문](#구문)
* [구문 트리](#구문-트리)
* [타입](#타입)
* [호환성](#호환성)
* [보안](#보안)
* [기여하기](#기여하기)
* [후원하기](#후원하기)
* [라이선스](#라이선스)

## 이것은 무엇인가요?

이 패키지는 마크다운을 입력으로 받아 구문 트리로 변환하는 방법을 정의하는 [unified][] ([remark][]) 플러그인입니다.

remark 생태계에 대한 정보는 [remark 모노레포의 readme][remark]를 참조하세요.

## 언제 이것을 사용해야 하나요?

이 플러그인은 unified에 마크다운 파싱 지원을 추가합니다.
마크다운을 직렬화해야 하는 경우, unified, 이 플러그인, 그리고 [`remark-stringify`][remark-stringify]를 결합한 [`remark`][remark-core]를 대신 사용할 수 있습니다.

마크다운을 HTML로 변환하는 것만 필요하다면 (약간의 확장 기능 포함), [`micromark`][micromark]를 추천합니다.
플러그인을 사용하지 않고 구문 트리에 접근하려면 [`mdast-util-from-markdown`][mdast-util-from-markdown]을 직접 사용할 수 있습니다.
remark는 이러한 내부 구현을 추상화하여 콘텐츠 변환을 더 쉽게 만드는 데 중점을 둡니다.

이 플러그인을 다른 플러그인과 결합하여 구문 확장을 추가할 수 있습니다.
대표적인 예로는 [`remark-gfm`][remark-gfm], [`remark-mdx`][remark-mdx], [`remark-frontmatter`][remark-frontmatter], [`remark-math`][remark-math], [`remark-directive`][remark-directive]가 있습니다.
`remark-parse` 이후에 다른 [remark 플러그인][remark-plugin]을 사용할 수도 있습니다.

## 설치

이 패키지는 [ESM 전용][esm]입니다.
Node.js(버전 16+)에서는 [npm][]으로 설치하세요:

```sh
npm install remark-parse
```

Deno에서는 [`esm.sh`][esmsh]를 사용하세요:

```js
import remarkParse from 'https://esm.sh/remark-parse@11'
```

브라우저에서는 [`esm.sh`][esmsh]를 사용하세요:

```html
<script type="module">
  import remarkParse from 'https://esm.sh/remark-parse@11?bundle'
</script>
```

## 사용

다음과 같은 `example.js` 모듈이 있다고 가정해 봅시다:

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

`node example.js`를 실행하면 다음과 같은 결과가 나옵니다:

```html
<h1>Mercury</h1>
<p><strong>Mercury</strong> is the first planet from the <a href="https://en.wikipedia.org/wiki/Sun">Sun</a>
and the smallest planet in the Solar System.</p>
```

## API

이 패키지는 식별자를 내보내지 않습니다.
기본 내보내기는 [`remarkParse`][api-remark-parse]입니다.

### `unified().use(remarkParse)`

마크다운에서 파싱하는 지원을 추가합니다.

###### 매개변수

매개변수가 없습니다.

###### 반환값

아무것도 반환하지 않습니다 (`undefined`).

## 예제

### 예제: GFM과 프론트매터 지원

기본적으로 CommonMark를 지원합니다.
비표준 마크다운 확장은 플러그인으로 활성화할 수 있습니다.

이 예제는 GFM 기능(자동 링크 리터럴, 각주, 취소선, 표, 작업 목록)과 프론트매터(YAML)를 지원하는 방법을 보여줍니다:

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

### 예제: 마크다운을 man 페이지로 변환

Man 페이지(설명서 페이지의 줄임말)는 CLI를 문서화하는 방법입니다(예: 터미널에서 `man git-log` 입력).
이들은 roff라는 오래된 마크업 형식을 사용합니다.
roff로 직렬화할 수 있는 remark 플러그인인 [`remark-man`][remark-man]이 있습니다.

이 예제는 unified와 `remark-parse`, `remark-man`을 사용하여 마크다운을 man 페이지로 변환하는 방법을 보여줍니다:

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

결과:

```roff
.TH "TITAN" "7" "September 2023" "" ""
.SH "NAME"
\fBtitan\fR - largest moon of saturn
.P
Titan is the largest moon…
```

## 구문

마크다운은 CommonMark에 따라 파싱됩니다.
다양한 플러그인을 통해 구문 확장이 가능해집니다
마크다운 확장에 관심이 있다면, [micromark의 readme에서 더 많은 정보를 확인할 수 있습니다][micromark-extend].

## 구문 트리

remark에서 사용되는 구문 트리는 [mdast][]입니다.

## 타입

이 패키지는 [TypeScript][]로 완전히 타입이 지정되어 있습니다.
추가적으로 `Options` 타입을 내보냅니다(현재는 비어 있음).

## 호환성

unified collective에서 유지 관리하는 프로젝트들은 유지 관리되는 Node.js 버전과 호환됩니다.

새로운 주요 릴리스를 할 때, 우리는 유지 관리되지 않는 Node.js 버전에 대한 지원을 중단합니다.
이는 현재 릴리스 라인인 `remark-parse@^11`을 Node.js 16과 호환되도록 유지하려고 노력한다는 의미입니다.

## 보안

마크다운은 HTML로 변환될 수 있고 HTML의 부적절한 사용은 [크로스 사이트 스크립팅 (XSS)][xss] 공격에 취약할 수 있으므로, remark의 사용은 안전하지 않을 수 있습니다.
HTML로 변환할 때는 remark를 **[rehype][]** 와 결합해야 하며, 이 경우 [`rehype-sanitize`][rehype-sanitize]를 사용해야 합니다.

remark 플러그인의 사용은 다른 공격에도 노출될 수 있습니다.
각 플러그인과 그 사용에 따른 위험을 충분히 검토하세요.

보고서를 제출하는 방법에 대한 정보는 [보안 정책][security]을 참조하세요.

## 기여하기

시작하는 방법은 [`remarkjs/.github`][health]의 [`contributing.md`][contributing]를 참조하세요.
도움을 받는 방법은 [`support.md`][support]를 참조하세요.
커뮤니티 및 기여자들과 대화하려면 [Discussions][chat]에 참여하세요.

이 프로젝트에는 [행동 강령][coc]이 있습니다.
이 저장소, 조직 또는 커뮤니티와 상호 작용함으로써 여러분은 그 조건을 준수하는 데 동의하는 것입니다.

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
  <a href="https://opencollective.com/unified"><strong>여러분도 후원할 수 있습니다!</strong></a>
  <br><br>
</td>
</tr>
</table>

## 라이선스

[MIT][license] © [Titus Wormer][author]

<!-- 정의 -->

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