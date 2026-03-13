# 📚 Notion 독서록 작성 - PRD (Product Requirements Document)

## 1. 프로젝트 개요

### 프로젝트명
Notion 독서록 작성 (Notion Books Reading Log)

### 목적
Notion을 CMS(Content Management System)로 활용하여 사용자가 읽은 책과 독서 내용을 기록하고, 체계적으로 관리할 수 있는 모던한 웹 애플리케이션을 제공합니다.

### CMS 선택 이유
- **비개발자 친화적**: Notion UI를 통해 개발 지식 없이도 콘텐츠 직접 관리 가능
- **확장성**: Notion API를 활용하여 데이터 구조 유연하게 확장 가능
- **실시간 동기화**: 웹사이트와 Notion 데이터베이스 실시간 동기화
- **비용 효율성**: 별도의 백엔드 데이터베이스 구축 불필요

---

## 2. 기술 스택

| 분류 | 기술 | 버전 |
|------|------|------|
| **프론트엔드** | Next.js | 15.5.3 |
| | React | 19.1.0 |
| | TypeScript | 5 |
| **CMS** | Notion API | Latest |
| **스타일링** | Tailwind CSS | v4 |
| | shadcn/ui | Latest (new-york style) |
| **아이콘** | Lucide React | Latest |
| **폼 처리** | React Hook Form | 7 |
| | Zod | v4 |
| **개발 도구** | ESLint | 9 |
| | Prettier | Latest |
| | Husky | Latest |
| | lint-staged | Latest |

---

## 3. Notion 데이터베이스 구조

### 테이블 스키마

| 필드명 | 타입 | 필수 | 설명 |
|--------|------|------|------|
| **Title** | Title (title) | ✅ | 책 제목 (Primary Key) |
| **Category** | Select | ✅ | 책의 카테고리 (예: 소설, 개발, 자기계발) |
| **Tags** | Multi-Select | ❌ | 책과 관련된 태그 (여러 개 선택 가능) |
| **Published** | Date (date) | ❌ | 책의 출판일 |
| **Content** | Page Content | ❌ | 독서 내용 및 감상문 본문 |
| **ReadStatus** | Select | ✅ | 읽기 상태 (읽기 시작, 읽는 중, 읽기 완료) |
| **ReadStartDate** | Date (date) | ✅ | 읽기 시작한 날 |
| **ReadEndDate** | Date (date) | ❌ | 읽기 완료한 날 (ReadStatus가 '읽기 완료'일 때만 필수) |

### 데이터 예시
```
Title: Clean Code
Category: 개발
Tags: [코딩, 파이썬]
Published: 2008-08-01
Content: 클린 코드는...
ReadStatus: 읽기 완료
ReadStartDate: 2024-01-15
ReadEndDate: 2024-02-28
```

---

## 4. 주요 기능

### 4.1 독서 내용 기록
- **기능**: 사용자가 Notion을 통해 책 정보와 독서 내용을 작성하고 저장
- **특징**:
  - 카테고리와 태그를 통한 책 분류
  - 읽기 시작, 진행 중, 완료 상태 관리
  - 읽기 시작 및 완료 날짜 자동 추적
  - 리치 텍스트 형식의 감상문 작성 지원

### 4.2 캘린더에서 간략 독서내역 조회 및 상세 내용 연결
- **기능**: 월간/주간 캘린더 뷰에서 독서 현황 한눈에 파악
- **특징**:
  - 월별/주별 독서 일정 시각화
  - 각 날짜별 읽기 시작/완료한 책 표시
  - 캘린더의 책 항목 클릭 시 상세 페이지로 이동
  - 색상으로 읽기 상태 구분 표시 (시작/진행중/완료)

### 4.3 주간/월간 독서 이력 추적
- **기능**: 기간별 독서 통계 및 이력 조회
- **특징**:
  - 주간 읽은 책 목록 및 독서량 통계
  - 월간 읽은 책 목록 및 완료율
  - 기간별 독서 시간 및 페이지 수 추적 (선택사항)
  - 카테고리별 독서 분포도

---

## 5. 화면 구성

### 5.1 홈 (Home)
- **위치**: `/`
- **표시 내용**:
  - 최근에 읽기 시작한 책 목록 (상위 5개)
  - 현재 읽고 있는 책 목록
  - 이달의 독서 통계 (읽은 책 수, 진행률)
  - 빠른 접근 링크 (캘린더, 카테고리, 통계)

### 5.2 책 상세 페이지 (Book Detail)
- **위치**: `/book/[id]`
- **표시 내용**:
  - 책 제목, 카테고리, 태그
  - 출판일, 읽기 상태
  - 읽기 시작/완료 날짜
  - 감상문 본문 (Notion 콘텐츠)
  - 뒤로가기 및 네비게이션

### 5.3 카테고리별 목록 (Category)
- **위치**: `/category/[categoryName]`
- **표시 내용**:
  - 특정 카테고리에 속한 책 목록
  - 책별 읽기 상태 배지
  - 필터링 옵션 (상태별, 날짜순)
  - 페이지네이션 (필요 시)

### 5.4 캘린더 뷰 (Calendar)
- **위치**: `/calendar`
- **표시 내용**:
  - 월별 캘린더
  - 각 날짜에 읽기 시작/완료한 책 표시
  - 상태별 색상 구분
  - 주간/월간 뷰 토글

### 5.5 통계 페이지 (Statistics)
- **위치**: `/stats`
- **표시 내용**:
  - 주간 독서 현황
  - 월간 독서 현황
  - 카테고리별 독서 분포
  - 읽기 상태별 책 수

---

## 6. MVP (Minimum Viable Product) 범위

### 포함 사항
1. ✅ **Notion API 연동**
   - Notion API 인증 및 데이터베이스 연동
   - 책 정보 조회 및 실시간 동기화

2. ✅ **글 목록 및 상세 페이지**
   - 홈 페이지: 최근 책 목록
   - 상세 페이지: 개별 책 정보 및 감상문 표시
   - 카테고리별 목록 페이지

3. ✅ **기본 스타일링**
   - Tailwind CSS를 활용한 모던 디자인
   - shadcn/ui 컴포넌트 활용

4. ✅ **반응형 디자인**
   - 모바일, 태블릿, 데스크톱 대응

### 제외 사항 (Post-MVP)
- ❌ 사용자 인증 및 계정 관리 (초기: 공개 읽기만 지원)
- ❌ 댓글 및 소셜 기능
- ❌ 고급 검색 기능
- ❌ 캐시 최적화
- ❌ SEO 최적화 (초기)

---

## 7. 구현 단계

### Phase 1: 환경 설정 및 Notion 연동
**목표**: Notion API와 프로젝트 환경 구성
- Notion API 패키지 설치 (`@notionhq/client`)
- Notion API 키 및 데이터베이스 ID 환경변수 설정
- 환경변수 검증 로직 구현
- TypeScript 타입 정의 (Notion 응답)

**결과물**: Notion API 연동 완료, 데이터 조회 함수 작성

---

### Phase 2: Notion 데이터베이스 생성 및 초기 데이터 설정
**목표**: Notion에 데이터베이스 구조 생성 및 샘플 데이터 입력
- Notion에 '독서록' 데이터베이스 생성
- 필드 설정 (Title, Category, Tags, Published, Content, ReadStatus, ReadStartDate, ReadEndDate)
- 카테고리 옵션값 설정
- 읽기 상태 옵션값 설정 (읽기 시작, 읽는 중, 읽기 완료)
- 샘플 데이터 3-5개 입력

**결과물**: Notion 데이터베이스 구조 완성, 샘플 데이터 입력

---

### Phase 3: 글 목록 페이지 및 상세 페이지 구현
**목표**: 책 데이터 표시 페이지 개발
- `/` (홈) 페이지: 최근 책 목록 표시
- `/category/[categoryName]` 페이지: 카테고리별 목록
- `/book/[id]` 페이지: 개별 책 상세 정보 및 감상문
- 페이지 레이아웃 및 네비게이션 구현
- 이미지 최적화

**결과물**: 기본 페이지 및 라우팅 완성

---

### Phase 4: 스타일링 및 UI 컴포넌트 구현
**목표**: 디자인 일관성 및 사용성 확보
- shadcn/ui 컴포넌트 활용 (Button, Card, Badge, Dialog 등)
- Tailwind CSS로 커스텀 스타일링
- 반응형 레이아웃 구현 (Mobile, Tablet, Desktop)
- 다크모드 지원 (선택사항)
- 로딩 상태 및 에러 핸들링 UI

**결과물**: 완성도 높은 UI/UX

---

### Phase 5: 최적화 및 배포 준비
**목표**: 성능 최적화 및 배포 가능 상태 확보
- 빌드 검증 (`npm run build`)
- 모든 린트/포맷 검사 통과 (`npm run check-all`)
- 페이지 로딩 성능 최적화
- 메타데이터 설정 (페이지별)
- 배포 환경 설정 (Vercel/자체 서버)

**결과물**: 배포 가능한 완성 프로젝트

---

## 8. 성공 기준

| 기준 | 상태 |
|------|------|
| Notion API 연동 완료 | ✅ |
| 글 목록 페이지 표시 | ✅ |
| 글 상세 페이지 표시 | ✅ |
| 기본 스타일링 완료 | ✅ |
| 반응형 디자인 구현 | ✅ |
| 모든 검사 통과 (`npm run check-all`) | ✅ |
| 빌드 성공 | ✅ |
| 배포 가능 | ✅ |

---

## 9. 일정 및 마일스톤

| 마일스톤 | Phase | 목표 |
|---------|-------|------|
| **Week 1** | 1-2 | Notion API 연동 완료, 데이터베이스 구성 |
| **Week 2** | 3 | 페이지 및 라우팅 구현 |
| **Week 3** | 4 | 스타일링 및 UI 완성 |
| **Week 4** | 5 | 최적화 및 배포 준비 완료 |

---

## 10. 참고 자료

### 외부 문서
- [Notion API 공식 문서](https://developers.notion.com/)
- [Next.js 15 공식 문서](https://nextjs.org/docs)
- [React 19 변경사항](https://react.dev/blog/2024/12/19/react-19)
- [Tailwind CSS 문서](https://tailwindcss.com/)
- [shadcn/ui 컴포넌트](https://ui.shadcn.com/)

### 프로젝트 내부 문서
- `@/docs/guides/nextjs-15.md` — Next.js 15.5.3 개발 가이드
- `@/docs/guides/styling-guide.md` — Tailwind CSS & shadcn/ui 스타일링 가이드
- `@/docs/guides/component-patterns.md` — 컴포넌트 패턴 가이드
- `@/docs/guides/project-structure.md` — 프로젝트 구조 안내

---

**작성일**: 2026-03-13
**최종 수정**: 2026-03-13
**상태**: Active
