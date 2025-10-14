---
title: "내가 사용하는 기술 스택"
date: 2025-10-17 09:00:00 +0900
categories: [Development, Tools]
tags: [tech-stack, productivity, development]
description: "개발에 사용하는 주요 기술 스택과 도구들을 정리했습니다."
---

## 소개

개발자로서 효율적으로 일하기 위해 다양한 기술과 도구를 사용하고 있습니다. 이 포스트에서는 제가 주로 사용하는 기술 스택을 분야별로 정리해보겠습니다.

## 프로그래밍 언어

### Python 🐍

주력 언어로 사용하고 있습니다.

**사용 분야:**
- 백엔드 API 개발 (Django, FastAPI)
- 데이터 분석 및 처리
- 자동화 스크립트
- 머신러닝 프로젝트

**선호하는 이유:**
- 깔끔한 문법과 높은 생산성
- 풍부한 라이브러리 생태계
- 다양한 분야에 활용 가능

### JavaScript/TypeScript 📜

프론트엔드 및 Node.js 백엔드에 사용합니다.

**사용 분야:**
- React 기반 웹 애플리케이션
- Node.js API 서버
- 브라우저 자동화

## 백엔드 프레임워크

### Django

| 항목 | 설명 |
|------|------|
| **사용 버전** | Django 4.x |
| **강점** | ORM, Admin, 보안 |
| **주요 프로젝트** | REST API, CMS |

**핵심 기능:**
```python
# Django ORM 예제
from django.db import models

class BlogPost(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)

    class Meta:
        ordering = ['-created_at']

    def __str__(self):
        return self.title
```

### FastAPI

최근 프로젝트에서 적극 도입 중입니다.

**장점:**
- 빠른 성능 (async/await 지원)
- 자동 API 문서 생성
- Pydantic을 통한 타입 검증

```python
# FastAPI 예제
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float

@app.post("/items/")
async def create_item(item: Item):
    return {"name": item.name, "price": item.price}
```

## 프론트엔드

### React ⚛️

현대적인 웹 UI 개발에 사용합니다.

**주요 라이브러리:**
- React Router - 라우팅
- Redux Toolkit - 상태 관리
- React Query - 서버 상태 관리
- Tailwind CSS - 스타일링

## 데이터베이스

### PostgreSQL 🐘

주력 관계형 데이터베이스입니다.

**선택 이유:**
- 강력한 SQL 기능
- JSON 지원
- 안정성과 성능
- 풍부한 확장 기능

### Redis

캐싱과 세션 관리에 사용합니다.

**활용 사례:**
- API 응답 캐싱
- 세션 저장소
- 실시간 기능 (Pub/Sub)
- 작업 큐 (Celery와 함께)

## DevOps 및 인프라

### Docker 🐳

모든 프로젝트를 컨테이너화하여 관리합니다.

```yaml
# docker-compose.yml 예제
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://db/mydb
    depends_on:
      - db

  db:
    image: postgres:15
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### GitHub Actions

CI/CD 파이프라인을 구축합니다.

**자동화 작업:**
- 테스트 실행
- 린팅 및 포맷팅 검사
- 도커 이미지 빌드
- 배포 자동화

## 개발 도구

### VS Code

주력 에디터로 사용합니다.

**필수 확장:**
- Python
- Pylance
- ESLint
- Prettier
- GitLens
- Docker

### Git

버전 관리의 핵심입니다.

**자주 사용하는 명령어:**
```bash
# 일상적인 워크플로우
git status
git add .
git commit -m "feat: 새 기능 추가"
git push origin main

# 브랜치 관리
git checkout -b feature/new-feature
git merge develop
```

## 협업 도구

| 도구 | 용도 | 선호도 |
|------|------|--------|
| GitHub | 코드 저장소 | ⭐⭐⭐⭐⭐ |
| Notion | 문서화 | ⭐⭐⭐⭐⭐ |
| Slack | 팀 커뮤니케이션 | ⭐⭐⭐⭐ |
| Jira | 이슈 트래킹 | ⭐⭐⭐ |

## 학습 중인 기술

계속해서 새로운 기술을 학습하고 있습니다:

- [ ] Kubernetes - 컨테이너 오케스트레이션
- [ ] GraphQL - API 쿼리 언어
- [ ] Next.js - React 프레임워크
- [ ] Rust - 시스템 프로그래밍

## 기술 스택 선택 기준

새로운 기술을 도입할 때 다음 기준을 고려합니다:

1. **팀 생산성**: 학습 곡선과 개발 속도
2. **커뮤니티**: 활발한 커뮤니티와 문서화
3. **안정성**: 프로덕션 환경에서의 검증
4. **유지보수성**: 장기적인 관리 용이성
5. **확장성**: 프로젝트 성장에 대응 가능

## 결론

기술 스택은 프로젝트 요구사항과 팀 상황에 따라 유연하게 선택해야 합니다. 위에 나열한 기술들은 현재 제가 사용하는 것들이며, 계속해서 학습하고 개선해 나가고 있습니다.

여러분은 어떤 기술 스택을 사용하시나요? 댓글로 공유해주세요!

---

**관련 링크:**
- [Python 공식 문서](https://docs.python.org/)
- [Django 프로젝트](https://www.djangoproject.com/)
- [React 공식 문서](https://react.dev/)
- [Docker 문서](https://docs.docker.com/)
