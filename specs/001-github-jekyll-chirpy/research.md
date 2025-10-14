# 연구 문서: GitHub 기반 테크 블로그

**프로젝트**: 001-github-jekyll-chirpy
**날짜**: 2025-10-15
**목적**: Jekyll + Chirpy 테마 기반 블로그 구축을 위한 기술 조사 및 모범 사례 연구

---

## 1. Chirpy Starter 템플릿 구조

### 결정: Chirpy Starter 사용

**선택한 방법**: [chirpy-starter](https://github.com/cotes2020/chirpy-starter) 템플릿 저장소 사용

**근거**:
- **간소화된 설정**: 전체 테마 저장소를 fork하는 대신 starter 템플릿 사용으로 불필요한 파일 제거
- **빠른 시작**: 템플릿에서 "Use this template" 버튼으로 즉시 저장소 생성 가능
- **업데이트 용이**: 테마 업데이트 시 gem 방식으로 간편하게 업데이트 (fork 방식보다 merge 충돌 적음)
- **유지보수성**: 사용자 정의 파일만 관리하면 되므로 프로젝트 구조가 깔끔함

**템플릿 사용 방법**:
1. https://github.com/cotes2020/chirpy-starter 접속
2. "Use this template" > "Create a new repository" 클릭
3. 저장소 이름을 `USERNAME.github.io` 형식으로 설정 (개인 블로그)
4. Public으로 설정 (GitHub Pages 무료 호스팅)
5. 로컬에 클론: `git clone https://github.com/USERNAME/USERNAME.github.io.git`

**필수 설정 항목** (_config.yml):
```yaml
lang: ko-KR                    # 언어 설정
timezone: Asia/Seoul           # 타임존
title: "블로그 제목"            # 사이트 제목
tagline: "블로그 설명"         # 서브타이틀
description: "SEO용 설명"      # 메타 설명
url: "https://USERNAME.github.io"  # 사이트 URL
github:
  username: USERNAME           # GitHub 사용자명
social:
  name: "이름"                 # 작성자 이름
  email: email@example.com     # 이메일
  links:                       # 소셜 링크
    - https://github.com/USERNAME
```

**대안으로 고려했지만 거부한 방법**:
- **전체 테마 fork**: 테마의 모든 파일을 포함하여 저장소가 무겁고 업데이트 시 merge 충돌 발생 가능
- **처음부터 Jekyll 구축**: 테마 없이 시작하면 디자인, JavaScript, 검색 등 모든 기능을 직접 구현해야 함
- **다른 Jekyll 테마**: Chirpy는 다크모드, 검색, TOC, 반응형 디자인이 모두 내장되어 있어 요구사항에 가장 적합

---

## 2. Jekyll Front Matter 스키마

### 결정: YAML Front Matter with Chirpy Extensions

**Front Matter 구조**:

```yaml
---
title: "포스트 제목"              # 필수
date: YYYY-MM-DD HH:MM:SS +0900  # 필수 (타임존 포함)
categories: [Category1, Category2]  # 선택 (최대 2개 권장)
tags: [tag1, tag2, tag3]          # 선택 (제한 없음)
author: <author_id>               # 선택 (기본값: _data/authors.yml)
description: "포스트 요약"        # 선택 (SEO용, 150-160자)
image:                            # 선택 (소셜 미디어 공유 이미지)
  path: /assets/img/posts/image.jpg
  alt: "이미지 설명"
pin: false                        # 선택 (홈페이지 상단 고정)
math: true                        # 선택 (수학 수식 렌더링)
mermaid: true                     # 선택 (다이어그램 렌더링)
toc: true                         # 선택 (목차 표시, 기본값 true)
comments: true                    # 선택 (댓글 표시, 기본값 true)
---
```

**필수 필드 검증**:
- `title`: 문자열, 비어있지 않아야 함
- `date`: `YYYY-MM-DD HH:MM:SS +HHMM` 형식 (Jekyll은 자동으로 파싱)

**선택 필드 가이드**:
- `categories`: 배열, 계층 구조 지원 (첫 번째가 상위 카테고리)
- `tags`: 배열, 소문자 권장 (URL 생성 시 소문자로 변환)
- `description`: SEO를 위해 150-160자 권장
- `image`: Open Graph/Twitter Card 메타 태그 생성
- `pin`: true 설정 시 홈페이지 최상단에 고정 표시

**파일명 규칙**:
- 형식: `YYYY-MM-DD-title-in-lowercase.md`
- 예시: `2025-10-15-my-first-post.md`
- 주의: 파일명의 날짜와 Front Matter의 date가 일치해야 함 (파일명이 우선)

**근거**:
- Jekyll 표준 Front Matter를 따르면서 Chirpy 테마의 확장 필드 활용
- YAML 형식은 가독성이 높고 다양한 데이터 타입 지원
- Chirpy의 기능(TOC, 수식, 다이어그램)을 플래그로 간편하게 제어

**대안 거부 이유**:
- **JSON Front Matter**: Jekyll이 기본 지원하지 않고 가독성이 낮음
- **커스텀 메타데이터 형식**: Jekyll/Chirpy 생태계 표준을 벗어나면 호환성 문제 발생

---

## 3. GitHub Actions 워크플로우 설정

### 결정: GitHub Pages Jekyll Workflow 사용

**워크플로우 파일**: `.github/workflows/pages-deploy.yml`

```yaml
name: "Build and Deploy"
on:
  push:
    branches:
      - main
      - master
    paths-ignore:
      - .gitignore
      - README.md
      - LICENSE
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v4

      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 3.2
          bundler-cache: true

      - name: Build site
        run: bundle exec jekyll b -d "_site${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production

      - name: Test site
        run: |
          bundle exec htmlproofer _site \
            --disable-external \
            --ignore-urls "/^http:\/\/127.0.0.1/,/^http:\/\/0.0.0.0/,/^http:\/\/localhost/"

      - name: Upload site artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: "_site${{ steps.pages.outputs.base_path }}"

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

**트리거 조건**:
- `push` to `main` or `master` 브랜치
- README, LICENSE 같은 문서 변경은 빌드하지 않음 (최적화)
- `workflow_dispatch`: 수동으로도 워크플로우 실행 가능

**빌드 프로세스**:
1. **Checkout**: 저장소 전체 히스토리 가져오기 (last modified date 계산용)
2. **Setup Ruby**: Ruby 3.2 설치 및 Bundler 캐시
3. **Build**: Jekyll 프로덕션 빌드 (`JEKYLL_ENV=production`)
4. **Test**: HTMLProofer로 링크 및 HTML 검증
5. **Upload**: 빌드 결과물을 GitHub Pages 아티팩트로 업로드
6. **Deploy**: GitHub Pages 환경에 배포

**권한 설정**:
- `contents: read`: 저장소 읽기
- `pages: write`: GitHub Pages 쓰기
- `id-token: write`: OIDC 토큰 (GitHub Pages 배포용)

**근거**:
- GitHub의 공식 Jekyll + Pages 워크플로우 사용으로 신뢰성 확보
- HTMLProofer 통합으로 빌드 시 링크 오류 조기 발견
- Bundler 캐시로 빌드 시간 단축 (의존성 재설치 불필요)
- Concurrency 설정으로 동시 배포 방지 (최신 커밋만 배포)

**GitHub Pages 설정 (저장소 Settings)**:
1. Settings > Pages로 이동
2. Source: "GitHub Actions" 선택 (기존 Branch 방식 대신)
3. 첫 워크플로우 실행 후 사이트 URL 자동 생성

**대안 거부 이유**:
- **Branch 기반 배포** (gh-pages): GitHub Actions가 더 유연하고 빌드 과정 제어 가능
- **Travis CI/CircleCI**: GitHub Actions가 GitHub와 네이티브 통합되어 설정 간단
- **수동 빌드**: 자동화가 핵심 요구사항 (SC-002: 2분 이내 자동 배포)

---

## 4. Chirpy 테마 커스터마이징

### 결정: _config.yml 기반 커스터마이징 우선

**주요 설정 옵션** (_config.yml):

```yaml
# 테마 설정
theme: jekyll-theme-chirpy
theme_mode: # [light | dark] 기본 테마 (비워두면 시스템 설정 따름)

# 사이드바 아바타
avatar: /assets/img/avatar.jpg

# TOC (Table of Contents)
toc: true  # 포스트에 목차 표시 (포스트별로 override 가능)

# 댓글 시스템 (선택)
comments:
  active: giscus  # [disqus | utterances | giscus]
  giscus:
    repo: USERNAME/REPO
    repo_id: REPO_ID
    category: Announcements
    category_id: CATEGORY_ID

# Google Analytics
google_analytics:
  id: G-XXXXXXXXXX

# PWA (Progressive Web App)
pwa:
  enabled: true
  cache:
    enabled: true

# Pagination
paginate: 10
```

**테마 색상/폰트 커스터마이징**:

파일: `_sass/addon/variables.scss` (필요시 생성)

```scss
// 라이트 모드 색상
$body-bg: #ffffff;
$text-color: #2c2c2c;
$link-color: #0066cc;

// 다크 모드 색상
$body-bg-dark: #1e1e1e;
$text-color-dark: #e0e0e0;
$link-color-dark: #4da6ff;

// 폰트
$font-family: 'Noto Sans KR', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
```

**CSS 추가 커스터마이징**: `assets/css/style.scss` (필요시 생성)

```scss
---
---

@import "main";

// 커스텀 스타일 추가
.post-content {
  // 포스트 본문 스타일 조정
}
```

**근거**:
- _config.yml만으로 대부분의 커스터마이징 가능 (테마 업데이트 시 호환성 유지)
- SCSS 변수 override로 색상/폰트 변경 (테마 파일 직접 수정 불필요)
- 최소한의 커스터마이징으로 테마의 장점(반응형, 접근성) 유지

**한글 폰트 최적화**:
- Google Fonts에서 Noto Sans KR 로드
- `_includes/head.html` override (필요시):

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700&display=swap" rel="stylesheet">
```

**대안 거부 이유**:
- **전체 테마 파일 수정**: 테마 업데이트 시 변경사항 유지 어려움
- **완전 커스텀 디자인**: 요구사항(간단한 설정)과 맞지 않고 개발 시간 증가

---

## 5. 한글 콘텐츠 지원

### 결정: Jekyll 기본 URL 인코딩 + 영문 슬러그 권장

**URL 인코딩 전략**:

Jekyll은 한글을 자동으로 URL 인코딩하지만, 가독성과 SEO를 위해 **영문 슬러그 사용 권장**:

**방법 1: 포스트 Front Matter에 slug 지정** (권장)
```yaml
---
title: "Jekyll 블로그 만들기"
categories: [개발, Tutorial]
tags: [jekyll, 블로그]
slug: jekyll-blog-tutorial  # 영문 슬러그
---
```
→ URL: `/posts/jekyll-blog-tutorial/`

**방법 2: 한글 그대로 사용**
```yaml
---
title: "Jekyll 블로그 만들기"
categories: [개발, Tutorial]
---
```
→ URL: `/posts/jekyll-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0/` (자동 인코딩)

**카테고리/태그 한글 처리**:

Jekyll과 Chirpy는 한글 카테고리/태그를 자동으로 처리:
- 표시: 한글 그대로 표시
- URL: 자동 인코딩 (`/categories/%EA%B0%9C%EB%B0%9C/`)
- 필터링: 정상 작동

**_config.yml 설정**:
```yaml
lang: ko-KR
timezone: Asia/Seoul
```

**한글 SEO 최적화**:

1. **메타 태그 최적화** (Chirpy 자동 생성):
```html
<meta property="og:title" content="포스트 제목">
<meta property="og:description" content="한글 설명">
<meta name="description" content="한글 설명">
```

2. **sitemap.xml 자동 생성**:
- Jekyll이 자동 생성 (한글 URL 포함)
- Google Search Console에 제출

3. **robots.txt**:
```
User-agent: *
Allow: /
Sitemap: https://USERNAME.github.io/sitemap.xml
```

**검색 엔진 최적화**:
- Chirpy는 한글 키워드 검색을 지원하는 클라이언트 사이드 검색 내장
- 검색 인덱스는 빌드 시 자동 생성 (한글 포함)

**근거**:
- Jekyll/Chirpy가 한글을 네이티브 지원하므로 추가 설정 최소화
- 영문 슬러그 사용으로 URL 가독성 향상 및 공유 용이
- SEO 메타 태그는 한글 사용 가능 (Google/Naver 모두 지원)

**대안 거부 이유**:
- **Transliteration (한글→영문 자동 변환)**: Jekyll 플러그인 필요하나 GitHub Pages에서 미지원
- **모든 URL을 영문으로 강제**: 한글 카테고리/태그의 자연스러움 손실

---

## 6. 추가 연구: 성능 최적화

### 결정: Jekyll 빌드 최적화 + CDN 활용

**Jekyll 빌드 최적화**:

1. **Incremental build** (_config.yml):
```yaml
incremental: true  # 변경된 파일만 재빌드 (개발 시)
```

2. **Liquid profiling**:
```bash
bundle exec jekyll build --profile
```
→ 느린 템플릿 식별 및 최적화

3. **이미지 최적화**:
- WebP 형식 사용 권장 (용량 30-50% 감소)
- 이미지 크기 조정: 블로그는 최대 1200px 너비면 충분
- 도구: ImageOptim, TinyPNG

**CDN 활용** (선택 사항):

Chirpy 테마는 외부 리소스를 CDN에서 로드:
- jQuery: jsDelivr CDN
- Bootstrap Icons: CDN
- 커스텀 이미지: GitHub Pages 또는 외부 CDN

**성능 목표 달성 전략**:
- **SC-004** (홈페이지 3초 이내): Pagination으로 페이지당 10개 포스트만 로드
- **SC-005** (검색 0.5초): 클라이언트 사이드 검색 (Simple-Jekyll-Search 라이브러리)
- **SC-006** (테마 전환 0.3초): CSS 변수 + JavaScript toggle (Chirpy 내장)

**대안 거부 이유**:
- **Server-side rendering**: GitHub Pages는 정적 호스팅만 지원
- **복잡한 캐싱 전략**: 정적 사이트는 브라우저 캐싱으로 충분

---

## 요약

| 항목 | 선택 | 근거 |
|------|------|------|
| **초기 설정** | Chirpy Starter 템플릿 | 간소화된 구조, 빠른 시작, 업데이트 용이 |
| **포스트 형식** | YAML Front Matter | Jekyll 표준, Chirpy 확장 필드 활용 |
| **CI/CD** | GitHub Actions | 네이티브 통합, 자동화, HTMLProofer 검증 |
| **커스터마이징** | _config.yml + SCSS 변수 | 최소 수정, 테마 업데이트 호환성 |
| **한글 지원** | 영문 슬러그 + 자동 인코딩 | SEO 최적화, URL 가독성 |
| **성능** | Pagination + CDN | 목표 달성, 추가 설정 최소 |

**다음 단계**: Phase 1 설계 문서 생성 (data-model.md, contracts/, quickstart.md)
