# 기술 블로그

Jekyll과 Chirpy 테마를 사용한 개발 블로그입니다.

## 특징

- 🎨 **Chirpy 테마** - 모던하고 깔끔한 디자인
- 🌓 **다크 모드** - 라이트/다크 테마 자동 전환
- 🔍 **검색 기능** - 포스트 빠른 검색
- 📱 **반응형 디자인** - 모든 기기에서 최적화
- 🏷️ **카테고리/태그** - 체계적인 콘텐츠 분류
- 🚀 **자동 배포** - GitHub Actions를 통한 CI/CD

## 로컬 개발 환경 설정

### 사전 요구사항

- Ruby 2.7 이상
- Bundler
- Git

### 설치 방법

1. **저장소 클론**
   ```bash
   git clone https://github.com/USERNAME/USERNAME.github.io.git
   cd USERNAME.github.io
   ```

2. **의존성 설치**
   ```bash
   gem install bundler
   bundle install
   ```

3. **로컬 서버 실행**
   ```bash
   bundle exec jekyll serve
   ```

4. **브라우저에서 확인**
   ```
   http://localhost:4000
   ```

## 새 포스트 작성하기

1. **포스트 파일 생성**

   `_posts/` 디렉토리에 `YYYY-MM-DD-title.md` 형식으로 파일을 생성합니다.

   예: `_posts/2025-10-15-my-new-post.md`

2. **Front Matter 작성**

   ```yaml
   ---
   title: "포스트 제목"
   date: 2025-10-15 14:30:00 +0900
   categories: [카테고리1, 카테고리2]
   tags: [태그1, 태그2, 태그3]
   description: "포스트 요약 (SEO용, 50-160자)"
   ---
   ```

3. **Markdown으로 본문 작성**

   Front Matter 아래에 Markdown 문법으로 내용을 작성합니다.

4. **로컬에서 확인**

   ```bash
   bundle exec jekyll serve
   ```

   브라우저에서 `http://localhost:4000`에 접속하여 확인합니다.

5. **배포**

   ```bash
   git add .
   git commit -m "Add new post: 제목"
   git push origin main
   ```

   GitHub Actions가 자동으로 빌드하고 배포합니다.

## 블로그 설정 변경

`_config.yml` 파일을 수정하여 블로그 설정을 변경할 수 있습니다:

- `title`: 블로그 제목
- `tagline`: 블로그 부제목
- `description`: 블로그 설명
- `url`: 블로그 URL (예: `https://username.github.io`)
- `github.username`: GitHub 사용자명
- `social`: 소셜 미디어 정보

변경 후 로컬 서버를 재시작해야 합니다.

## 디렉토리 구조

```
tech_blog/
├── _config.yml           # Jekyll 설정 파일
├── _posts/               # 블로그 포스트
├── _tabs/                # About, Archives 등 페이지
├── assets/               # 이미지, CSS, JS 등
│   └── img/
│       ├── avatar.jpg    # 프로필 이미지
│       └── posts/        # 포스트 이미지
├── .github/
│   └── workflows/
│       └── pages-deploy.yml  # GitHub Actions 워크플로우
├── Gemfile               # Ruby 의존성
└── README.md             # 이 파일
```

## Front Matter 옵션

### 필수 필드
- `title`: 포스트 제목
- `date`: 발행 날짜 및 시간 (YYYY-MM-DD HH:MM:SS +HHMM)

### 선택 필드
- `categories`: 카테고리 (최대 2개 권장)
- `tags`: 태그 (3-7개 권장)
- `description`: SEO용 요약 (50-160자)
- `image`: 대표 이미지
  - `path`: 이미지 경로
  - `alt`: 대체 텍스트
- `pin`: 홈페이지 상단 고정 (true/false)
- `math`: MathJax 활성화 (true/false)
- `mermaid`: Mermaid 다이어그램 활성화 (true/false)
- `toc`: 목차 표시 (true/false, 기본값: true)
- `comments`: 댓글 활성화 (true/false, 기본값: true)

## 이미지 추가하기

1. 이미지를 `assets/img/posts/` 디렉토리에 저장
2. 포스트에서 참조:
   ```markdown
   ![이미지 설명](/assets/img/posts/image-name.jpg)
   ```

## 배포

GitHub에 푸시하면 GitHub Actions가 자동으로 빌드하고 배포합니다.

배포 상태는 저장소의 "Actions" 탭에서 확인할 수 있습니다.

## 문제 해결

### 로컬 빌드 오류

```bash
# 의존성 다시 설치
bundle install

# 캐시 삭제
bundle exec jekyll clean
bundle exec jekyll serve
```

### GitHub Actions 빌드 실패

1. 저장소의 "Actions" 탭에서 오류 로그 확인
2. `_config.yml`의 `url` 설정 확인
3. Front Matter 문법 오류 확인

### 포스트가 표시되지 않음

- 파일명이 `YYYY-MM-DD-title.md` 형식인지 확인
- Front Matter의 `date`가 미래 날짜가 아닌지 확인
- Front Matter가 올바른 YAML 형식인지 확인

## 참고 자료

- [Jekyll 공식 문서](https://jekyllrb.com/)
- [Chirpy 테마](https://github.com/cotes2020/jekyll-theme-chirpy)
- [Chirpy 데모](https://chirpy.cotes.page/)
- [GitHub Pages 문서](https://docs.github.com/pages)
- [Markdown 가이드](https://www.markdownguide.org/)

## 라이선스

이 프로젝트는 MIT 라이선스를 따릅니다.
