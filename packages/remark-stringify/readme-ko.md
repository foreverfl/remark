# remark-stringify

[![빌드][build-badge]][build]
[![커버리지][coverage-badge]][coverage]
[![다운로드][downloads-badge]][downloads]
[![크기][size-badge]][size]
[![스폰서][sponsors-badge]][collective]
[![후원자][backers-badge]][collective]
[![채팅][chat-badge]][chat]

**[remark][]** 마크다운으로 직렬화를 지원하기 위한 플러그인입니다.

## 목차

* [이것은 무엇인가요?](#이것은-무엇인가요)
* [언제 이것을 사용해야 하나요?](#언제-이것을-사용해야-하나요)
* [설치](#설치)
* [사용법](#사용법)
* [API](#api)
  * [`unified().use(remarkStringify[, options])`](#unifieduseremarkstringify-options)
  * [`Options`](#options)
* [구문](#구문)
* [구문 트리](#구문-트리)
* [타입](#타입)
* [호환성](#호환성)
* [보안](#보안)
* [기여하기](#기여하기)
* [후원하기](#후원하기)
* [라이선스](#라이선스)

## 이것은 무엇인가요?

이 패키지는 [unified][] ([remark][]) 플러그인으로, 구문 트리를 입력으로 받아 직렬화된 마크다운으로 변환하는 방법을 정의합니다.
이 플러그인이 사용되면 최종 결과물로 마크다운이 직렬화됩니다.

remark 생태계에 대한 정보는 [remark 모노레포의 readme][remark]를 참조하세요.

## 언제 이것을 사용해야 하나요?

이 플러그인은 unified에 마크다운 직렬화 지원을 추가합니다.
마크다운 파싱도 필요하다면 [`remark`][remark-core]를 대신 사용할 수 있습니다. 이는 unified, [`remark-parse`][remark-parse], 그리고 이 플러그인을 결합한 것입니다.

플러그인을 사용하지 않고 구문 트리에 접근할 수 있다면 이 플러그인 내부에서 사용되는 [`mdast-util-to-markdown`][mdast-util-to-markdown]을 직접 사용할 수 있습니다.
remark는 이러한 내부 구조를 추상화하여 콘텐츠 변환을 더 쉽게 만드는 데 중점을 둡니다.

이 플러그인을 다른 플러그인과 결합하여 구문 확장을 추가할 수 있습니다.
주목할 만한 예로 [`remark-gfm`][remark-gfm], [`remark-mdx`][remark-mdx], [`remark-frontmatter`][remark-frontmatter], [`remark-math`][remark-math], [`remark-directive`][remark-directive]가 있습니다.
`remark-stringify` 이전에 다른 [remark 플러그인][remark-plugin]을 사용할 수도 있습니다.

## 설치

이 패키지는 [ESM 전용][esm]입니다.
Node.js (버전 16+)에서는 [npm][]으로 설치하세요:

```sh
npm install remark-stringify
```

Deno에서는 [`esm.sh`][esmsh]를 사용하세요:

```js
import remarkStringify from 'https://esm.sh/remark-stringify@11'
```

브라우저에서는 [`esm.sh`][esmsh]를 사용하세요:

```html
<script type="module">
  import remarkStringify from 'https://esm.sh/remark-stringify@11?bundle'
</script>
```

## 사용법

다음과 같은 `example.js` 모듈이 있다고 가정해 봅시다:

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

...그리고 `node example.js`를 실행하면 다음과 같은 결과가 나옵니다:

```markdown
# Uranus

**Uranus** is the seventh [planet](/wiki/Planet "Planet") from the Sun and is a gaseous cyan [ice giant](/wiki/Ice_giant "Ice giant").
```

## API

이 패키지는 식별자를 내보내지 않습니다.
기본 내보내기는 [`remarkStringify`][api-remark-stringify]입니다.

### `unified().use(remarkStringify[, options])`

마크다운으로 직렬화하는 지원을 추가합니다.

###### 매개변수

* `options` ([`Options`][api-options], 선택 사항)
  — 설정

###### 반환값

없음 (`undefined`).

### `Options`

설정 (TypeScript 타입).

###### 필드

* `bullet` (`'*'`, `'+'`, 또는 `'-'`, 기본값: `'*'`)
  — 순서 없는 목록 항목의 글머리 기호로 사용할 마커
* `bulletOther` (`'*'`, `'+'`, 또는 `'-'`, 기본값: `bullet`이 `'*'`일 때 `'-'`, 그 외에는 `'*'`)
  — 주 글머리 기호가 작동하지 않는 특정 경우에 사용할 마커; `bullet`과 같을 수 없음
* `bulletOrdered` (`'.'` 또는 `')'`, 기본값: `'.'`)
  — 순서 있는 목록 항목의 글머리 기호로 사용할 마커
* `closeAtx` (`boolean`, 기본값: `false`)
  — ATX 제목의 끝에 시작 시퀀스와 동일한 수의 숫자 기호(`#`)를 추가
* `emphasis` (`'*'` 또는 `'_'`, 기본값: `'*'`)
  — 강조에 사용할 마커
* `fence` (``'`'`` 또는 `'~'`, 기본값: ``'`'``)
  — 코드 펜스에 사용할 마커
* `fences` (`boolean`, 기본값: `true`)
  — 항상 코드 펜스 사용; `false`일 경우, 언어가 정의되어 있거나 코드가 비어 있거나 빈 줄로 시작하거나 끝나는 경우에만 코드 펜스 사용
* `handlers` (`Handlers`, 선택 사항)
  — 특정 노드 처리;
  자세한 정보는 [`mdast-util-to-markdown`][mdast-util-to-markdown] 참조
* `incrementListMarker` (`boolean`, 기본값: `true`)
  — 순서 있는 목록 항목의 카운터 증가
* `join` (`Array<Join>`, 선택 사항)
  — 블록을 연결하는 방법;
  자세한 정보는 [`mdast-util-to-markdown`][mdast-util-to-markdown] 참조
* `listItemIndent` (`'mixed'`, `'one'`, 또는 `'tab'`, 기본값: `'one'`)
  — 목록 항목 내용의 들여쓰기 방법;
  글머리 기호 크기에 공백 하나를 더한 크기(`'one'`인 경우), 탭 정지(`'tab'`), 또는 항목과 상위 목록에 따라 다름: `'mixed'`는 항목과 목록이 꽉 차있으면 `'one'`을 사용하고 그렇지 않으면 `'tab'`을 사용
* `quote` (`'"'` 또는 `"'"`, 기본값: `'"'`)
  — 제목에 사용할 마커
* `resourceLink` (`boolean`, 기본값: `false`)
  — 항상 리소스 링크(`[text](url)`) 사용;
  `false`일 경우 가능한 경우 자동 링크(`<https://example.com>`) 사용
* `rule` (`'*'`, `'-'`, 또는 `'_'`, 기본값: `'*'`)
  — 구분선에 사용할 마커
* `ruleRepetition` (`number`, 기본값: `3`, 최소: `3`)
  — 구분선에 사용할 마커 수
* `ruleSpaces` (`boolean`, 기본값: `false`)
  — 구분선의 마커 사이에 공백 추가
* `setext` (`boolean`, 기본값: `false`)
  — 가능한 경우 setext 제목 사용;
  `true`일 경우 비어 있지 않은 랭크 1 또는 2 제목에 대해 setext 제목(`heading\n=======`) 사용
* `strong` (`'*'` 또는 `'_'`, 기본값: `'*'`)
  — 강조에 사용할 마커
* `tightDefinitions` (`boolean`, 기본값: `false`)
  — 정의를 빈 줄 없이 연결
* `unsafe` (`Array<Unsafe>`, 선택 사항)
  — 문자가 발생할 수 없는 경우를 정의하는 스키마;
  자세한 정보는 [`mdast-util-to-markdown`][mdast-util-to-markdown] 참조

<!-- 참고: `extensions`는 의도적으로 지원/문서화되지 않았습니다. -->

## 구문

마크다운은 CommonMark에 따라 직렬화되지만 대부분의 마크다운 파서와 호환되는 방식으로 형식화됩니다.
다른 플러그인을 사용하여 구문 확장에 대한 지원을 추가할 수 있습니다.

## 구문 트리

remark에서 사용되는 구문 트리는 [mdast][]입니다.

## 타입

이 패키지는 [TypeScript][]로 완전히 타입이 지정되어 있습니다.
추가로 [`Options`][api-options] 타입을 내보냅니다.

또한 `unified`에 `Settings`를 등록합니다.
`.data('settings', …)`로 옵션을 전달하는 경우, 이 패키지를 타입 어딘가에서 가져와야 합니다. 이렇게 하면 필드가 등록됩니다.

```js
/// <reference types="remark-stringify" />

import {unified} from 'unified'

// @ts-expect-error: `thisDoesNotExist`는 유효한 옵션이 아닙니다.
unified().data('settings', {thisDoesNotExist: false})
```

## 호환성

unified collective에서 유지 관리하는 프로젝트는 유지 관리되는 Node.js 버전과 호환됩니다.

새로운 메이저 릴리스를 할 때, 유지 관리되지 않는 Node.js 버전에 대한 지원을 중단합니다.
이는 현재 릴리스 라인인 `remark-stringify@^11`을 Node.js 16과 호환되도록 유지하려 노력한다는 의미입니다.

## 보안

`remark-stringify` 사용은 안전합니다.

remark 플러그인 사용은 공격에 취약할 수 있습니다.
각 플러그인과 관련된 위험을 신중히 평가하세요.

보고서 제출 방법에 대한 정보는 [보안 정책][security]을 참조하세요.

## 기여하기

시작하는 방법은 [`remarkjs/.github`][health]의 [`contributing.md`][contributing]를 참조하세요.
도움을 받는 방법은 [`support.md`][support]를 참조하세요.
커뮤니티 및 기여자와 대화하려면 [Discussions][chat]에 참여하세요.

이 프로젝트에는 [행동 강령][coc]이 있습니다.
이 저장소, 조직 또는 커뮤니티와 상호 작용함으로써 귀하는 그 조건을 준수하는 데 동의합니다.

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
  <a href="https://opencollective.com/unified"><strong>당신도 함께 하시겠습니까?</strong></a>
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