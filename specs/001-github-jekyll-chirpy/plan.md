# 구현 계획: GitHub 기반 테크 블로그

**브랜치**: `001-github-jekyll-chirpy` | **날짜**: 2025-10-15 | **명세서**: [spec.md](./spec.md)
**입력**: `/specs/001-github-jekyll-chirpy/spec.md`의 기능 명세서

**참고**: 이 템플릿은 `/speckit.plan` 명령어로 작성됩니다. 실행 워크플로우는 `.specify/templates/commands/plan.md`를 참조하세요.

## 요약

Jekyll 정적 사이트 생성기와 Chirpy 테마를 사용하여 GitHub Pages에서 호스팅되는 개인 테크 블로그를 구축합니다. 블로그는 Markdown 기반 포스트 작성, 카테고리/태그 분류, 다크모드, 검색 기능, 반응형 디자인을 지원하며, GitHub Actions를 통해 자동으로 빌드 및 배포됩니다.

**핵심 접근법**:
- Chirpy Starter 템플릿 사용으로 초기 설정 간소화
- Jekyll의 Front Matter 및 Liquid 템플릿을 활용한 콘텐츠 관리
- GitHub Actions 워크플로우로 빌드/배포 자동화
- 테마 내장 기능(검색, 다크모드, TOC)을 활용하여 추가 개발 최소화

## 기술 컨텍스트

**언어/버전**: Ruby 2.7+ (Jekyll 4.x 실행 환경)
**주요 의존성**:
- Jekyll 4.3+ (정적 사이트 생성기)
- Chirpy 테마 6.x (Jekyll 테마)
- GitHub Pages gem (배포 환경)
- Bundler (Ruby 의존성 관리)

**저장소**: 파일 시스템 기반 (Markdown 파일, YAML 설정 파일)
**테스트**:
- 로컬: `bundle exec jekyll serve` (라이브 프리뷰)
- CI: GitHub Actions에서 빌드 검증
- 수동: 브라우저 기반 UI/UX 테스트

**대상 플랫폼**:
- 호스팅: GitHub Pages (정적 파일 호스팅)
- 브라우저: 현대 브라우저 (Chrome, Firefox, Safari, Edge)
- 기기: 데스크톱, 태블릿, 모바일 (반응형)

**프로젝트 유형**: 정적 웹사이트 (Static Site Generator 기반)
**성능 목표**:
- 홈페이지 로딩: 3초 이내 (50+ 포스트)
- 검색 결과: 0.5초 이내
- 테마 전환: 0.3초 이내
- 빌드 시간: 2분 이내

**제약사항**:
- GitHub Pages 제약사항 (Jekyll 플러그인 화이트리스트)
- 클라이언트 사이드 검색 (서버리스 환경)
- 정적 파일만 지원 (동적 서버 로직 없음)
- 브라우저 LocalStorage에 테마 설정 저장

**규모/범위**:
- 예상 포스트 개수: 50-200개
- 카테고리: 5-10개
- 태그: 20-50개
- 페이지: 5-10개 (About, Archives, Categories, Tags 등)

## 헌법 검사

*게이트: Phase 0 연구 전에 통과해야 함. Phase 1 설계 후 재검사.*

**헌법 파일 상태**: 템플릿 상태 (프로젝트별 원칙 미정의)

**헌법 준수 평가**: ✅ PASS (조건부)

**분석**:
- 현재 constitution.md는 템플릿 상태로 프로젝트별 원칙이 정의되지 않음
- 이 기능은 일반적인 모범 사례를 따름:
  - **단순성**: Chirpy Starter 템플릿 활용으로 복잡도 최소화
  - **문서화**: Markdown 기반으로 문서와 콘텐츠가 동일 형식
  - **자동화**: GitHub Actions로 빌드/배포 자동화
  - **테스트 가능성**: 로컬 환경에서 프리뷰 가능
  - **유지보수성**: Jekyll 생태계의 표준 구조 사용

**조건**:
- 프로젝트 헌법이 향후 정의되면 재검토 필요
- 현 단계에서는 Jekyll/GitHub Pages 모범 사례를 준수

**Phase 1 재검사 예정**: ✓

## 프로젝트 구조

### 문서화 (이 기능)

```
specs/001-github-jekyll-chirpy/
├── spec.md              # 기능 명세서
├── plan.md              # 이 파일 (/speckit.plan 명령어 출력)
├── research.md          # Phase 0 출력 (Jekyll/Chirpy 모범 사례)
├── data-model.md        # Phase 1 출력 (콘텐츠 구조)
├── quickstart.md        # Phase 1 출력 (설정 가이드)
├── contracts/           # Phase 1 출력 (Front Matter 스키마)
│   └── post-schema.yaml
├── checklists/          # 검증 체크리스트
│   └── requirements.md
└── tasks.md             # Phase 2 출력 (/speckit.tasks 명령어)
```

### 소스 코드 (저장소 루트)

```
tech_blog/                      # 프로젝트 루트
├── _config.yml                 # Jekyll 전역 설정
├── _posts/                     # 블로그 포스트
│   └── YYYY-MM-DD-title.md    # 포스트 파일 (Front Matter + Markdown)
├── _tabs/                      # 고정 페이지
│   ├── about.md               # About 페이지
│   ├── archives.md            # 아카이브
│   ├── categories.md          # 카테고리 목록
│   └── tags.md                # 태그 목록
├── _data/                      # 데이터 파일
│   ├── contact.yml            # 연락처 정보
│   └── share.yml              # 소셜 공유 설정
├── assets/                     # 정적 리소스
│   ├── img/                   # 이미지
│   │   ├── favicons/          # 파비콘
│   │   └── posts/             # 포스트 이미지
│   ├── js/                    # JavaScript (테마 제공)
│   └── css/                   # CSS (테마 제공)
├── _site/                      # 빌드 출력 (Git ignore)
├── .github/
│   └── workflows/
│       └── pages-deploy.yml   # GitHub Actions 워크플로우
├── Gemfile                     # Ruby 의존성
├── Gemfile.lock                # 의존성 버전 락
├── index.html                  # 홈페이지 (테마 제공)
└── README.md                   # 프로젝트 README
```

**구조 결정**:
- **정적 웹사이트 구조** 선택 (Jekyll 표준)
- Chirpy 테마의 기본 구조를 따름 (변경 최소화)
- 콘텐츠(`_posts/`)와 설정(`_config.yml`)의 명확한 분리
- GitHub Actions 워크플로우는 `.github/workflows/`에 위치
- 사용자 정의 파일은 최소화하여 테마 업데이트 용이성 확보

## 복잡도 추적

*헌법 검사에서 정당화가 필요한 위반이 있는 경우에만 작성*

**현재 복잡도 위반 없음** - 표준 Jekyll + Chirpy 구조를 사용하여 복잡도가 낮음.

## Phase 0: 연구 계획

**연구 필요 항목**:

1. **Chirpy Starter 템플릿 구조**
   - 목적: 초기 설정 프로세스 이해
   - 산출물: 템플릿 사용 방법, 필수 설정 항목

2. **Jekyll Front Matter 스키마**
   - 목적: 포스트 메타데이터 구조 정의
   - 산출물: 필수/선택 필드, 검증 규칙

3. **GitHub Actions 워크플로우 설정**
   - 목적: 자동 빌드/배포 구성
   - 산출물: 워크플로우 파일 구조, 트리거 조건

4. **Chirpy 테마 커스터마이징**
   - 목적: 설정 파일을 통한 커스터마이징 범위 파악
   - 산출물: _config.yml 주요 옵션, 테마 색상/폰트 변경 방법

5. **한글 콘텐츠 지원**
   - 목적: 한글 카테고리/태그 URL 인코딩 처리
   - 산출물: URL 인코딩 전략, 한글 SEO 최적화

**연구 결과 위치**: `research.md`

## Phase 1: 설계 계획

**산출물**:

1. **data-model.md**: 콘텐츠 구조
   - Post 구조 (Front Matter 필드)
   - Category 구조
   - Tag 구조
   - Page 구조
   - Blog Configuration 구조

2. **contracts/post-schema.yaml**: Front Matter 스키마
   - 필수 필드 (title, date)
   - 선택 필드 (categories, tags, author, description, image)
   - 검증 규칙 (날짜 형식, 카테고리 형식 등)

3. **quickstart.md**: 빠른 시작 가이드
   - 초기 설정 단계 (템플릿 사용)
   - 첫 포스트 작성
   - 로컬 테스트
   - GitHub Pages 배포

## Phase 2: 작업 분해

**작업 생성 명령어**: `/speckit.tasks`

**예상 작업 카테고리**:
1. 프로젝트 초기화 (Chirpy Starter 템플릿 적용)
2. 블로그 설정 (_config.yml 수정)
3. GitHub Actions 워크플로우 구성
4. 샘플 포스트 작성
5. About 페이지 작성
6. 로컬 테스트 및 검증
7. GitHub Pages 배포 설정
8. 최종 검증 (모든 기능 테스트)

## 다음 단계

1. ✅ Phase 0: `research.md` 생성 (자동)
2. ✅ Phase 1: `data-model.md`, `contracts/`, `quickstart.md` 생성 (자동)
3. ⏳ Phase 2: `/speckit.tasks` 실행하여 `tasks.md` 생성 (수동)
4. ⏳ Phase 3: `/speckit.implement` 실행하여 작업 수행 (수동)
