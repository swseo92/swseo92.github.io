# 데이터 모델: GitHub 기반 테크 블로그

**프로젝트**: 001-github-jekyll-chirpy
**날짜**: 2025-10-15
**기반**: [spec.md](./spec.md) 주요 엔티티 섹션

---

## 개요

Jekyll 블로그의 데이터는 파일 시스템 기반으로 구조화됩니다. 데이터베이스가 아닌 Markdown 파일, YAML 설정 파일, 데이터 파일로 관리되며, Jekyll 빌드 시 정적 HTML로 변환됩니다.

---

## 1. Post (포스트)

### 설명
블로그의 핵심 콘텐츠 단위. 기술 글, 튜토리얼, 개발 경험 등을 담는 문서.

### 저장 위치
`_posts/YYYY-MM-DD-title.md`

### 구조

#### Front Matter (YAML)
```yaml
---
title: string                    # 필수: 포스트 제목
date: datetime                   # 필수: YYYY-MM-DD HH:MM:SS +HHMM
categories: [string, string]     # 선택: 최대 2개 권장 (계층 구조)
tags: [string, ...]              # 선택: 제한 없음 (키워드)
author: string                   # 선택: 작성자 ID (_data/authors.yml 참조)
description: string              # 선택: SEO용 요약 (150-160자 권장)
image:                           # 선택: 대표 이미지
  path: string                   # 이미지 경로
  alt: string                    # 대체 텍스트
pin: boolean                     # 선택: 홈페이지 상단 고정 (기본값: false)
math: boolean                    # 선택: 수학 수식 렌더링 (기본값: false)
mermaid: boolean                 # 선택: 다이어그램 렌더링 (기본값: false)
toc: boolean                     # 선택: 목차 표시 (기본값: true)
comments: boolean                # 선택: 댓글 표시 (기본값: true)
slug: string                     # 선택: URL 슬러그 (기본값: 파일명에서 추출)
---
```

#### Content (Markdown)
```markdown
# 포스트 본문

Markdown 형식으로 작성된 콘텐츠.

## 섹션 제목

본문 내용...

```python
# 코드 블록
def example():
    pass
\```

![이미지 설명](/assets/img/posts/image.jpg)
```

### 필드 상세

| 필드 | 타입 | 필수 | 설명 | 검증 규칙 |
|------|------|------|------|----------|
| `title` | string | 필수 | 포스트 제목 | 비어있지 않아야 함 |
| `date` | datetime | 필수 | 발행 날짜/시간 | ISO 8601 형식 + 타임존 |
| `categories` | array<string> | 선택 | 카테고리 목록 | 최대 2개 권장 |
| `tags` | array<string> | 선택 | 태그 목록 | 소문자 권장 |
| `author` | string | 선택 | 작성자 ID | `_data/authors.yml`에 정의되어야 함 |
| `description` | string | 선택 | 포스트 요약 | 150-160자 권장 (SEO) |
| `image.path` | string | 선택 | 이미지 경로 | 유효한 파일 경로 |
| `image.alt` | string | 선택 | 이미지 대체 텍스트 | 접근성 |
| `pin` | boolean | 선택 | 고정 여부 | true/false |
| `math` | boolean | 선택 | 수식 렌더링 | true/false |
| `mermaid` | boolean | 선택 | 다이어그램 | true/false |
| `toc` | boolean | 선택 | 목차 표시 | true/false |
| `comments` | boolean | 선택 | 댓글 표시 | true/false |
| `slug` | string | 선택 | URL 슬러그 | 영문 소문자, 하이픈만 허용 |

### URL 생성 규칙
- 기본: `/posts/{slug}/` (slug는 파일명 또는 Front Matter의 slug 값)
- 예: `2025-10-15-my-first-post.md` → `/posts/my-first-post/`
- slug 지정 시: `slug: custom-url` → `/posts/custom-url/`

### 관계
- **1:N** → Category (포스트는 여러 카테고리에 속할 수 있음)
- **1:N** → Tag (포스트는 여러 태그를 가질 수 있음)
- **N:1** → Author (포스트는 하나의 작성자를 가짐)

---

## 2. Category (카테고리)

### 설명
포스트를 주제별로 분류하는 계층적 구조. 카테고리는 명시적으로 정의하지 않고, 포스트의 Front Matter에서 참조되면 자동 생성됩니다.

### 저장 방식
- 파일: 없음 (자동 생성)
- 페이지: `_tabs/categories.md` (카테고리 목록 페이지)
- 개별 페이지: `/categories/{category-name}/` (Jekyll이 자동 생성)

### 구조

```yaml
name: string           # 카테고리 이름 (포스트의 categories 배열에서 추출)
slug: string           # URL 슬러그 (자동 생성)
post_count: integer    # 이 카테고리의 포스트 개수 (동적 계산)
```

### 계층 구조
- 포스트의 `categories: [Parent, Child]`에서 첫 번째가 상위, 두 번째가 하위
- 예: `categories: [Backend, Python]`
  - Backend (상위 카테고리)
    - Python (하위 카테고리)

### URL 생성 규칙
- 상위: `/categories/backend/`
- 하위: `/categories/python/`
- Jekyll은 계층을 URL에 직접 반영하지 않지만, 표시는 계층적으로 가능

### 필터링
- 카테고리 페이지에서 해당 카테고리를 포함한 모든 포스트 목록 표시
- Liquid 템플릿 필터: `{% for post in site.categories['Backend'] %}`

### 관계
- **N:1** → Post (여러 포스트가 하나의 카테고리에 속함)

---

## 3. Tag (태그)

### 설명
포스트를 키워드 기반으로 분류하는 비계층적 레이블. 카테고리보다 세분화된 분류.

### 저장 방식
- 파일: 없음 (자동 생성)
- 페이지: `_tabs/tags.md` (태그 목록 페이지)
- 개별 페이지: `/tags/{tag-name}/` (Jekyll이 자동 생성)

### 구조

```yaml
name: string           # 태그 이름 (포스트의 tags 배열에서 추출)
slug: string           # URL 슬러그 (자동 생성)
post_count: integer    # 이 태그의 포스트 개수 (동적 계산)
```

### 명명 규칙
- 소문자 권장 (예: `python`, `api`, `rest`)
- 하이픈 사용 (예: `web-development`)
- 일관성 유지 (예: `JavaScript` vs `javascript` 혼용 금지)

### URL 생성 규칙
- `/tags/{tag-name}/`
- 예: `tags: [python, django]` → `/tags/python/`, `/tags/django/`

### 필터링
- 태그 페이지에서 해당 태그를 포함한 모든 포스트 목록 표시
- Liquid 템플릿 필터: `{% for post in site.tags['python'] %}`

### 관계
- **N:1** → Post (여러 포스트가 하나의 태그를 가짐)

---

## 4. Blog Configuration (블로그 설정)

### 설명
블로그의 전역 설정 정보. 사이트 메타데이터, 작성자 정보, 테마 설정 등을 포함.

### 저장 위치
`_config.yml`

### 구조

```yaml
# 사이트 기본 정보
lang: string                      # 언어 코드 (예: ko-KR, en)
timezone: string                  # 타임존 (예: Asia/Seoul)
title: string                     # 사이트 제목
tagline: string                   # 서브타이틀
description: string               # 사이트 설명 (SEO)
url: string                       # 베이스 URL (https://username.github.io)
baseurl: string                   # 서브 경로 (선택, 보통 비워둠)

# 작성자 정보
social:
  name: string                    # 작성자 이름
  email: string                   # 이메일
  links:                          # 소셜 링크
    - string                      # GitHub, LinkedIn 등

# GitHub 정보
github:
  username: string                # GitHub 사용자명

# 테마 설정
theme: string                     # jekyll-theme-chirpy
theme_mode: string                # [light | dark] (선택)

# 아바타
avatar: string                    # 아바타 이미지 경로

# 댓글 시스템
comments:
  active: string                  # [disqus | utterances | giscus]
  # 각 댓글 시스템별 설정 (선택)

# Google Analytics
google_analytics:
  id: string                      # GA Tracking ID (선택)

# PWA
pwa:
  enabled: boolean                # PWA 활성화 여부
  cache:
    enabled: boolean              # 캐시 활성화

# 페이지네이션
paginate: integer                 # 페이지당 포스트 개수 (기본: 10)

# TOC (Table of Contents)
toc: boolean                      # 기본 TOC 표시 여부
```

### 주요 필드

| 필드 | 타입 | 필수 | 설명 | 예시 |
|------|------|------|------|------|
| `lang` | string | 필수 | 언어 코드 | `ko-KR` |
| `timezone` | string | 필수 | 타임존 | `Asia/Seoul` |
| `title` | string | 필수 | 사이트 제목 | `나의 기술 블로그` |
| `tagline` | string | 선택 | 서브타이틀 | `개발 여정 기록` |
| `description` | string | 필수 | SEO 설명 | `개발 관련 글을 작성합니다` |
| `url` | string | 필수 | 베이스 URL | `https://username.github.io` |
| `social.name` | string | 필수 | 작성자 이름 | `홍길동` |
| `social.email` | string | 필수 | 이메일 | `user@example.com` |
| `github.username` | string | 필수 | GitHub 사용자명 | `username` |
| `avatar` | string | 선택 | 아바타 경로 | `/assets/img/avatar.jpg` |
| `paginate` | integer | 선택 | 페이지당 포스트 수 | `10` |

### 검증 규칙
- `url`: 프로토콜 포함 (http:// 또는 https://)
- `timezone`: IANA 타임존 데이터베이스 형식
- `paginate`: 양의 정수

### 관계
- 전역 설정이므로 다른 엔티티와 직접 관계 없음
- Post, Page에서 이 설정 값 참조

---

## 5. Page (페이지)

### 설명
포스트가 아닌 정적 콘텐츠 페이지. About, Archives, Categories, Tags 등 고정 페이지.

### 저장 위치
`_tabs/{page-name}.md`

### 구조

#### Front Matter (YAML)
```yaml
---
layout: string         # 레이아웃 (page, archives 등)
icon: string           # 아이콘 클래스 (Font Awesome)
order: integer         # 사이드바 정렬 순서
title: string          # 페이지 제목 (선택, layout이 결정)
---
```

#### Content (Markdown)
```markdown
# 페이지 내용

Markdown 형식의 정적 콘텐츠.
```

### 기본 페이지

| 파일 | Layout | Icon | Order | 설명 |
|------|--------|------|-------|------|
| `about.md` | `page` | `fas fa-info-circle` | 4 | 작성자 소개 |
| `archives.md` | `archives` | `fas fa-archive` | 3 | 전체 포스트 아카이브 |
| `categories.md` | `categories` | `fas fa-stream` | 1 | 카테고리 목록 |
| `tags.md` | `tags` | `fas fa-tag` | 2 | 태그 목록 |

### URL 생성 규칙
- `/about/`
- `/archives/`
- `/categories/`
- `/tags/`

### 커스터마이징
- About 페이지는 자유롭게 Markdown 작성 가능
- 다른 페이지(Archives, Categories, Tags)는 레이아웃이 자동 생성

### 관계
- 독립적 (다른 엔티티와 직접 관계 없음)

---

## 6. Author (작성자)

### 설명
포스트 작성자 정보. 여러 작성자가 블로그를 운영하는 경우 사용.

### 저장 위치
`_data/authors.yml`

### 구조

```yaml
author_id:
  name: string          # 작성자 이름
  twitter: string       # 트위터 핸들 (선택)
  url: string           # 개인 URL (선택)

# 예시
cotes:
  name: Cotes Chung
  twitter: cotes2020
  url: https://github.com/cotes2020
```

### 사용 방법
- 포스트 Front Matter에서 `author: author_id` 형식으로 참조
- 미지정 시 _config.yml의 `social.name` 사용

### 관계
- **1:N** → Post (하나의 작성자가 여러 포스트를 작성)

---

## 데이터 흐름

```
_config.yml → Jekyll Build → 정적 HTML
_posts/*.md → Jekyll Build → /posts/{slug}/index.html
_tabs/*.md → Jekyll Build → /{page}/index.html
_data/authors.yml → 포스트에서 참조
```

### 빌드 프로세스
1. Jekyll이 `_config.yml` 읽어 전역 설정 로드
2. `_posts/` 디렉토리의 모든 Markdown 파일 파싱
3. Front Matter 추출 및 검증
4. Markdown 본문을 HTML로 변환
5. Liquid 템플릿 적용 (레이아웃, 카테고리/태그 페이지 생성)
6. `_site/` 디렉토리에 정적 HTML 출력

### 카테고리/태그 자동 생성
- Jekyll은 모든 포스트의 Front Matter를 스캔
- 사용된 카테고리/태그 자동 수집
- 각각에 대한 필터링 페이지 자동 생성

---

## 검증 및 제약사항

### 파일명 검증
- 포스트: `YYYY-MM-DD-{slug}.md` 형식 필수
- 날짜 불일치 시 Jekyll 경고 (파일명 날짜가 우선)

### Front Matter 검증
- `title`: 비어있으면 빌드 실패
- `date`: 형식 오류 시 빌드 실패
- `categories`, `tags`: YAML 배열 형식 필수

### 이미지 경로 검증
- `assets/img/` 하위에 저장 권장
- 절대 경로 또는 상대 경로 사용 가능
- 존재하지 않는 이미지: 빌드는 성공하지만 브라우저에서 404

### 성능 제약사항
- 포스트 개수: 50-200개 권장 (더 많으면 빌드 시간 증가)
- 이미지 크기: 개당 1MB 이하 권장
- Pagination: 페이지당 10개 포스트 (성능 최적화)

---

## 상태 전환

### Post 상태
- **Draft** (초안): `_drafts/` 폴더 (프로덕션 빌드에서 제외)
- **Published** (발행): `_posts/` 폴더 + `date`가 현재 시간 이전
- **Scheduled** (예약): `_posts/` 폴더 + `date`가 미래 (빌드 시 제외)

### Category/Tag 상태
- **Active**: 최소 1개 포스트에서 참조
- **Inactive**: 어떤 포스트도 참조하지 않음 (페이지에 표시 안됨)

---

## 데이터 무결성

### 참조 무결성
- `author`: `_data/authors.yml`에 정의되지 않으면 기본값 사용 (오류 없음)
- `categories`, `tags`: 미리 정의 불필요 (자동 생성)

### 중복 방지
- 포스트 파일명 중복: Git이 방지
- 카테고리/태그 중복: Jekyll이 자동 병합

---

## 다음 단계

이 데이터 모델을 기반으로 다음 문서 생성:
1. **contracts/post-schema.yaml**: Front Matter 스키마 정의
2. **quickstart.md**: 빠른 시작 가이드 (데이터 모델 사용 예시)
