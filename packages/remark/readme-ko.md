# remark

[![빌드][build-badge]][build]
[![커버리지][coverage-badge]][coverage]
[![다운로드][downloads-badge]][downloads]
[![크기][size-badge]][size]
[![스폰서][sponsors-badge]][collective]
[![후원자][backers-badge]][collective]
[![채팅][chat-badge]][chat]

마크다운 파싱 및 마크다운으로의 직렬화를 지원하는 **[unified][]** 프로세서입니다.

## 목차

* [이것은 무엇인가요?](#이것은-무엇인가요)
* [언제 이것을 사용해야 하나요?](#언제-이것을-사용해야-하나요)
* [설치](#설치)
* [사용법](#사용법)
* [API](#api)
  * [`remark()`](#remark-1)
* [예시](#예시)
  * [예시: 마크다운 검사](#예시-마크다운-검사)
  * [예시: `remark-stringify`에 옵션 전달](#예시-remark-stringify에-옵션-전달)
* [구문](#구문)
* [구문 트리](#구문-트리)
* [타입](#타입)
* [호환성](#호환성)
* [보안](#보안)
* [기여하기](#기여하기)
* [후원하기](#후원하기)
* [라이선스](#라이선스)

## 이것은 무엇인가요?

이 패키지는 unified를 [`remark-parse`][remark-parse]와 [`remark-stringify`][remark-stringify]와 함께 사용하여 입력으로 마크다운을 파싱하고 출력으로 마크다운을 직렬화하는 기능을 지원하는 [unified][] 프로세서입니다.

remark 생태계에 대한 자세한 정보는 [remark 모노레포의 readme][remark]를 참조하세요.

## 언제 이것을 사용해야 하나요?

unified를 사용하고 입력으로 마크다운을 사용하며 출력으로 마크다운을 원할 때 이 패키지를 사용할 수 있습니다.
이 패키지는 `unified().use(remarkParse).use(remarkStringify)`의 단축형입니다.
입력이 마크다운이 아니거나(`remark-parse`가 필요 없는 경우) 출력이 마크다운이 아닌 경우(`remark-stringify`가 필요 없는 경우)에는 `unified`를 직접 사용하는 것이 좋습니다.

프로젝트에서 명령줄로 마크다운 파일을 검사하고 포맷팅하려면 [`remark-cli`][remark-cli]를 사용할 수 있습니다.

## 설치

이 패키지는 [ESM 전용][esm]입니다.
Node.js(버전 16+)에서는 [npm][]으로 설치하세요:

```sh
npm install remark
```

Deno에서는 [`esm.sh`][esmsh]를 사용하세요:

```js
import {remark} from 'https://esm.sh/remark@15'
```

브라우저에서는 [`esm.sh`][esmsh]를 사용하세요:

```html
<script type="module">
  import {remark} from 'https://esm.sh/remark@15?bundle'
</script>
```

## 사용법

다음과 같은 `example.js` 모듈이 있다고 가정해 봅시다:

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

...`node example.js`로 실행하면 다음과 같은 결과가 나옵니다:

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

이 패키지는 [`remark`][api-remark] 식별자를 내보냅니다.
기본 내보내기는 없습니다.

### `remark()`

[`remark-parse`][remark-parse]와 [`remark-stringify`][remark-stringify]를 이미 사용하는 새로운 unified 프로세서를 생성합니다.

`use`를 사용하여 더 많은 플러그인을 추가할 수 있습니다.
자세한 정보는 [`unified`][unified]를 참조하세요.

## 예시

### 예시: 마크다운 검사

다음 예시는 마크다운 코드 스타일이 일관되고 몇 가지 모범 사례를 따르는지 확인합니다:

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

### 예시: `remark-stringify`에 옵션 전달

`remark-stringify`를 수동으로 사용할 때는 `use`에 옵션을 전달할 수 있습니다.
`remark-stringify`가 이미 `remark`에서 사용되고 있기 때문에 그렇게 할 수 없습니다.
`remark-stringify`에 대한 옵션을 정의하려면 대신 `data`에 옵션을 전달할 수 있습니다:

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

결과:

```markdown
Moons of Neptune
================

1) Naiad
1) Thalassa
1) Despine
1) …
```

## 구문

마크다운은 CommonMark에 따라 파싱되고 직렬화됩니다.
다른 플러그인을 사용하여 구문 확장에 대한 지원을 추가할 수 있습니다.

## 구문 트리

remark에서 사용되는 구문 트리는 [mdast][]입니다.

## 타입

이 패키지는 [TypeScript][]로 완전히 타입이 지정되어 있습니다.
추가로 내보내는 타입은 없습니다.

또한 `unified`에 `Settings`를 등록합니다.
`.data('settings', …)`로 옵션을 전달하는 경우, 이 패키지를 타입 어딘가에서 가져와야 합니다. 이렇게 하면 필드가 등록됩니다.

```js
/// <reference types="remark" />

import {unified} from 'unified'

// @ts-expect-error: `thisDoesNotExist`는 유효한 옵션이 아닙니다.
unified().data('settings', {thisDoesNotExist: false})
```

## 호환성

unified 집단에서 유지 관리하는 프로젝트는 유지 관리되는 Node.js 버전과 호환됩니다.

새로운 주요 버전을 출시할 때, 유지 관리되지 않는 Node.js 버전에 대한 지원을 중단합니다.
이는 현재 릴리스 라인인 `remark@^15`를 Node.js 16과 호환되도록 유지하려 노력한다는 의미입니다.

## 보안

마크다운은 HTML로 변환될 수 있고 HTML의 부적절한 사용은 [크로스 사이트 스크립팅 (XSS)][xss] 공격에 노출될 수 있으므로, remark 사용은 안전하지 않을 수 있습니다.
HTML로 변환할 때는 remark를 **[rehype][]**와 결합하게 되는데, 이 경우 [`rehype-sanitize`][rehype-sanitize]를 사용해야 합니다.

remark 플러그인 사용은 다른 공격에 노출될 수도 있습니다.
각 플러그인과 그 사용에 따른 위험을 신중히 평가하세요.

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