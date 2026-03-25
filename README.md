Summary of work and next steps for the “의사가 배우는 AI” site

## 작업 개요
- source 문서는 frontmatter 기반으로 유지하며 각 페이지(`00`, `01`, `11~15`)에는 canonical/OG/twitter/meta/로고/푸터가 정상적으로 들어가 있습니다.
- `home.html`(공개 랜딩)과 `index.html`(내부 인덱스)는 Node생성기(`scripts/build-s-reborn-pages.js`)를 통해 자동 생성되며, 풍부한 메타 및 스타일을 JSON + 템플릿으로 채웁니다.
- generator는 CSS 규칙을 배열화한 뒤 `prettyHtml()`로 전체 markup을 들여쓰기하고, `thumb` 영역을 flex(column)으로 바꾸며 배지가 겹치지 않도록 정리했습니다.
- build 스크립트는 PowerShell 래퍼(`build-s-reborn-home.ps1`, `build-s-reborn-index.ps1`)가 `node`를 호출해 콘텐츠 폴더를 세팅하고 다시 생성합니다.

## 진행 로그
1. 처음에는 PowerShell 템플릿이 UTF-8 인코딩 관련 문제로 `home/index`에 한글이 깨졌습니다 → Node 기반 생성기로 교체해서 인코딩 안정화.
2. CSS/HTML이 한 줄로 뭉쳐 읽기 어려웠기에 `prettyHtml()` + `css()` 헬퍼로 `<style>`/markup을 들여쓰기된 형태로 출력하도록 개선.
3. `thumb` 내부 요소의 flex 구조와 `small`/`strong` block 처리로 “Series/Intro” 배지가 실제 카드에서 겹치지 않게 정리.
4. 이 모든 변경을 반영해서 `home.html`, `index.html`을 다시 생성했고, 현재 상태가 바로 보여지는 출력물로 존재합니다.

## 향후 작업
1. 카드 썸네일 내 문장(viable `small`/`strong`)에서 텍스트 길이라도 늘어나면 `card()` 템플릿에서 `thumb` 내 label 스타일을 조정하고 다시 빌드할 것.
2. `category-row`는 지금 `flex-wrap:nowrap`+`overflow-x:auto`로 한 줄에 고정되므로, 라벨 수가 많아지면 CSS 패딩/마진을 조절하거나 스크롤 UX 변화를 살피면서 재생성할 것.
3. generator용 CSS 배열에 추가/수정할 때는 `css()` 함수 배열에 규칙 문장을 추가한 뒤 `node` 호출로 재생성할 것.
4. 새로운 문서를 추가하거나 기존 frontmatter를 바꾸는 경우 `.\scripts\build-s-reborn-home.ps1` 및 `.\scripts\build-s-reborn-index.ps1`을 재실행해서 출력물을 갱신할 것.

## 전달용 프롬프트 (다음 주자가 이어갈 때)
```
현재 `s-reborn-world-doctor-ai` 폴더 안의 `home.html`(공개 홈)과 `index.html`(내부 인덱스)은
`scripts/build-s-reborn-pages.js`를 통해 Node에서 생성됩니다. CSS 규칙은 배열(`css()`)로
관리하고, 반복문 `card()`에서 thumb/배지 레이아웃을 조절합니다. 새로운 문서나 스타일 변경이
생기면 PowerShell 래퍼(`build-s-reborn-home.ps1`, `build-s-reborn-index.ps1`)로 node 생성기를
다시 호출하세요. 터미널에서 `node scripts/build-s-reborn-pages.js --mode home` 또는 `--mode index`
를 먼저 실행해도 됩니다.  ```
