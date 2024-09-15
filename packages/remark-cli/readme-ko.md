# remark-cli

[![빌드][build-badge]][build]
[![커버리지][coverage-badge]][coverage]
[![다운로드][downloads-badge]][downloads]
[![스폰서][sponsors-badge]][collective]
[![후원자][backers-badge]][collective]
[![채팅][chat-badge]][chat]

**[remark][]** 를 사용하여 마크다운 파일을 검사하고 변경하기 위한 명령줄 인터페이스입니다.

## 목차

* [이것은 무엇인가요?](#이것은-무엇인가요)
* [언제 이것을 사용해야 하나요?](#언제-이것을-사용해야-하나요)
* [설치](#설치)
* [사용법](#사용법)
* [CLI](#cli)
* [예시](#예시)
  * [예시: CLI에서 마크다운 검사 및 포맷팅](#예시-cli에서-마크다운-검사-및-포맷팅)
  * [예시: 설정 파일 (JSON, YAML, JS)](#예시-설정-파일-json-yaml-js)
* [호환성](#호환성)
* [보안](#보안)
* [기여하기](#기여하기)
* [후원하기](#후원하기)
* [라이선스](#라이선스)

## 이것은 무엇인가요?

이 패키지는 터미널이나 npm 스크립트 등에서 마크다운 파일을 검사하고 변경하는 데 사용할 수 있는 명령줄 인터페이스(CLI)입니다.
이 CLI는 마크다운을 구조화된 데이터, 특히 AST(추상 구문 트리)로 다루는 플러그인 생태계인 remark를 기반으로 구축되었습니다.
150개 이상의 기존 플러그인 중에서 선택하거나 직접 만들 수 있습니다.

remark 생태계에 대한 자세한 정보는 [remark 모노레포의 readme][remark]를 참조하세요.

## 언제 이것을 사용해야 하나요?

프로젝트의 마크다운 파일을 명령줄에서 작업하고 싶을 때 이 패키지를 사용할 수 있습니다.
`remark-cli`에는 많은 옵션이 있고 많은 플러그인과 결합할 수 있으므로 원하는 작업을 수행할 수 있어야 합니다.
그렇지 않은 경우, 스크립트에서 [`remark`][remark-core] 자체를 수동으로 사용할 수 있습니다.

## 설치

이 패키지는 [ESM 전용][esm]입니다.
Node.js(버전 16+)에서 [npm][]으로 설치하세요:

```sh
npm install remark-cli
```

## 사용법

[`remark-toc`][remark-toc]를 사용하여 `readme.md`에 목차를 추가합니다:

```sh
remark readme.md --output --use remark-toc
```

[`remark-preset-lint-markdown-style-guide`][markdown-style-guide]를 사용하여 마크다운 스타일 가이드에 따라 현재 디렉토리의 모든 마크다운 파일을 검사합니다.

```sh
remark . --use remark-preset-lint-markdown-style-guide
```

## CLI

`remark-cli`의 인터페이스는 도움말 페이지(`remark --help`)에서 다음과 같이 설명됩니다:

```txt
사용법: remark [옵션] [경로 | 파일 패턴 ...]

  remark로 마크다운을 처리하는 CLI

옵션:

      --[no-]color                        보고서에 색상 지정 (기본값: 켜짐)
      --[no-]config                       구성 파일 검색 (기본값: 켜짐)
  -e  --ext <확장자>                      확장자 지정
      --file-path <경로>                  처리할 경로 지정
  -f  --frail                             경고 시 1로 종료
  -h  --help                              사용법 정보 출력
      --[no-]ignore                       무시 파일 검색 (기본값: 켜짐)
  -i  --ignore-path <경로>                무시 파일 지정
      --ignore-path-resolve-from cwd|dir  `ignore-path`의 패턴을 해당 디렉토리 또는 cwd에서 해결
      --ignore-pattern <글로브>           무시 패턴 지정
      --inspect                           형식화된 구문 트리 출력
  -o  --output [경로]                     출력 위치 지정
  -q  --quiet                             경고와 오류만 출력
  -r  --rc-path <경로>                    구성 파일 지정
      --report <리포터>                   리포터 지정
  -s  --setting <설정>                    설정 지정
  -S  --silent                            오류만 출력
      --silently-ignore                   무시된 파일이 주어졌을 때 실패하지 않음
      --[no-]stdout                       stdout에 쓰기 지정 (기본값: 켜짐)
  -t  --tree                              입력과 출력을 구문 트리로 지정
      --tree-in                           입력을 구문 트리로 지정
      --tree-out                          구문 트리 출력
  -u  --use <플러그인>                    플러그인 사용
      --verbose                           메시지에 대한 추가 정보 보고
  -v  --version                           버전 번호 출력
  -w  --watch                             변경 사항을 감시하고 재처리

예시:

  # `input.md` 처리
  $ remark input.md -o output.md

  # 파이프
  $ remark < input.md > output.md

  # 모든 해당 파일 다시 쓰기
  $ remark . -o
```

이 모든 옵션에 대한 자세한 정보는 작업을 수행하는 [`unified-args`][unified-args]에서 확인할 수 있습니다.
`remark-cli`는 다음과 같이 사전 구성된 `unified-args`입니다:

* `remark-` 플러그인 로드
* 마크다운 확장자 검색
  ([`.md`, `.markdown` 등][markdown-extensions])
* [`.remarkignore` 파일][ignore-file]에서 찾은 경로 무시
* [`.remarkrc`, `.remarkrc.js` 등 파일][config-file]에서 구성 로드
* [`package.json` 파일의 `remarkConfig` 필드][config-file]에서 구성 사용

## 예시

### 예시: CLI에서 마크다운 검사 및 포맷팅

이 예시에서는 `remark-cli`를 사용하여 마크다운을 검사하고 포맷팅합니다.
Node.js 패키지에 있다고 가정합니다.

CLI와 플러그인을 설치합니다:

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
이는 모든 마크다운 파일(`.`)에서 remark를 실행하고 다시 씁니다(`--output`).
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
      "remark-preset-lint-consistent", // 마크다운이 일관성 있는지 확인합니다.
      "remark-preset-lint-recommended", // 몇 가지 권장 규칙.
      [
        // `## Contents`에 목차 생성
        "remark-toc",
        {
          "heading": "contents"
        }
      ]
    ]
  },
  /* … */
```

> 👉 **참고**: 위의 예시를 복사/붙여넣을 때는 주석을 제거해야 합니다. `package.json` 파일에서는 주석이 지원되지 않습니다.

마지막으로, npm 스크립트를 실행하여 프로젝트의 마크다운 파일을 검사하고 포맷팅할 수 있습니다:

```sh
npm run format
```

### 예시: 설정 파일 (JSON, YAML, JS)

이전 예시에서 `remark-cli`가 `package.json` 파일 내에서 구성되는 것을 보았습니다.
구성이 비교적 짧고, `package.json`이 있으며, 주석이 필요하지 않은 경우(JSON에서는 주석이 허용되지 않음) 좋은 위치입니다.

별도의 파일에서 다른 언어로 구성을 정의할 수도 있습니다.
`package.json` 구성을 참고로 하여, `.remarkrc.js`에 배치할 수 있는 JavaScript 버전은 다음과 같습니다:

```js
import remarkPresetLintConsistent from 'remark-preset-lint-consistent'
import remarkPresetLintRecommended from 'remark-preset-lint-recommended'
import remarkToc from 'remark-toc'

const remarkConfig = {
  settings: {
    bullet: '*', // 목록 항목 글머리에 `*` 사용 (기본값)
    // 더 많은 옵션은 <https://github.com/remarkjs/remark/tree/main/packages/remark-stringify>를 참조하세요.
  },
  plugins: [
    remarkPresetLintConsistent, // 마크다운이 일관성 있는지 확인합니다.
    remarkPresetLintRecommended, // 몇 가지 권장 규칙.
    // `## Contents`에 목차 생성
    [remarkToc, {heading: 'contents'}]
  ]
}

export default remarkConfig
```

다음은 `.remarkrc.yml`에 배치할 수 있는 YAML의 동일한 구성입니다:

```yml
settings:
  bullet: "*"
plugins:
  # 마크다운이 일관성 있는지 확인합니다.
  - remark-preset-lint-consistent
  # 몇 가지 권장 규칙.
  - remark-preset-lint-recommended
  # `## Contents`에 목차 생성
  - - remark-toc
    - heading: contents
```

`remark-cli`가 마크다운 파일을 처리하려고 할 때, 해당 파일이 존재하는 폴더에서 시작하여 파일 시스템을 위로 올라가며 구성 파일을 검색합니다.
다음 파일 구조를 예로 들어 설명하겠습니다:

```txt
folder/
├─ subfolder/
│  ├─ .remarkrc.json
│  └─ file.md
├─ .remarkrc.js
├─ package.json
└─ readme.md
```

`folder/subfolder/file.md`가 처리될 때, 가장 가까운 구성 파일은 `folder/subfolder/.remarkrc.json`입니다.
`folder/readme.md`의 경우, 가장 가까운 구성 파일은 `folder/.remarkrc.js`입니다.

우선 순위는 다음과 같습니다.
먼저 나오는 것이 우선합니다(따라서 위의 파일 구조에서 `folder/.remarkrc.js`가 `folder/package.json`보다 우선합니다):

1. `.remarkrc` (JSON)
2. `.remarkrc.cjs` (CJS)
3. `.remarkrc.js` (CJS 또는 ESM, `package.json`의 `type: 'module'`에 따라 다름)
4. `.remarkrc.json` (JSON)
5. `.remarkrc.mjs` (ESM)
6. `.remarkrc.yaml` (YAML)
7. `.remarkrc.yml` (YAML)
8. `remarkConfig` 필드가 있는 `package.json`

## 호환성

unified collective에서 유지 관리하는 프로젝트는 유지 관리되는 Node.js 버전과 호환됩니다.

새로운 주요 버전을 출시할 때, 유지 관리되지 않는 Node.js 버전에 대한 지원을 중단합니다.
이는 현재 릴리스 라인인 `remark-cli@^12`를 Node.js 16과 호환되도록 유지하려 노력한다는 의미입니다.

## 보안

마크다운은 HTML로 변환될 수 있고 HTML의 부적절한 사용은 [크로스 사이트 스크립팅 (XSS)][xss] 공격에 노출될 수 있으므로, remark 사용은 안전하지 않을 수 있습니다.
HTML로 변환할 때는 대개 remark를 **[rehype][]** 와 함께 사용하게 되는데, 이 경우 [`rehype-sanitize`][rehype-sanitize]를 사용해야 합니다.

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