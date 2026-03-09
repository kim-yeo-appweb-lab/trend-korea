# Trend Korea

대한민국 사회 이슈와 사건을 추적하고 분석하는 서비스.

## 구조

이 워크스페이스는 독립된 두 레포지토리의 공통 작업 공간입니다.

| 프로젝트        | 설명       | 스택                                | 레포지토리                                                      |
| --------------- | ---------- | ----------------------------------- | --------------------------------------------------------------- |
| trend-korea-api | 백엔드 API | FastAPI, SQLAlchemy 2.0, PostgreSQL | [GitHub](https://github.com/kim-yeo-appweb-lab/trend-korea-api) |
| trend-korea-app | 프론트엔드 | Next.js 16, React 19, TypeScript    | [GitHub](https://github.com/kim-yeo-appweb-lab/trend-korea-app) |

## 시작하기

```bash
# API
cd trend-korea-api
uv sync
uv run trend-korea-api           # localhost:8000

# App
cd trend-korea-app
pnpm install
pnpm dev                         # localhost:3100
```
