# Claude Code 프로젝트 가이드

이 문서는 Claude Code 또는 AI 어시스턴트가 이 프로젝트를 효과적으로 지원하기 위한 컨텍스트를 제공합니다.

## 프로젝트 개요

**프로젝트명**: swseo92 기술 블로그
**타입**: Jekyll 정적 사이트 (GitHub Pages)
**테마**: Chirpy 6.x
**언어**: 한국어 (ko-KR)
**호스팅**: GitHub Pages (https://swseo92.github.io)
**생성일**: 2025-10-15

### 목적
개발 경험과 기술 지식을 공유하는 개인 기술 블로그

## 기술 스택

### 코어 기술
- **Jekyll**: 4.3+ (정적 사이트 생성기)
- **Ruby**: 2.7+ (Jekyll 실행 환경)
- **Chirpy Theme**: 6.x (Jekyll 테마)
- **Markdown**: 콘텐츠 작성 형식

### 의존성
- `jekyll-theme-chirpy` - 메인 테마
- `jekyll-paginate` - 페이지네이션
- `jekyll-seo-tag` - SEO 최적화
- `jekyll-archives` - 카테고리/태그 아카이브
- `jekyll-sitemap` - 사이트맵 생성
- `jekyll-feed` - RSS 피드

### 배포
- **CI/CD**: GitHub Actions
- **워크플로우**: `.github/workflows/pages-deploy.yml`
- **빌드 시간**: 1-3분
- **자동 배포**: main 브랜치 푸시 시

## 디렉토리 구조

```
swseo92.github.io/
├── _config.yml              # Jekyll 설정 (중요!)
├── _posts/                  # 블로그 포스트 (YYYY-MM-DD-title.md)
├── _tabs/                   # 고정 페이지 (about.md, archives.md 등)
├── _data/                   # 데이터 파일
├── assets/                  # 정적 자산
│   └── img/                 # 이미지 파일
│       ├── avatar.jpg       # 프로필 이미지
│       └── posts/           # 포스트 이미지
├── .github/workflows/       # GitHub Actions
├── docs/                    # 프로젝트 문서
│   └── tech-blog-prd.md     # PRD
├── specs/                   # Speckit 설계 문서
│   └── 001-github-jekyll-chirpy/
│       ├── spec.md          # 기능 명세 (6 user stories)
│       ├── tasks.md         # 구현 작업 (48 tasks)
│       ├── plan.md          # 기술 계획
│       ├── research.md      # 기술 조사
│       ├── data-model.md    # 데이터 모델
│       ├── quickstart.md    # 퀵스타트 가이드
│       ├── contracts/       # 스키마 정의
│       └── checklists/      # 품질 체크리스트
├── .specify/                # Speckit 템플릿
├── README.md                # 프로젝트 README
├── 시작_가이드.md           # 한국어 퀵스타트
├── Gemfile                  # Ruby 의존성
└── index.html               # 홈페이지 (테마 제공)
```

## 주요 파일

### _config.yml (필수)
블로그의 모든 전역 설정을 담당합니다.

**중요 설정:**
```yaml
lang: ko-KR
timezone: Asia/Seoul
title: 기술 블로그
tagline: 개발 경험과 배운 것들을 공유합니다
url: "https://swseo92.github.io"
github:
  username: swseo92
social:
  name: swseo92
  email: swseo92@example.com
```

**수정 시 주의:**
- YAML 문법 엄격 (들여쓰기 중요)
- `url` 필드는 슬래시(/)로 끝나면 안 됨
- 변경 후 로컬 서버 재시작 필요

### Front Matter 스키마
모든 포스트는 다음 Front Matter를 따라야 합니다:

**필수 필드:**
```yaml
title: "포스트 제목"
date: YYYY-MM-DD HH:MM:SS +0900
```

**권장 필드:**
```yaml
categories: [카테고리1, 카테고리2]  # 최대 2개
tags: [태그1, 태그2, 태그3]         # 3-7개 권장
description: "SEO용 요약 (50-160자)"
```

**선택 필드:**
```yaml
image:
  path: /assets/img/posts/image.jpg
  alt: "이미지 설명"
pin: false           # 홈페이지 상단 고정
math: false          # MathJax 활성화
mermaid: false       # Mermaid 다이어그램
toc: true            # 목차 표시
comments: true       # 댓글 활성화
```

자세한 스키마는 `specs/001-github-jekyll-chirpy/contracts/post-schema.yaml` 참조

## 개발 워크플로우

### 1. 로컬 개발 환경 시작

```bash
# 의존성 설치 (최초 1회)
bundle install

# 로컬 서버 실행
bundle exec jekyll serve

# 브라우저 접속
# http://localhost:4000
```

### 2. 새 포스트 작성

```bash
# 1. 파일 생성 (파일명 형식 중요!)
_posts/YYYY-MM-DD-title.md

# 2. Front Matter 작성
---
title: "포스트 제목"
date: 2025-10-15 14:30:00 +0900
categories: [Development, Tutorial]
tags: [jekyll, blogging]
description: "간단한 설명"
---

# 3. Markdown 본문 작성

# 4. 로컬에서 확인
bundle exec jekyll serve

# 5. Git 커밋 및 푸시
git add _posts/
git commit -m "Add new post: 제목"
git push origin main
```

### 3. 이미지 추가

```bash
# 1. 이미지 저장
assets/img/posts/2025-10-15-image-name.jpg

# 2. 포스트에서 참조
![이미지 설명](/assets/img/posts/2025-10-15-image-name.jpg)
```

### 4. 배포

```bash
# main 브랜치에 푸시하면 자동 배포
git push origin main

# GitHub Actions에서 빌드 및 배포 (1-3분)
# https://github.com/swseo92/swseo92.github.io/actions
```

## AI 어시스턴트 작업 가이드

### 포스트 작성 지원 시

1. **파일명 규칙 확인**
   - 반드시 `YYYY-MM-DD-title.md` 형식
   - 날짜는 Front Matter의 date와 일치하는 것이 좋음

2. **Front Matter 검증**
   - `title`과 `date`는 필수
   - `date` 형식: `YYYY-MM-DD HH:MM:SS +0900`
   - 미래 날짜는 빌드에서 제외됨

3. **카테고리/태그 일관성**
   - 기존 카테고리: Blogging, Development, Tools, Guide
   - 기존 태그: jekyll, chirpy, github-pages, markdown, tech-stack, productivity
   - 새 카테고리/태그 추가 시 Title Case (카테고리), lowercase (태그)

4. **이미지 경로**
   - 절대 경로 사용: `/assets/img/posts/`
   - 상대 경로 사용 금지

### 설정 파일 수정 시

1. **_config.yml 수정**
   - YAML 문법 검증 필수
   - 들여쓰기는 스페이스 2칸
   - 변경 후 로컬 빌드 테스트 권장

2. **GitHub Actions 워크플로우**
   - `.github/workflows/pages-deploy.yml` 수정 시 주의
   - Ruby 버전, Jekyll 버전 호환성 확인

### 문서 생성/수정 시

1. **Markdown 형식**
   - GitHub Flavored Markdown (GFM) 사용
   - 코드 블록에 언어 지정 (Syntax Highlighting)

2. **한글 인코딩**
   - 모든 파일은 UTF-8로 저장
   - Windows에서 CRLF/LF 변환 경고는 정상

## 자주 발생하는 문제

### 1. 빌드 실패

**증상**: GitHub Actions에서 빌드 실패
**원인**:
- `_config.yml` YAML 문법 오류
- Front Matter 형식 오류
- 미래 날짜의 포스트

**해결**:
```bash
# 로컬에서 빌드 테스트
bundle exec jekyll build

# YAML 문법 검증
# _config.yml의 들여쓰기 확인
```

### 2. 포스트가 표시되지 않음

**원인**:
- 파일명 형식 오류
- Front Matter 누락
- 미래 날짜 설정

**해결**:
- 파일명: `YYYY-MM-DD-title.md`
- `title`과 `date` 필수 필드 확인
- 날짜를 현재 또는 과거로 설정

### 3. 이미지가 표시되지 않음

**원인**:
- 잘못된 경로
- 파일이 커밋되지 않음

**해결**:
```bash
# 절대 경로 사용
![설명](/assets/img/posts/image.jpg)

# 이미지 파일 커밋 확인
git add assets/img/posts/
git commit -m "Add images"
git push
```

### 4. 한글 깨짐

**원인**:
- UTF-8 인코딩 아님
- BOM (Byte Order Mark) 포함

**해결**:
- 파일을 UTF-8 (BOM 없음)으로 저장
- VS Code: 우측 하단 인코딩 확인

## 성능 최적화

### 이미지 최적화
- 권장 크기: 1200x630px (소셜 미디어)
- 포맷: WebP 권장 (JPEG, PNG도 가능)
- 파일 크기: 500KB 이하

### 빌드 시간 단축
- 포스트 수가 많아지면 빌드 시간 증가
- `_posts/` 디렉토리만 변경 시 빌드 빠름
- 로컬 개발 시 `--incremental` 플래그 사용 가능

## 참고 문서

### 프로젝트 문서
- `README.md` - 일반 사용자용 문서
- `시작_가이드.md` - 한국어 퀵스타트 (단계별)
- `docs/tech-blog-prd.md` - 제품 요구사항 문서

### Speckit 문서
- `specs/001-github-jekyll-chirpy/spec.md` - 기능 명세
- `specs/001-github-jekyll-chirpy/tasks.md` - 구현 작업 목록
- `specs/001-github-jekyll-chirpy/quickstart.md` - 2시간 퀵스타트
- `specs/001-github-jekyll-chirpy/contracts/post-schema.yaml` - Front Matter 스키마

### 외부 문서
- [Jekyll 공식 문서](https://jekyllrb.com/docs/)
- [Chirpy 테마 문서](https://chirpy.cotes.page/)
- [GitHub Pages 문서](https://docs.github.com/pages)
- [Markdown 가이드](https://www.markdownguide.org/)

## 프로젝트 히스토리

### 생성 경위
- **날짜**: 2025-10-15
- **방법**: Chirpy Starter 템플릿 사용
- **초기 설정**: Claude Code를 통한 자동화 설정
- **첫 커밋**: Initial blog setup

### 주요 마일스톤
- ✅ 저장소 생성 및 초기 설정
- ✅ 샘플 포스트 3개 추가
- ✅ About 페이지 작성
- ✅ 프로젝트 문서화 (PRD, Specs, Tasks)
- ⏳ 첫 배포 (진행 중)
- ⏳ 실제 기술 포스트 작성 (예정)

## 다음 작업 (TODO)

### 필수
- [ ] GitHub Pages 배포 완료
- [ ] 첫 실제 포스트 작성
- [ ] 프로필 이미지 추가 (`assets/img/avatar.jpg`)

### 권장
- [ ] 댓글 시스템 설정 (Utterances/Giscus)
- [ ] Google Analytics 추가
- [ ] 커스텀 도메인 설정 (선택)
- [ ] RSS 피드 홍보

### 콘텐츠
- [ ] Python/Django 관련 포스트
- [ ] React/TypeScript 튜토리얼
- [ ] DevOps/Docker 가이드
- [ ] 프로젝트 회고록

## AI 어시스턴트에게

이 프로젝트는 체계적인 문서화와 자동화를 중시합니다. 작업 시 다음을 고려해주세요:

1. **문서 우선**: 코드 변경 시 관련 문서도 업데이트
2. **일관성**: 기존 스타일과 규칙 준수
3. **검증**: 변경 사항은 로컬 빌드로 테스트
4. **명확성**: 커밋 메시지는 구체적으로 작성

### 금지 사항
- ❌ `_config.yml`의 `url` 필드를 슬래시(/)로 끝내지 말 것
- ❌ 포스트 파일명을 잘못된 형식으로 생성하지 말 것
- ❌ Front Matter 없이 포스트를 생성하지 말 것
- ❌ 미래 날짜로 포스트를 생성하지 말 것 (테스트 제외)

### 권장 사항
- ✅ 변경 전 관련 문서 읽기
- ✅ 로컬 테스트 후 커밋
- ✅ 구체적이고 명확한 커밋 메시지
- ✅ 한글 콘텐츠의 경우 UTF-8 인코딩 확인

---

**최종 업데이트**: 2025-10-15
**작성자**: Claude Code
**프로젝트 버전**: 0.1.0 (Initial Setup)
