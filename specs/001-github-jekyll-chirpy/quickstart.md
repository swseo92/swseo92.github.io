# 빠른 시작 가이드: GitHub 기반 테크 블로그

**프로젝트**: 001-github-jekyll-chirpy
**날짜**: 2025-10-15
**목표**: 2시간 이내에 Jekyll + Chirpy 블로그를 설정하고 GitHub Pages에 배포

---

## 개요

이 가이드는 Chirpy Starter 템플릿을 사용하여 기술 블로그를 빠르게 설정하는 방법을 단계별로 안내합니다. 개발 경험이 있으면 Ruby나 Jekyll에 대한 사전 지식 없이도 따라할 수 있습니다.

**예상 소요 시간**: 1-2시간
**난이도**: 초급
**전제 조건**:
- GitHub 계정
- Git 설치
- 텍스트 에디터 (VS Code 권장)

---

## 단계 1: 저장소 생성 (5분)

### 1.1 Chirpy Starter 템플릿 사용

1. 브라우저에서 https://github.com/cotes2020/chirpy-starter 접속
2. 우측 상단의 **"Use this template"** 버튼 클릭
3. **"Create a new repository"** 선택

### 1.2 저장소 설정

**저장소 이름 규칙**:
- 개인 블로그: `USERNAME.github.io` (예: `john-doe.github.io`)
- 프로젝트 블로그: 원하는 이름 (예: `tech-blog`)

**설정**:
- Owner: 본인 계정 선택
- Repository name: 위 규칙에 따라 입력
- Public: 선택 (GitHub Pages 무료 호스팅)
- "Create repository from template" 클릭

### 1.3 로컬에 클론

```bash
# 터미널 또는 Git Bash 열기
git clone https://github.com/USERNAME/USERNAME.github.io.git
cd USERNAME.github.io
```

---

## 단계 2: Ruby 및 Jekyll 설치 (10-15분)

### 2.1 Ruby 설치

#### Windows
1. https://rubyinstaller.org/ 접속
2. **Ruby+Devkit 3.2.x (x64)** 다운로드 및 설치
3. 설치 마지막 단계에서 "Run 'ridk install'" 체크
4. 옵션 1, 2, 3 순서로 설치

#### macOS
```bash
# Homebrew 설치 (없는 경우)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Ruby 설치
brew install ruby
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

#### Linux (Ubuntu/Debian)
```bash
sudo apt-get update
sudo apt-get install ruby-full build-essential zlib1g-dev
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### 2.2 Ruby 설치 확인

```bash
ruby -v
# 출력 예: ruby 3.2.2 (2023-03-30 revision ...) [x64-mingw-ucrt]

gem -v
# 출력 예: 3.4.10
```

### 2.3 Bundler 설치

```bash
gem install bundler
bundler -v
# 출력 예: Bundler version 2.4.10
```

---

## 단계 3: 의존성 설치 (5-10분)

### 3.1 프로젝트 의존성 설치

```bash
# 프로젝트 디렉토리에서 실행
bundle install
```

**예상 출력**:
```
Fetching gem metadata from https://rubygems.org/...
...
Bundle complete! 7 Gemfile dependencies, XX gems now installed.
```

**문제 해결**:
- **"Could not locate Gemfile"**: 올바른 디렉토리에 있는지 확인 (`ls Gemfile`)
- **"Failed to build gem native extension"**: Ruby Devkit 설치 확인 (Windows)

---

## 단계 4: 블로그 기본 설정 (15-20분)

### 4.1 _config.yml 편집

텍스트 에디터로 `_config.yml` 파일을 열고 다음 항목 수정:

```yaml
# ============================================================
# 사이트 설정 (필수)
# ============================================================

lang: ko-KR                           # 언어 설정
timezone: Asia/Seoul                  # 타임존

# ============================================================
# 사이트 정보 (필수)
# ============================================================

title: "나의 기술 블로그"              # 사이트 제목
tagline: "개발 여정을 기록하는 공간"    # 서브타이틀
description: >-                       # SEO용 설명 (2-3줄)
  소프트웨어 개발, 프로그래밍 기술, 그리고
  배운 것들을 기록하고 공유하는 블로그입니다.

url: "https://USERNAME.github.io"     # ⚠️ USERNAME을 본인 것으로 변경

# ============================================================
# 작성자 정보 (필수)
# ============================================================

github:
  username: USERNAME                  # ⚠️ GitHub 사용자명

social:
  name: "홍길동"                       # 작성자 이름
  email: your.email@example.com       # 이메일
  links:
    - https://github.com/USERNAME    # ⚠️ USERNAME 변경
    # - https://twitter.com/username  # 선택 사항
    # - https://linkedin.com/in/username

# ============================================================
# 테마 설정 (선택)
# ============================================================

theme_mode: # [light | dark]          # 비워두면 시스템 설정 따름

avatar: /assets/img/avatar.jpg        # 프로필 이미지 (선택)

# ============================================================
# 댓글 시스템 (선택 - 나중에 설정 가능)
# ============================================================

comments:
  active: # giscus                    # 주석 처리 (나중에 활성화)

# ============================================================
# Google Analytics (선택 - 나중에 설정 가능)
# ============================================================

google_analytics:
  id: # G-XXXXXXXXXX                  # 주석 처리 (나중에 활성화)
```

### 4.2 설정 값 치환 체크리스트

- [ ] `url`: `https://USERNAME.github.io` → 본인 저장소 URL로 변경
- [ ] `github.username`: 본인 GitHub 사용자명
- [ ] `social.name`: 본인 이름
- [ ] `social.email`: 본인 이메일
- [ ] `social.links`: GitHub URL 수정
- [ ] `title`, `tagline`, `description`: 원하는 내용으로 변경

---

## 단계 5: 로컬 테스트 (5분)

### 5.1 로컬 서버 시작

```bash
bundle exec jekyll serve
```

**예상 출력**:
```
Configuration file: /path/to/_config.yml
            Source: /path/to/
       Destination: /path/to/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in X.XXX seconds.
 Auto-regeneration: enabled for '/path/to/'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

### 5.2 브라우저에서 확인

1. 브라우저를 열고 `http://localhost:4000` 접속
2. Chirpy 테마가 적용된 블로그 홈페이지 확인
3. 설정한 제목, 설명이 제대로 표시되는지 확인

**문제 해결**:
- **404 에러**: 올바른 URL인지 확인 (`http://localhost:4000`)
- **빌드 에러**: `_config.yml` 문법 오류 확인 (YAML 들여쓰기)

### 5.3 서버 중지

```bash
# 터미널에서 Ctrl+C 누르기
```

---

## 단계 6: 첫 포스트 작성 (15-20분)

### 6.1 포스트 파일 생성

```bash
# _posts 디렉토리로 이동
cd _posts

# 샘플 포스트 삭제 (선택)
rm *.md

# 새 포스트 생성 (날짜는 오늘 날짜로)
touch 2025-10-15-my-first-post.md
```

### 6.2 포스트 내용 작성

`_posts/2025-10-15-my-first-post.md` 파일을 열고 다음 내용 입력:

```markdown
---
title: "첫 번째 포스트: Jekyll 블로그 시작"
date: 2025-10-15 14:30:00 +0900
categories: [Blogging, Tutorial]
tags: [jekyll, chirpy, github-pages]
---

## 안녕하세요!

이것은 Jekyll과 Chirpy 테마를 사용한 첫 번째 블로그 포스트입니다.

### 블로그 설정이 완료되었습니다

다음 기능들이 작동합니다:

- ✅ Markdown 문법
- ✅ 코드 하이라이팅
- ✅ 카테고리/태그
- ✅ 다크모드
- ✅ 반응형 디자인

### 코드 블록 예시

\```python
def hello_world():
    print("Hello, World!")

hello_world()
\```

### 다음 단계

이제 본격적으로 기술 블로그를 작성할 준비가 되었습니다!
```

### 6.3 로컬에서 포스트 확인

```bash
# 프로젝트 루트로 돌아가기
cd ..

# 로컬 서버 다시 시작
bundle exec jekyll serve
```

브라우저에서 `http://localhost:4000` 접속하여 새 포스트 확인

---

## 단계 7: About 페이지 작성 (10분)

### 7.1 About 페이지 편집

`_tabs/about.md` 파일을 열고 내용 작성:

```markdown
---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

## 소개

안녕하세요! 저는 소프트웨어 개발자 홍길동입니다.

### 관심 분야

- 웹 개발 (Frontend & Backend)
- Python, JavaScript
- DevOps & Cloud

### 기술 스택

- **Languages**: Python, JavaScript, TypeScript
- **Frameworks**: Django, React, Next.js
- **Tools**: Docker, Git, GitHub Actions

### 연락처

- Email: your.email@example.com
- GitHub: [@USERNAME](https://github.com/USERNAME)

---

> 이 블로그는 제가 배운 것들을 기록하고 공유하는 공간입니다.
```

---

## 단계 8: GitHub에 푸시 (5분)

### 8.1 변경사항 커밋

```bash
# 변경된 파일 확인
git status

# 모든 변경사항 스테이징
git add .

# 커밋
git commit -m "Initial blog setup: config, first post, about page"

# GitHub에 푸시
git push origin main
```

---

## 단계 9: GitHub Pages 설정 (5분)

### 9.1 GitHub Actions 확인

1. GitHub 저장소 페이지 접속
2. **"Actions"** 탭 클릭
3. 자동으로 실행된 워크플로우 확인 (주황색 → 녹색)

**워크플로우 진행 상황**:
- 🟡 **In progress**: 빌드 중 (1-2분 소요)
- ✅ **Success**: 빌드 성공
- ❌ **Failure**: 빌드 실패 (로그 확인 필요)

### 9.2 GitHub Pages 활성화

1. 저장소 **"Settings"** 탭 클릭
2. 왼쪽 메뉴에서 **"Pages"** 선택
3. **Source**: "GitHub Actions" 선택 (자동 감지)
4. 빌드 완료 후 사이트 URL 표시: `https://USERNAME.github.io`

### 9.3 배포 확인

브라우저에서 `https://USERNAME.github.io` 접속하여 블로그 확인

**예상 소요 시간**: 첫 배포는 1-2분 소요

---

## 단계 10: 추가 설정 (선택 사항)

### 10.1 프로필 이미지 추가

1. 이미지 파일 준비 (정사각형, 200x200px 이상)
2. `assets/img/` 디렉토리에 `avatar.jpg` 저장
3. `_config.yml`에서 이미 설정됨: `avatar: /assets/img/avatar.jpg`

### 10.2 파비콘 설정

1. https://realfavicongenerator.net/ 접속
2. 이미지 업로드 및 파비콘 생성
3. 생성된 파일들을 `assets/img/favicons/` 디렉토리에 저장

### 10.3 댓글 시스템 설정 (Giscus)

#### 전제 조건
- GitHub Discussions 활성화 (저장소 Settings > Features > Discussions)

#### 설정 단계
1. https://giscus.app/ 접속
2. 저장소 선택: `USERNAME/USERNAME.github.io`
3. Discussion 카테고리 선택: "Announcements" 권장
4. 설정 코드 복사 (repo, repo-id, category, category-id)

5. `_config.yml` 수정:
```yaml
comments:
  active: giscus
  giscus:
    repo: USERNAME/USERNAME.github.io
    repo_id: R_xxxxxxxxxxxxx        # Giscus에서 복사
    category: Announcements
    category_id: DIC_xxxxxxxxxxxxx  # Giscus에서 복사
```

6. 커밋 및 푸시:
```bash
git add _config.yml
git commit -m "Add Giscus comments system"
git push origin main
```

### 10.4 Google Analytics 설정

1. https://analytics.google.com/ 접속
2. 새 속성 생성: "데이터 스트림" > "웹"
3. 측정 ID 복사 (G-XXXXXXXXXX 형식)

4. `_config.yml` 수정:
```yaml
google_analytics:
  id: G-XXXXXXXXXX
```

5. 커밋 및 푸시:
```bash
git add _config.yml
git commit -m "Add Google Analytics"
git push origin main
```

---

## 빠른 참조

### 일상적인 블로그 운영

#### 새 포스트 작성 프로세스

```bash
# 1. 새 포스트 파일 생성 (날짜는 오늘)
touch _posts/$(date +%Y-%m-%d)-my-new-post.md

# 2. 포스트 작성 (에디터에서)
# Front Matter + Markdown 내용 입력

# 3. 로컬에서 미리보기 (선택)
bundle exec jekyll serve
# 브라우저: http://localhost:4000

# 4. Git 커밋 및 푸시
git add _posts/$(date +%Y-%m-%d)-my-new-post.md
git commit -m "Add new post: [제목]"
git push origin main

# 5. 배포 확인 (1-2분 후)
# https://USERNAME.github.io 접속
```

### 유용한 명령어

```bash
# 로컬 서버 시작
bundle exec jekyll serve

# 로컬 서버 시작 (초안 포함)
bundle exec jekyll serve --drafts

# 빌드만 (서버 시작 안함)
bundle exec jekyll build

# 의존성 업데이트
bundle update

# 캐시 정리 (빌드 이슈 시)
bundle exec jekyll clean
```

### 포스트 Front Matter 템플릿

```yaml
---
title: "포스트 제목"
date: YYYY-MM-DD HH:MM:SS +0900
categories: [Category1, Category2]
tags: [tag1, tag2, tag3]
description: "SEO용 포스트 요약 (150-160자)"
image:
  path: /assets/img/posts/image.jpg
  alt: "이미지 설명"
pin: false
math: false
mermaid: false
---
```

---

## 문제 해결

### 로컬 빌드 오류

**증상**: `bundle exec jekyll serve` 실패

**해결**:
```bash
# 1. Gemfile.lock 삭제
rm Gemfile.lock

# 2. 의존성 재설치
bundle install

# 3. 캐시 정리
bundle exec jekyll clean

# 4. 다시 시도
bundle exec jekyll serve
```

### GitHub Actions 빌드 실패

**증상**: Actions 탭에서 빌드 실패 (빨간색 X)

**해결**:
1. Actions 탭에서 실패한 워크플로우 클릭
2. 로그 확인 (에러 메시지 확인)
3. 일반적인 원인:
   - `_config.yml` 문법 오류 (YAML 들여쓰기)
   - Front Matter 형식 오류
   - 날짜 형식 오류

### 포스트가 표시되지 않음

**원인 1**: 날짜가 미래인 경우
- **해결**: Front Matter의 `date`를 현재 시간 이전으로 변경

**원인 2**: 파일명 형식 오류
- **해결**: `YYYY-MM-DD-title.md` 형식 확인

**원인 3**: Front Matter 누락
- **해결**: `title`과 `date` 필수 필드 확인

### 한글이 깨짐

**원인**: 파일 인코딩 문제

**해결**:
1. 에디터 설정: 파일을 UTF-8로 저장
2. VS Code: 우측 하단 "UTF-8" 확인
3. Windows 메모장 사용 시: "다른 이름으로 저장" > 인코딩: UTF-8

---

## 다음 단계

블로그 설정이 완료되었습니다! 이제 다음을 수행할 수 있습니다:

1. **포스트 작성**: 기술 글, 튜토리얼, 프로젝트 소개 등
2. **카테고리 정리**: 일관된 카테고리 구조 설계
3. **테마 커스터마이징**: `_sass/` 폴더에서 색상/폰트 변경 (선택)
4. **댓글/Analytics 추가**: 독자와 소통 및 방문자 추적
5. **SEO 최적화**: sitemap.xml 확인, Google Search Console 등록

---

## 참고 자료

- **Chirpy 공식 문서**: https://chirpy.cotes.page/
- **Jekyll 공식 문서**: https://jekyllrb.com/docs/
- **Markdown 가이드**: https://www.markdownguide.org/
- **GitHub Pages 문서**: https://docs.github.com/en/pages
- **프로젝트 data-model.md**: 데이터 구조 상세 설명
- **프로젝트 post-schema.yaml**: Front Matter 스키마 전체

---

**축하합니다! 블로그 설정이 완료되었습니다.** 🎉

이제 기술 블로그 작성을 시작하세요!
