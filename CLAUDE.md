# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

`slimpumpkin.com` 도메인으로 서비스되는 GitHub Pages 정적 사이트입니다. 모바일 앱 SlimPumpkin (마인드풀 식사·영양소 섭취 순서 훈련 기반 체중 감량 앱)의 단일 페이지 "Coming Soon" 랜딩 페이지 한 장으로 구성되어 있습니다.

- 빌드 시스템 없음 (no `package.json`, no bundler, no linter, no tests).
- `index.html` 한 파일을 직접 편집해서 GitHub Pages가 그대로 서빙합니다.
- `CNAME` 파일이 GitHub Pages의 사용자 지정 도메인(`slimpumpkin.com`) 매핑을 담당합니다 — 도메인 변경 시 이 파일을 수정해야 합니다.

## Local preview

별도 빌드 단계가 없으므로 정적 서버로 띄워서 확인합니다 (Tailwind CDN과 Google Fonts가 CORS·CDN을 위해 `http(s)://`를 요구하므로 `file://`로 직접 열면 일부 폰트/스타일이 깨질 수 있음):

```bash
python3 -m http.server 8000
# → http://localhost:8000
```

## Architecture / styling notes

- **Tailwind는 CDN 런타임 모드**로 로드됩니다 (`https://cdn.tailwindcss.com?plugins=forms,container-queries`). 이는 브라우저에서 JIT 컴파일하므로 별도 빌드가 필요 없는 대신, 프로덕션 권장 방식은 아닙니다. 클래스 추가는 즉시 반영되지만 `@apply`나 커스텀 디렉티브는 사용 불가입니다.
- **Tailwind theme 확장**은 `<script id="tailwind-config">` 인라인 블록(`index.html:26-48`)에서 정의됩니다. 디자인 토큰을 바꿀 때 이 블록을 직접 수정합니다:
  - `primary: #ec5b13` (오렌지 — 브랜드 컬러)
  - `background-light: #f8f6f6`, `background-dark: #221610`
  - `fontFamily.display: ["Noto Sans KR", "Public Sans", "sans-serif"]`
- **다크 모드**는 `darkMode: "class"` 전략입니다. `<html>` 또는 상위 요소에 `class="dark"`를 추가하면 토글되지만 현재는 토글 UI가 없습니다.
- **`bg-mesh` 커스텀 클래스**(`index.html:54-64`)는 라이트/다크 양쪽에서 radial-gradient 배경을 만듭니다. Tailwind config 밖에 있는 유일한 CSS이므로 새 커스텀 스타일은 같은 `<style>` 블록에 추가합니다.
- **Material Symbols Outlined 폰트**가 두 번 중복 로드되어 있습니다(`index.html:14-19`) — 편집 시 한쪽만 남겨도 무방합니다.

## Assets

- `favicon/` 디렉터리에 다양한 해상도의 favicon과 PWA 매니페스트(`site.webmanifest`)가 있습니다. 매니페스트의 앱 이름·테마 컬러를 바꾸려면 이 파일을 직접 편집합니다.
- 메인 시각 이미지는 외부 호스팅된 Google `lh3.googleusercontent.com` URL을 직접 참조합니다(`index.html:103`). 자체 호스팅으로 옮기려면 이미지를 저장소에 추가하고 경로를 상대 경로로 교체합니다.

## Content language

본문은 한국어, `<html lang="en">`로 표기되어 있습니다 (`index.html:3`). 추가 번역 가능성이 있다면 `lang` 속성을 `ko`로 바꾸는 게 정확합니다.

## Deploy

`main` 브랜치에 푸시하면 GitHub Pages가 자동 배포합니다. 별도 CI/CD나 빌드 스텝이 없으므로 커밋이 곧 배포입니다 — 깨진 HTML이 바로 라이브에 올라가니 푸시 전 로컬 미리보기로 확인하는 것이 좋습니다.
