---
title: "첫 번째 포스트: Jekyll 블로그 시작"
date: 2024-10-15 14:30:00 +0900
categories: [Blogging, Tutorial]
tags: [jekyll, chirpy, github-pages]
description: "Jekyll과 Chirpy 테마로 GitHub Pages에 블로그를 만드는 방법을 소개합니다."
---

## 환영합니다!

Jekyll과 Chirpy 테마를 사용하여 GitHub Pages에 블로그를 만들었습니다. 이 포스트에서는 블로그 설정 과정을 간단히 소개합니다.

## Jekyll이란?

**Jekyll**은 정적 사이트 생성기(Static Site Generator)입니다. 주요 특징은:

- Markdown으로 콘텐츠 작성
- GitHub Pages 기본 지원
- 플러그인 시스템
- 테마 커스터마이징

## Chirpy 테마의 장점

Chirpy 테마는 다음과 같은 기능을 제공합니다:

1. **반응형 디자인** - 모바일, 태블릿, 데스크톱 모두 지원
2. **다크 모드** - 자동 테마 전환
3. **검색 기능** - 클라이언트 사이드 검색
4. **TOC** - 목차 자동 생성
5. **카테고리/태그** - 체계적인 분류

## 설치 과정

기본적인 설치 과정은 다음과 같습니다:

```bash
# 1. Chirpy Starter 템플릿으로 저장소 생성
# GitHub에서 "Use this template" 클릭

# 2. 로컬에 클론
git clone https://github.com/USERNAME/USERNAME.github.io.git

# 3. 의존성 설치
bundle install

# 4. 로컬 서버 실행
bundle exec jekyll serve
```

## 포스트 작성 방법

새 포스트를 작성하려면:

1. `_posts/` 디렉토리에 `YYYY-MM-DD-title.md` 형식으로 파일 생성
2. Front Matter 작성 (title, date, categories, tags)
3. Markdown으로 본문 작성
4. Git에 커밋하고 푸시

```yaml
---
title: "포스트 제목"
date: 2025-10-15 14:30:00 +0900
categories: [카테고리1, 카테고리2]
tags: [태그1, 태그2]
---
```

## 다음 단계

앞으로 이 블로그에서 다룰 주제:

- [ ] Markdown 문법 가이드
- [ ] 기술 스택 소개
- [ ] 프로젝트 경험 공유
- [ ] 개발 팁과 트릭

## 결론

Jekyll과 Chirpy를 사용하면 빠르고 쉽게 전문적인 블로그를 만들 수 있습니다. GitHub Pages를 통해 무료 호스팅도 제공되므로, 개발자라면 누구나 시도해볼 만합니다.

블로그 구축 과정에서 궁금한 점이 있다면 댓글로 남겨주세요!

---

**참고 링크:**
- [Jekyll 공식 문서](https://jekyllrb.com/)
- [Chirpy 테마](https://github.com/cotes2020/jekyll-theme-chirpy)
- [GitHub Pages](https://pages.github.com/)
