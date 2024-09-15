![remark][logo]

# 플러그인

**remark**는 플러그인을 사용하여 마크다운을 변환하는 도구입니다.
remark 생태계에 대한 정보는 [모노레포 readme][remark]를 참조하세요.
이 페이지에는 기존 플러그인들이 나열되어 있습니다.

## 목차

* [플러그인 목록](#플러그인-목록)
* [프리셋 목록](#프리셋-목록)
* [유틸리티 목록](#유틸리티-목록)
* [플러그인 사용하기](#플러그인-사용하기)
* [플러그인 만들기](#플러그인-만들기)

## 플러그인 목록

생태계에서 가장 뛰어난 프로젝트들은 [`awesome-remark`][awesome-remark]를 참조하세요.
더 많은 플러그인은 GitHub에서 [`remark-plugin` 토픽][topic]으로 태그된 것들을 찾을 수 있습니다.

> 👉 **참고**: 일부 플러그인은 기본 파서(micromark)의 변경으로 인해 remark의 최신 버전에서 작동하지 않습니다.
> 최신 상태이거나 영향을 받지 않는 플러그인은 `🟢`로 표시되어 있고, **현재 작동하지 않는** 플러그인은 `⚠️`로 표시되어 있습니다.

> 💡 **팁**: remark 플러그인은 마크다운과 작동하고 **rehype** 플러그인은 HTML과 작동합니다.
> 더 많은 플러그인은 rehype의 [플러그인 목록][rehype-plugins]을 참조하세요.

플러그인 목록:

* [`remark-a11y-emoji`](https://github.com/florianeckerstorfer/remark-a11y-emoji)
  — 접근 가능한 이모지
* ⚠️ [`remark-abbr`](https://github.com/zestedesavoir/zmarkdown/tree/HEAD/packages/remark-abbr#readme)
  — 약어를 위한 새로운 구문 (새로운 노드 유형, rehype 호환)
* ⚠️ [`remark-admonitions`](https://github.com/elviswolcott/remark-admonitions)
  — 경고문을 위한 새로운 구문
  (👉 **참고**: [`remark-directive`][d]가 유사하며 최신 상태입니다)
* ⚠️ [`remark-align`](https://github.com/zestedesavoir/zmarkdown/tree/HEAD/packages/remark-align#readme)
  — 텍스트나 블록을 정렬하기 위한 새로운 구문 (새로운 노드 유형, rehype 호환)
* 🟢 [`remark-api`](https://github.com/wooorm/remark-api)
  — API 섹션 생성
* ⚠️ [`remark-attr`](https://github.com/arobase-che/remark-attr)
  — 마크다운에 속성을 추가하기 위한 새로운 구문
* 🟢 [`remark-behead`](https://github.com/mrzmmr/remark-behead)
  — 제목 깊이 증가 또는 감소
* 🟢 [`remark-breaks`](https://github.com/remarkjs/remark-breaks)
  – 공백 없이 하드 브레이크 사용 (이슈에서처럼)
* 🟢 [`remark-capitalize`](https://github.com/zeit/remark-capitalize)
  – [`title.sh`](https://github.com/zeit/title)를 사용하여 모든 제목 변환
* 🟢 [`remark-capitalize-headings`](https://github.com/Xunnamius/unified-utils/blob/main/packages/remark-capitalize-headings)
  – 선택적으로 제목 대문자화
  (👉 **참고**: [`remark-capitalize`](https://github.com/zeit/remark-capitalize)의 대안)
* 🟢 [`remark-cite`](https://github.com/benrbray/remark-cite)
  – Pandoc 스타일 인용을 위한 새로운 구문
* 🟢 [`remark-cloudinary-docusaurus`](https://github.com/johnnyreilly/remark-cloudinary-docusaurus)
  – Docusaurus가 Cloudinary를 사용하여 최적화된 이미지를 제공할 수 있도록 함
* 🟢 [`remark-code-blocks`](https://github.com/mrzmmr/remark-code-blocks)
  — 코드 블록 선택 및 저장
* 🟢 [`remark-code-extra`](https://github.com/samlanning/remark-code-extra)
  — 코드 블록의 HTML 출력에 추가 또는 변환 (rehype 호환)
* 🟢 [`remark-code-frontmatter`](https://github.com/samlanning/remark-code-frontmatter)
  — 코드 블록에서 frontmatter 추출
* 🟢 [`remark-code-import`](https://github.com/kevin940726/remark-code-import)
  — 파일에서 코드 블록 채우기
* 🟢 [`remark-code-screenshot`](https://github.com/Swizec/remark-code-screenshot)
  – 코드 블록을 `carbon.now.sh` 스크린샷으로 변환
* 🟢 [`remark-code-title`](https://github.com/kevinzunigacuellar/remark-code-title)
  — 코드 블록에 제목 추가
* 🟢 [`remark-codesandbox`](https://github.com/kevin940726/remark-codesandbox)
  – 코드 블록에서 CodeSandbox 생성
* 🟢 [`remark-collapse`](https://github.com/Rokt33r/remark-collapse)
  — 섹션을 접을 수 있게 만들기
* 🟢 [`remark-comment-config`](https://github.com/remarkjs/remark-comment-config)
  — 주석으로 remark 구성
* ⚠️ [`remark-comments`](https://github.com/zestedesavoir/zmarkdown/tree/HEAD/packages/remark-comments#readme)
  — 무시할 것들을 위한 새로운 구문
* ⚠️ [`remark-container`](https://github.com/zWingz/remark-container)
  — 컨테이너를 위한 새로운 구문
  (👉 **참고**: [`remark-directive`][d]가 유사하며 최신 상태입니다)
* ⚠️ [`remark-containers`](https://github.com/Nevenall/remark-containers)
  — 컨테이너를 위한 새로운 구문
  (👉 **참고**: [`remark-directive`][d]가 유사하며 최신 상태입니다)
* 🟢 [`remark-contributors`](https://github.com/remarkjs/remark-contributors)
  — 기여자 표 추가
* 🟢 [`remark-copy-linked-files`](https://github.com/sergioramos/remark-copy-linked-files)
  — 링크된 파일을 찾아 대상 디렉토리로 복사
* 🟢 [`remark-corebc`](https://github.com/bchainhub/remark-corebc)
  — Core Blockchain 표기법을 마크다운 링크로 변환
* 🟢 [`remark-corepass`](https://github.com/bchainhub/remark-corepass)
  — CorePass 표기법을 마크다운 링크로 변환
* ⚠️ [`remark-custom-blocks`](https://github.com/zestedesavoir/zmarkdown/tree/HEAD/packages/remark-custom-blocks#readme)
  — 사용자 정의 블록을 위한 새로운 구문 (새로운 노드 유형, rehype 호환)
  (👉 **참고**: [`remark-directive`][d]가 유사하며 최신 상태입니다)
* 🟢 [`remark-custom-header-id`](https://github.com/sindresorhus/remark-custom-header-id)
  — 헤더에 사용자 정의 ID 속성 추가 (`{#some-id}`)
* 🟢 [`remark-definition-list`](https://github.com/wataru-chocola/remark-definition-list)
  — 정의 목록 지원
* 🟢 [`remark-defsplit`](https://github.com/remarkjs/remark-defsplit)
  — 링크와 이미지를 별도의 정의가 있는 참조로 변경
* ⚠️ [`remark-disable-tokenizers`](https://github.com/zestedesavoir/zmarkdown/tree/HEAD/packages/remark-disable-tokenizers#readme)
  — remark의 일부 또는 모든 토크나이저 켜기 또는 끄기
* 🟢 [`remark-directive`](https://github.com/remarkjs/remark-directive)
  — 지시문을 위한 새로운 구문 (일반적인 확장)
* 🟢 [`remark-directive-rehype`](https://github.com/IGassmann/remark-directive-rehype)
  — [지시문][d]을 HTML 사용자 정의 요소로 변환 (rehype 호환)
* 🟢 [`remark-docx`](https://github.com/inokawa/remark-docx)
  — 마크다운을 docx로 컴파일
* 🟢 [`remark-dropcap`](https://github.com/brev/remark-dropcap)
  — 멋지고 접근 가능한 드롭캡
* 🟢 [`remark-embed-images`](https://github.com/remarkjs/remark-embed-images)
  — 로컬 이미지를 base64로 인코딩된 데이터 URI로 임베드
* 🟢 [`remark-emoji`](https://github.com/rhysd/remark-emoji)
  — Gemoji 단축 코드를 이모지로 변환
* 🟢 [`remark-extended-table`](https://github.com/wataru-chocola/remark-extended-table)
  — colspan / rowspan을 허용하는 확장 테이블 구문
* 🟢 [`remark-extract-frontmatter`](https://github.com/mrzmmr/remark-extract-frontmatter)
  — frontmatter를 vfiles에 저장
* 🟢 [`remark-fediverse-user`](https://github.com/bchainhub/remark-fediverse-user)
  — Fediverse 사용자 표기법을 마크다운 링크로 변환
* 🟢 [`remark-first-heading`](https://github.com/laat/remark-first-heading)
  — 문서의 첫 번째 제목 변경
* 🟢 [`remark-fix-guillemets`](https://github.com/zestedesavoir/zmarkdown/tree/HEAD/packages/remark-fix-guillemets#readme)
  — ASCII 귀요메 (`<<`, `>>`)를 HTML로 매핑하는 지원
* 🟢 [`remark-flexible-code-titles`](https://github.com/ipikuka/remark-flexible-code-titles)
  — 사용자 정의 가능한 속성으로 코드 블록에 제목 또는/및 컨테이너 추가
* 🟢 [`remark-flexible-containers`](https://github.com/ipikuka/remark-flexible-containers)
  — 사용자 정의 가능한 속성으로 사용자 정의/유연한 컨테이너 추가
* 🟢 [`remark-flexible-markers`](https://github.com/ipikuka/remark-flexible-markers)
  — 사용자 정의 가능한 속성으로 사용자 정의/유연한 마크 요소 추가
* 🟢 [`remark-flexible-paragraphs`](https://github.com/ipikuka/remark-flexible-paragraphs)
  — 사용자 정의 가능한 속성으로 사용자 정의/유연한 단락 추가
* 🟢 [`remark-flexible-toc`](https://github.com/ipikuka/remark-flexible-toc)
  — 목차(toc)를 Vfile.data 또는 옵션 참조를 통해 노출
* 🟢 [`remark-frontmatter`](https://github.com/remarkjs/remark-frontmatter)
  – frontmatter 지원 (yaml, toml 등)
* 🟢 [`remark-gemoji`](https://github.com/remarkjs/remark-gemoji)
  — Gemoji 단축 코드에 대한 더 나은 지원
* ⚠️ [`remark-generic-extensions`](https://github.com/medfreeman/remark-generic-extensions)
  — CommonMark 일반 지시문 확장을 위한 새로운 구문
  (👉 **참고**: [`remark-directive`][d]가 유사하며 최신 상태입니다)
* 🟢 [`remark-gfm`](https://github.com/remarkjs/remark-gfm)
  — GFM 지원 (자동 링크 리터럴, 각주, 취소선, 표, 작업 목록)
* 🟢 [`remark-git-contributors`](https://github.com/remarkjs/remark-git-contributors)
  — Git 이력, 옵션 등을 기반으로 기여자 표 추가
* 🟢 [`remark-github`](https://github.com/remarkjs/remark-github)
  — 커밋, 이슈, 풀 리퀘스트 및 사용자에 대한 참조 자동 링크
* 🟢 [`remark-github-admonitions-to-directives`](https://github.com/incentro-dc/remark-github-admonitions-to-directives)
  — GitHub의 블록 인용 기반 경고 구문을 지시문 구문으로 변환
* 🟢 [`remark-github-beta-blockquote-admonitions`](https://github.com/myl7/remark-github-beta-blockquote-admonitions)
  — [GitHub 베타 블록 인용 기반 경고](https://github.com/github/feedback/discussions/16925)
* 🟢 [`remark-github-blockquote-alert`](https://github.com/jaywcjlove/remark-github-blockquote-alert)
  — [GitHub 경고](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#alerts) 지원을 추가하는 remark 플러그인
* ⚠️ [`remark-grid-tables`](https://github.com/zestedesavoir/zmarkdown/tree/HEAD/packages/remark-grid-tables#readme)
  — 표를 설명하기 위한 새로운 구문 (rehype 호환)
* 🟢 [`remark-heading-id`](https://github.com/imcuttle/remark-heading-id)
  — 사용자 정의 제목 id 지원 `{#custom-id}`
* 🟢 [`remark-heading-gap`](https://github.com/remarkjs/remark-heading-gap)
  — 제목 사이에 더 많은 빈 줄을 추가하여 직렬화
* 🟢 [`@vcarl/remark-headings`](https://github.com/vcarl/remark-headings)
  — 제목 목록을 데이터로 추출
* 🟢 [`remark-hexo`](https://github.com/bennycode/remark-hexo)
  — [Hexo 태그](https://hexo.io/docs/tag-plugins) 렌더링
* 🟢 [`remark-highlight.js`](https://github.com/remarkjs/remark-highlight.js)
  — [`highlight.js`](https://github.com/isagalaev/highlight.js)로 코드 블록 강조 표시
  (rehype 호환)
* 🟢 [`remark-hint`](https://github.com/sergioramos/remark-hint)
  — 마크다운에 힌트/팁/경고 추가
* 🟢 [`remark-html`](https://github.com/remarkjs/remark-html)
  — 마크다운을 HTML로 직렬화
* ⚠️ [`remark-iframes`](https://github.com/zestedesavoir/zmarkdown/tree/HEAD/packages/remark-iframes#readme)
  — iframe 생성을 위한 새로운 구문 (새로운 노드 유형, rehype 호환)
* 🟢 [`remark-ignore`](https://github.com/Xunnamius/unified-utils/blob/main/packages/remark-ignore)
  — 주석을 사용하여 변환에서 노드 제외
* 🟢 [`remark-images`](https://github.com/remarkjs/remark-images)
  — 개선된 이미지 구문 추가
* 🟢 [`remark-img-links`](https://github.com/Pondorasti/remark-img-links)
  — 상대적인 이미지 경로에 절대 URL 접두사 추가
* 🟢 [`remark-inline-links`](https://github.com/remarkjs/remark-inline-links)
  — 참조와 정의를 링크와 이미지로 변경
* 🟢 [`remark-ins`](https://github.com/ipikuka/remark-ins)
  — 삭제된 텍스트와 반대되는 삽입된 텍스트를 위한 ins 요소 추가
* 🟢 [`remark-join-cjk-lines`](https://github.com/purefun/remark-join-cjk-lines)
  — CJK 문자 사이의 추가 공백 제거
* ⚠️ [`remark-kbd`](https://github.com/zestedesavoir/zmarkdown/tree/HEAD/packages/remark-kbd#readme)
  — 키보드 키를 위한 새로운 구문 (새로운 노드 유형, rehype 호환)
* ⚠️ [`remark-kbd-plus`](https://github.com/twardoch/remark-kbd-plus)
  — 플러스가 있는 키보드 키를 위한 새로운 구문 (새로운 노드 유형, rehype 호환)
* 🟢 [`remark-license`](https://github.com/remarkjs/remark-license)
  — 라이선스 섹션 추가
* 🟢 [`remark-link-rewrite`](https://github.com/rjanjic/remark-link-rewrite)
  — 링크 URL을 동적으로 사용자 정의
* 🟢 [`remark-linkify-regex`](https://gitlab.com/staltz/remark-linkify-regex)
  — 정규식과 일치하는 텍스트를 링크로 변경
* 🟢 [`remark-lint`](https://github.com/remarkjs/remark-lint)
  — 마크다운 코드 스타일 검사
* 🟢 [`remark-macro`](https://github.com/dimerapp/remark-macro)
  — 블록 매크로 지원 (새로운 노드 유형, rehype 호환)
* 🟢 [`remark-man`](https://github.com/remarkjs/remark-man)
  — 마크다운을 man 페이지(roff)로 직렬화
* 🟢 [`remark-math`](https://github.com/remarkjs/remark-math)
  — 수학을 위한 새로운 구문 (새로운 노드 유형, rehype 호환)
* 🟢 [`remark-mdx`](https://github.com/mdx-js/mdx/tree/main/packages/remark-mdx)
  — MDX 지원 (JSX, 표현식, ESM)
* 🟢 [`remark-mentions`](https://github.com/FinnRG/remark-mentions)
  — @ 멘션을 링크로 대체
* 🟢 [`remark-mermaidjs`](https://github.com/remcohaszing/remark-mermaidjs)
  — mermaid 코드 블록을 인라인 SVG로 변환
* 🟢 [`remark-message-control`](https://github.com/remarkjs/remark-message-control)
  — 일부 또는 모든 메시지 켜기 또는 끄기
* 🟢 [`remark-normalize-headings`](https://github.com/remarkjs/remark-normalize-headings)
  — 최상위 제목이 하나만 존재하도록 보장
* 🟢 [`remark-numbered-footnote-labels`](https://github.com/jackfletch/remark-numbered-footnote-labels)
  — 각주에 숫자로 라벨 지정
* 🟢 [`@agentofuser/remark-oembed`](https://github.com/agentofuser/remark-oembed)
  — YouTube, Twitter 등의 URL을 임베드로 변환
* 🟢 [`remark-oembed`](https://github.com/sergioramos/remark-oembed)
  — 새 줄로 둘러싸인 URL을 *비동기적으로* 로드되는 임베드로 변환
* 🟢 [`remark-package-dependencies`](https://github.com/unlight/remark-package-dependencies)
  — 종속성 주입
* ⚠️ [`remark-parse-yaml`](https://github.com/landakram/remark-parse-yaml)
  — YAML 노드를 파싱하고 그 값을 `parsedValue`로 노출
* 🟢 [`remark-pdf`](https://github.com/inokawa/remark-pdf)
  — 마크다운을 pdf로 컴파일
* ⚠️ [`remark-ping`](https://github.com/zestedesavoir/zmarkdown/tree/HEAD/packages/remark-ping#readme)
  — 구성 가능한 존재 확인이 있는 멘션을 위한 새로운 구문 (새로운 노드 유형, rehype 호환)
* 🟢 [`remark-prettier`](https://github.com/remcohaszing/remark-prettier)
  — Prettier를 사용하여 마크다운 검사 및 포맷팅
* 🟢 [`remark-prism`](https://github.com/sergioramos/remark-prism)
  — [Prism](https://prismjs.com/)으로 코드 블록 강조 표시 (대부분의 Prism 플러그인 지원)
* ⚠️ [`remark-redact`](https://github.com/seafoam6/remark-redact)
  — 정규식과 일치하는 텍스트를 숨기기 위한 새로운 구문
* 🟢 [`remark-redactable`](https://github.com/code-dot-org/remark-redactable)
  — 마크다운 문서에서 내용을 편집한 다음 나중에 복원하는 플러그인 작성
* 🟢 [`remark-reference-links`](https://github.com/remarkjs/remark-reference-links)
  — 링크와 이미지를 참조와 정의로 변환
* 🟢 [`remark-rehype`](https://github.com/remarkjs/remark-rehype)
  — [rehype](https://github.com/rehypejs/rehype)로 변환
* 🟢 [`remark-relative-links`](https://github.com/zslabs/remark-relative-links)
  — 절대 URL을 상대 URL로 변경
* 🟢 [`remark-remove-comments`](https://github.com/alvinometric/remark-remove-comments)
  — 처리된 출력에서 HTML 주석 제거
* 🟢 [`remark-remove-unused-definitions`](https://github.com/Xunnamius/unified-utils/blob/main/packages/remark-remove-unused-definitions)
  — 사용되지 않은 참조 스타일 링크 정의 제거
* 🟢 [`remark-remove-url-trailing-slash`](https://github.com/Xunnamius/unified-utils/blob/main/packages/remark-remove-url-trailing-slash)
  — 모든 URL 경로 끝에서 후행 슬래시 제거
* 🟢 [`remark-renumber-references`](https://github.com/Xunnamius/unified-utils/blob/main/packages/remark-renumber-references)
  — 숫자 참조 스타일 링크 ID를 1부터 시작하여 연속적으로 다시 번호 매기기
* 🟢 [`remark-retext`](https://github.com/remarkjs/remark-retext)
  — [retext](https://github.com/retextjs/retext)로 변환
* 🟢 [`remark-ruby`](https://github.com/laysent/remark-ruby)
  — 루비(후리가나)를 위한 새로운 구문
* 🟢 [`remark-sectionize`](https://github.com/jake-low/remark-sectionize)
  — 제목과 후속 내용을 섹션 태그로 감싸기 (새로운 노드 유형, rehype 호환)
* ⚠️ [`remark-shortcodes`](https://github.com/djm/remark-shortcodes)
  — Wordpress 및 Hugo와 같은 단축 코드를 위한 새로운 구문 (새로운 노드 유형)
  (👉 **참고**: [`remark-directive`][d]가 유사하며 최신 상태입니다)
* 🟢 [`remark-simple-plantuml`](https://github.com/akebifiky/remark-simple-plantuml)
  — PlantUML 코드 블록을 이미지로 변환
* 🟢 [`remark-slate`](https://github.com/hanford/remark-slate)
  — 마크다운을 [Slate 노드](https://docs.slatejs.org/concepts/02-nodes)로 컴파일
* 🟢 [`remark-slate-transformer`](https://github.com/inokawa/remark-slate-transformer)
  — 마크다운을 [Slate 노드](https://docs.slatejs.org/concepts/02-nodes)로 컴파일하고
  Slate 노드를 마크다운으로 컴파일
* 🟢 [`remark-smartypants`](https://github.com/silvenon/remark-smartypants)
  — SmartyPants
* 🟢 [`remark-smcat`](https://github.com/shedali/remark-smcat)
  — 상태 머신 cat
* 🟢 [`remark-sort-definitions`](https://github.com/Xunnamius/unified-utils/blob/main/packages/remark-sort-definitions)
  — 참조 스타일 링크 정의 재정렬
* 🟢 [`remark-sources`](https://github.com/unlight/remark-sources)
  — 소스 코드 삽입
* 🟢 [`remark-strip-badges`](https://github.com/remarkjs/remark-strip-badges)
  — 배지 제거 (`shields.io` 등)
* 🟢 [`remark-strip-html`](https://github.com/craftzdog/remark-strip-html)
  — HTML 제거
* 🟢 [`remark-squeeze-paragraphs`](https://github.com/remarkjs/remark-squeeze-paragraphs)
  — 빈 단락 제거
* ⚠️ [`remark-sub-super`](https://github.com/zestedesavoir/zmarkdown/tree/HEAD/packages/remark-sub-super)
  — 위 첨자와 아래 첨자를 위한 새로운 구문 (새로운 노드 유형, rehype 호환)
* ⚠️ [`remark-terms`](https://github.com/Nevenall/remark-terms)
  — 특별한 용어와 구문을 위한 새로운 사용자 정의 가능한 구문
* 🟢 [`remark-textr`](https://github.com/remarkjs/remark-textr)
  — [`Textr`](https://github.com/shuvalov-anton/textr)로 텍스트 변환
* 🟢 [`remark-tight-comments`](https://github.com/Xunnamius/unified-utils/blob/main/packages/remark-tight-comments)
  — 선택적으로 주석 주변의 줄바꿈 제거
* 🟢 [`remark-title`](https://github.com/RichardLitt/remark-title)
  — 문서 제목 확인 및 추가
* 🟢 [`remark-toc`](https://github.com/remarkjs/remark-toc)
  — 목차 추가
* 🟢 [`remark-torchlight`](https://github.com/torchlight-api/remark-torchlight)
  — [torchlight.dev](https://torchlight.dev)에 의해 구동되는 구문 강조
* 🟢 [`remark-tree-sitter`](https://github.com/samlanning/remark-tree-sitter)
  — [Tree-sitter](https://tree-sitter.github.io/tree-sitter/)를 사용하여 마크다운 파일의 코드 블록 강조 표시
  (rehype 호환)
* 🟢 [`remark-truncate-links`](https://github.com/GaiAma/Coding4GaiAma/tree/HEAD/packages/remark-truncate-links)
  — 수동으로 이름 지정되지 않은 URL 잘라내기/단축하기
* 🟢 [`remark-twemoji`](https://github.com/madiodio/remark-twemoji)
  — 이모지를 [Twemoji](https://github.com/twitter/twemoji)로 변환
* 🟢 [`remark-typedoc-symbol-links`](https://github.com/kamranayub/remark-typedoc-symbol-links)
  — Typedoc 심볼 링크 표현식을 마크다운 링크로 변환
* 🟢 [`remark-typescript`](https://github.com/trevorblades/remark-typescript)
  — TypeScript 코드를 JavaScript로 변환
* 🟢 [`remark-typograf`](https://github.com/mavrin/remark-typograf)
  — [Typograf](https://github.com/typograf)로 텍스트 변환
* 🟢 [`remark-unlink`](https://github.com/remarkjs/remark-unlink)
  — 모든 링크, 참조 및 정의 제거
* 🟢 [`remark-unwrap-images`](https://github.com/remarkjs/remark-unwrap-images)
  — 이미지를 감싸고 있는 단락 제거
* 🟢 [`remark-usage`](https://github.com/remarkjs/remark-usage)
  — 사용 예제 추가
* 🟢 [`remark-utf8`](https://github.com/Swizec/remark-utf8)
  — 굵게, 기울임꼴 및 코드를 UTF 8 특수 문자로 변환
* 🟢 [`remark-validate-links`](https://github.com/remarkjs/remark-validate-links)
  — 제목과 파일에 대한 링크 확인
* ⚠️ [`remark-variables`](https://github.com/mrzmmr/remark-variables)
  — 변수를 위한 새로운 구문
* 🟢 [`remark-vdom`](https://github.com/remarkjs/remark-vdom)
  — 마크다운을 [VDOM](https://github.com/Matt-Esch/virtual-dom/)으로 컴파일
* 🟢 [`remark-wiki-link`](https://github.com/landakram/remark-wiki-link)
  — 위키 링크를 위한 새로운 구문 (rehype 호환)
* 🟢 [`remark-yaml-config`](https://github.com/remarkjs/remark-yaml-config)
  — YAML로 remark 구성

## 프리셋 목록

사용 가능하고 종종 영감을 주는 프리셋을 찾으려면 [GitHub 검색][github-preset-search]을 사용하세요.

## 유틸리티 목록

구문 트리와 함께 작동하는 유틸리티 목록은 [mdast][mdast-util]를 참조하세요.
**mdast**와 다른 구문 트리에도 작동하는 다른 유틸리티는 [unist][unist-util]를 참조하세요.
마지막으로, 가상 파일과 함께 작동하는 유틸리티 목록은 [vfile][vfile-util]을 참조하세요.

## 플러그인 사용하기

프로그래밍 방식으로 플러그인을 사용하려면 [`use()`][unified-use] 함수를 호출하세요.
`remark-cli`로 플러그인을 사용하려면 [`--use` 플래그][unified-args-use]를 전달하거나
[구성 파일][config-file-use]에 지정하세요.

## 플러그인 만들기

플러그인을 만들려면 먼저 [플러그인의 개념][unified-plugins]에 대해 읽어보세요.
그 다음, [unified로 플러그인 만들기 가이드][guide]를 읽으세요.
마지막으로, 만들려는 것과 비슷해 보이는 기존 플러그인 중 하나를 선택하고 그것을 기반으로 작업하세요.
막히는 부분이 있다면 [discussions][]에서 도움을 받을 수 있습니다.

`'remark-'` 접두사가 붙은 이름을 선택해야 합니다(예: `remark-lint`).
만든 것이 `remark().use()`와 함께 작동하지 않는다면 **`remark-` 접두사를 사용하지 마세요**: 그것은 "플러그인"이 아니며 사용자를 혼란스럽게 할 것입니다.
mdast와 함께 작동한다면 `'mdast-util-'`을, 모든 unist 트리와 함께 작동한다면 `unist-util-`을, 가상 파일과 함께 작동한다면 `vfile-`을 사용하세요.

패키지에서 플러그인을 노출할 때는 기본 내보내기를 사용하고, `package.json`에 `remark-plugin` 키워드를 추가하고, GitHub 저장소에 `remark-plugin` 토픽을 추가하세요. 그리고 이 페이지에 플러그인을 추가하기 위한 풀 리퀘스트를 만드세요!

<!--정의:-->

[logo]: https://raw.githubusercontent.com/remarkjs/remark/1f338e72/logo.svg?sanitize=true

[d]: https://github.com/remarkjs/remark-directive

[remark]: https://github.com/remarkjs/remark

[awesome-remark]: https://github.com/remarkjs/awesome-remark

[topic]: https://github.com/topics/remark-plugin

[github-preset-search]: https://github.com/topics/remark-preset

[mdast-util]: https://github.com/syntax-tree/mdast#list-of-utilities

[unist-util]: https://github.com/syntax-tree/unist#unist-utilities

[vfile-util]: https://github.com/vfile/vfile#utilities

[unified-use]: https://github.com/unifiedjs/unified#processoruseplugin-options

[unified-args-use]: https://github.com/unifiedjs/unified-args#--use-plugin

[config-file-use]: https://github.com/unifiedjs/unified-engine/blob/HEAD/doc/configure.md#plugins

[unified-plugins]: https://github.com/unifiedjs/unified#plugin

[guide]: https://unifiedjs.com/learn/guide/create-a-plugin/

[discussions]: https://github.com/remarkjs/remark/discussions

[rehype-plugins]: https://github.com/rehypejs/rehype/blob/main/doc/plugins.md#list-of-plugins

이 문서는 remark 플러그인들의 포괄적인 목록을 제공하며, 각 플러그인의 간단한 설명과 함께 그 상태(최신 상태인지 또는 현재 작동하지 않는지)를 표시합니다. 또한 플러그인을 사용하는 방법과 새로운 플러그인을 만드는 방법에 대한 지침도 포함되어 있습니다.

remark 생태계는 계속 발전하고 있으며, 이 목록은 마크다운 처리를 위한 다양한 도구와 기능을 제공하는 풍부한 플러그인 생태계를 보여줍니다. 개발자들은 이 목록을 참조하여 자신의 프로젝트에 필요한 기능을 찾거나, 새로운 플러그인 개발에 대한 아이디어를 얻을 수 있습니다.