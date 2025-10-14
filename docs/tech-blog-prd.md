# Product Requirements Document (PRD)
## GitHub 기반 테크 블로그 구축

### 1. 프로젝트 개요

**목표:** Jekyll + Chirpy 테마를 사용한 GitHub Pages 기반 개인 테크 블로그 구축

**배포 방식:** GitHub Pages (username.github.io 또는 custom domain)

**핵심 원칙:** 최대한 간단하고 유지보수가 쉬운 구조

---

### 2. 기술 스택

- **정적 사이트 생성기:** Jekyll 4.x
- **테마:** Chirpy (https://github.com/cotes2020/jekyll-theme-chirpy)
- **호스팅:** GitHub Pages
- **빌드/배포:** GitHub Actions (자동)
- **콘텐츠 형식:** Markdown

---

### 3. 기능 요구사항

#### 3.1 필수 기능 (Must Have)
- [ ] Markdown 기반 포스트 작성
- [ ] 포스트 목록 (최신순 정렬)
- [ ] 카테고리 시스템
- [ ] 태그 시스템
- [ ] 반응형 디자인 (모바일/태블릿/데스크톱)
- [ ] 다크모드/라이트모드 토글
- [ ] 검색 기능
- [ ] RSS 피드
- [ ] 사이드바 네비게이션
- [ ] About 페이지

#### 3.2 권장 기능 (Should Have)
- [ ] 댓글 시스템 (utterances 또는 giscus)
- [ ] Google Analytics 연동
- [ ] 소셜 미디어 공유 버튼
- [ ] 목차(TOC) 자동 생성
- [ ] 코드 하이라이팅 (Syntax Highlighting)
- [ ] 포스트 읽는 시간 표시
- [ ] 최근 포스트/인기 포스트 위젯

#### 3.3 선택 기능 (Nice to Have)
- [ ] 다국어 지원
- [ ] 커스텀 도메인 연결
- [ ] SEO 최적화 (sitemap, robots.txt)
- [ ] 이미지 최적화

---

### 4. 프로젝트 구조

```
tech_blog/
├── _config.yml           # Jekyll 설정 파일
├── _posts/               # 블로그 포스트 저장
│   └── YYYY-MM-DD-title.md
├── _tabs/                # About, Archives 등 고정 페이지
│   ├── about.md
│   ├── archives.md
│   ├── categories.md
│   └── tags.md
├── assets/               # 이미지, CSS, JS
│   └── img/
├── .github/
│   └── workflows/        # GitHub Actions 설정
│       └── pages-deploy.yml
├── Gemfile               # Ruby 의존성
├── README.md
└── index.html            # 홈페이지
```

---

### 5. 구현 단계

#### Phase 1: 초기 설정 (필수)
1. **Chirpy Starter 템플릿 적용**
   - Chirpy Starter repository fork 또는 template 사용
   - 로컬 환경에 클론

2. **기본 설정 (`_config.yml`)**
   - 블로그 제목/설명
   - 작성자 정보
   - 타임존 (Asia/Seoul)
   - URL 설정
   - 소셜 미디어 링크

3. **GitHub Pages 활성화**
   - Repository 설정에서 Pages 활성화
   - GitHub Actions 워크플로우 설정

4. **샘플 포스트 작성**
   - `_posts/` 폴더에 첫 포스트 작성
   - Front matter 형식 확인
   - 로컬 테스트

#### Phase 2: 커스터마이징 (선택)
1. **About 페이지 작성**
   - `_tabs/about.md` 수정
   - 자기소개, 기술 스택, 경력 등

2. **테마 색상/폰트 조정**
   - `_sass/` 폴더 내 SCSS 파일 수정 (필요시)

3. **댓글 시스템 추가**
   - utterances 또는 giscus 설정
   - `_config.yml`에 설정 추가

4. **Analytics 연동**
   - Google Analytics 설정
   - `_config.yml`에 Tracking ID 추가

#### Phase 3: 콘텐츠 작성
1. **초기 포스트 작성 (3-5개)**
   - 기술 관련 주제
   - Markdown 문법 활용
   - 이미지/코드 블록 포함

2. **카테고리 및 태그 정리**
   - 일관된 카테고리 구조
   - 의미있는 태그 사용

---

### 6. 포스트 작성 가이드

#### 6.1 파일 명명 규칙
```
YYYY-MM-DD-title-in-lowercase.md
```
예: `2025-10-15-github-blog-setup.md`

#### 6.2 Front Matter 템플릿
```yaml
---
title: "포스트 제목"
date: YYYY-MM-DD HH:MM:SS +0900
categories: [Main Category, Sub Category]
tags: [tag1, tag2, tag3]
author: <author_id>
description: "포스트 요약 (SEO용)"
---
```

#### 6.3 콘텐츠 작성 규칙
- 제목은 H1(#) 사용하지 않음 (title에서 자동 생성)
- 섹션은 H2(##)부터 시작
- 코드 블록은 언어 지정 (```python, ```javascript 등)
- 이미지는 `assets/img/` 폴더에 저장

---

### 7. 배포 프로세스

#### 7.1 로컬 테스트
```bash
bundle install
bundle exec jekyll serve
# http://localhost:4000 접속
```

#### 7.2 GitHub 배포
```bash
git add .
git commit -m "Add new post: [post title]"
git push origin main
```

#### 7.3 자동 배포
- GitHub Actions가 자동으로 빌드/배포
- `gh-pages` 브랜치에 결과물 생성
- 약 1-2분 후 사이트 업데이트

---

### 8. 성공 기준

#### 8.1 기술적 성공 지표
- [ ] GitHub Pages에서 사이트 정상 작동
- [ ] 모바일/데스크톱 반응형 확인
- [ ] 검색 기능 작동
- [ ] 다크모드 전환 정상 작동
- [ ] 빌드 에러 없음

#### 8.2 콘텐츠 성공 지표
- [ ] 최소 3개 이상의 포스트 작성
- [ ] About 페이지 완성
- [ ] 카테고리/태그 정리 완료

---

### 9. 참고 자료

- **Chirpy 공식 문서:** https://chirpy.cotes.page/
- **Jekyll 공식 문서:** https://jekyllrb.com/docs/
- **GitHub Pages 가이드:** https://docs.github.com/en/pages
- **Markdown 가이드:** https://www.markdownguide.org/

---

### 10. 예상 작업 시간

- Phase 1 (초기 설정): 1-2시간
- Phase 2 (커스터마이징): 1-2시간
- Phase 3 (콘텐츠 작성): 시간당 1-2개 포스트

**총 예상 시간:** 4-6시간 (초기 설정 + 기본 콘텐츠)

---

### 11. 주의사항

1. **Ruby 환경 필요**
   - Jekyll은 Ruby 기반
   - Ruby 2.7 이상, Bundler 설치 필요

2. **Git 저장소 이름**
   - 개인 블로그: `username.github.io`
   - 프로젝트 블로그: 임의 이름 가능

3. **빌드 시간**
   - GitHub Actions 빌드 시간 고려
   - 실시간 반영 아님 (1-2분 소요)

4. **이미지 최적화**
   - 큰 이미지는 사이트 로딩 속도 저하
   - 적절한 크기로 리사이징 권장

---

### 12. 향후 확장 가능성

- 시리즈 포스트 기능
- 프로젝트 포트폴리오 섹션
- Newsletter 구독 기능
- 커스텀 도메인 연결
- CDN 연동으로 성능 최적화

---

## 구현 체크리스트

### 환경 설정
- [ ] Ruby 설치 확인
- [ ] Bundler 설치
- [ ] Git 설정 확인
- [ ] GitHub 계정 준비

### 프로젝트 초기화
- [ ] Chirpy Starter 템플릿 적용
- [ ] `_config.yml` 기본 설정
- [ ] Gemfile 의존성 설치
- [ ] 로컬 서버 실행 확인

### GitHub Pages 설정
- [ ] Repository 생성
- [ ] GitHub Pages 활성화
- [ ] GitHub Actions 워크플로우 추가
- [ ] 배포 확인

### 콘텐츠 작성
- [ ] About 페이지 작성
- [ ] 첫 번째 포스트 작성
- [ ] 카테고리/태그 설정
- [ ] 이미지 업로드 테스트

### 테스트
- [ ] 데스크톱 브라우저 확인
- [ ] 모바일 브라우저 확인
- [ ] 검색 기능 테스트
- [ ] 다크모드 전환 테스트
- [ ] RSS 피드 확인

---

**작성자:** Claude Code
**작성일:** 2025-10-15
**버전:** 1.0
