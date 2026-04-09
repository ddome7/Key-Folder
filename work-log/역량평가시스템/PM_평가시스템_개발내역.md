# 퍼포먼스 마케팅 팀원 역량 평가 시스템 — 개발내역

> 작성일: 2026-04-03
> 작성자: 퍼즐코퍼레이션
> 배포 URL: https://ddome7.github.io/PM_Evaluation/

---

## 1. 프로젝트 개요

| 항목 | 내용 |
|------|------|
| 목적 | 퍼포먼스 마케팅 팀장이 팀원을 분기·연간 단위로 정량 평가하는 웹 도구 |
| 형태 | 단일 HTML 파일 SPA (index.html, 약 7,500줄) |
| 배포 | GitHub Pages (자동 배포, master 브랜치 push 시 반영) |
| 저장소 | https://github.com/ddome7/PM_Evaluation |
| 데이터 저장 | Supabase (다기기 동기화) + localStorage (오프라인 폴백) |
| 개발 기간 | 2026-04-02 ~ 2026-04-03 (2일) |
| 총 커밋 | 57 commits |

---

## 2. 기술 스택

| 구분 | 기술 |
|------|------|
| 언어 | HTML5 / CSS3 / Vanilla JavaScript (ES6+) |
| 데이터 동기화 | Supabase (PostgreSQL REST API) |
| 로컬 저장 | localStorage (폴백) |
| 배포 | GitHub Pages + GitHub Actions CI/CD |
| 버전 관리 | Git (브랜치 보호 설정 완료) |
| 외부 라이브러리 | 없음 (zero-dependency) |

---

## 3. 평가 구조

### 3개 영역 총 100점 만점

| 영역 | 배점 | 평가 방식 |
|------|------|-----------|
| ① 매출 강화 | 60점 | 월별 취급고 수동 입력 → 등급 자동 환산 |
| ② 성과 강화 | 25점 | 6개 항목 평가 (KPI달성률 포함 일부 자동 계산) |
| ③ 역량 강화 | 15점 | 가점/감점 항목 직접 선택 |

### 최종 등급 기준
- **S** 90점 이상 / **A** 80~89점 / **B** 70~79점 / **C** 60~69점 / **D** 60점 미만

---

## 4. 주요 기능 목록

### 대시보드
- 전체 팀원 평가 현황 카드 (점수 · 등급 · 진행률 바)
- 팀 취급고 현황 테이블 (재직자 · 퇴사자 토글)
- 재직 중인 팀원만 표시 (임시·퇴사 자동 제외)
- 역량강화 승인 대기 카드 (팀원별 N개 대기 뱃지)

### 팀원 · 광고주 관리
- 팀원 추가 · 수정 · 삭제 (재직 / 퇴사 / 임시 상태)
- 광고주 추가 · 수정 (운영중 / 종료 구분, 팀별 필터 탭)
- 월별 취급고 입력 (2023~2026)
- 광고주 이관(담당자 변경) 이력 관리

### 매출 강화 평가 (60점)
- 12개월 취급고 수동 입력
- 당분기 / 전분기 취급고 자동 합산
- 취급고 상세 팝업 (돋보기 버튼) — 당분기 · 전분기 모두 지원
- 전분기 데이터 자동 불러오기 (저장된 데이터 기반)

### 성과 강화 평가 (25점)
- **KPI 달성률** (광고주 카드 단위)
  - 광고주별 KPI 항목 최대 5개 (핵심 · 보조 구분)
  - 핵심 KPI 가중 70%, 보조 KPI 가중 30%
  - 전략 중요도 가중 (S=1.5배 / A=1.0배 / B=0.7배)
  - KPI 방향성: higher(실적/목표) / lower(목표/실적), 150% 상한
  - 도전 목표 보너스 자동 계산
  - 월별 탭 UI (분기 내 각 월 독립 입력)
  - KPI 700만원 미만 광고주 자동 제외
  - 광고주별 KPI 평가 제외 기능 (빨간 제외 버튼 · 태그 표시 · 복원)
  - KPI 카드 0개 시 분모 자동 조정 (30→25점, 5개 항목 기준)
  - 이관(담당자 변경) 광고주 탭 내 사유 표시
- **예산 증액률** 자동 계산
- 기타 항목 수동 선택 (콘텐츠기획 · 보고서품질 · 무사고운영 · 협업)
- 가점 2개 항목 (+2pt × 2)

### 역량 강화 평가 (15점)
- 기본 3점 + 가점 항목 선택 + 감점 항목 선택
- 항목 직접 추가 · 삭제 가능
- 증빙 URL / 메모 첨부
- **항목별 팀장 승인 요청** (팀원) / **항목별 승인하기** (팀장)
  - 팀원: 각 항목 우측 끝 "승인 요청" 버튼 (토글, 요청됨 표시)
  - 팀장: 요청된 항목에 "승인하기" 버튼 → 역할별 체크박스 자동 체크
  - 양쪽 팀장 모두 승인 완료 시 항목 상단 고정 + 잠금

### 계정 · 권한 관리
- Supabase Auth 기반 이메일/비밀번호 로그인
- 신규 가입 → 관리자 승인 후 로그인 허용 (approval_status)
- 5단계 역할: admin / team1_lead / team2_lead / head_lead / member
- 팀원(member): 매출강화·성과강화 수정 불가, 역량강화 직접 입력 가능
- 1팀장 승인은 team1_lead만, 2팀장 승인은 team2_lead만 처리 가능

### 데이터 관리
- Supabase 자동 저장 · 로드 (다기기 동기화)
- localStorage 폴백 (오프라인 환경)
- CSV 백업 (팀원 데이터 · 광고주 데이터 개별 다운로드)
- JSON 전체 백업 · 복구 (앱 전체 데이터 스냅샷)

### 배포 · 협업
- GitHub Actions 자동 배포 (master push → Pages 반영)
- master 브랜치 보호 (direct push 차단, force push 차단)
- 로컬 pre-push 훅 (master 직접 push 시 경고 · 차단)
- 신규 feature 브랜치 생성 스크립트 (`new-feature.sh`)

---

## 5. 일자별 개발 내역

### 2026-04-02 (Day 1)

| # | 작업 내용 | 유형 |
|---|-----------|------|
| 1 | 퍼포먼스 마케팅 팀원 평가 SPA 최초 생성 | 신규 |
| 2 | 팀원 · 광고주 관리 기능 구현 | 신규 |
| 3 | localStorage 완전 연동 (URL 배포 후 데이터 영구 보존) | 신규 |
| 4 | GitHub Pages 배포용 index.html 및 .nojekyll 추가 | 인프라 |
| 5 | GitHub Actions CI/CD 워크플로우 추가 | 인프라 |
| 6 | 팀원 목록 재직자/퇴사자 토글 접기/펼치기 | 신규 |
| 7 | 팀원 목록 토글 팀별 독립 동작 수정 | 수정 |
| 8 | 팀 취급고 현황 재직자/퇴사자 토글 추가 | 신규 |
| 9 | 헤더 CSV 백업 버튼 (팀원 · 광고주 개별) | 신규 |
| 10 | CSV 백업에 광고주 월별 취급고 데이터 포함 (2023~2026) | 수정 |
| 11 | JSON 전체 백업 버튼 추가 | 신규 |
| 12 | Supabase 연동 — 다기기 데이터 동기화 (localStorage 폴백) | 신규 |
| 13 | Supabase 저장 실패 수정 (id 자동증가 충돌) | 버그픽스 |
| 14 | JSON 복구 기능 추가 | 신규 |
| 15 | JSON 복구 후 renderAll → 실제 렌더 함수들로 교체 | 버그픽스 |
| 16 | Supabase save — update 대신 delete+insert로 500 에러 해결 | 버그픽스 |
| 17 | loadAppState · beforeunload saveAppState 제거 (Supabase 덮어씌움 버그) | 버그픽스 |
| 18 | 성과강화 평가기간 外 광고주 KPI카드 생성 차단 + 예산증액률 필터 개선 | 수정 |
| 19 | restoreAdvState에서 평가기간 外 이탈 광고주 카드 필터링 | 수정 |
| 20 | KPI 카드 필터를 현 반기(H1/H2) 기준으로 변경 + 이탈 광고주 제외 | 수정 |
| 21 | 종료 광고주 KPI/예산증액 완전 제외 + 이관 광고주 이전 담당자 카드 제거 | 수정 |
| 22 | KPI 필터 분기 기준+연도 정확화 | 수정 |
| 23 | KPI 탭 활성화 이관 일정 반영 | 수정 |
| 24 | KPI 달성률 월 소진액 700만원 미만 광고주 제외 | 수정 |
| 25 | KPI 카드 이관 광고주 필터링 전면 수정 | 버그픽스 |

### 2026-04-03 (Day 2)

| # | 작업 내용 | 유형 |
|---|-----------|------|
| 1 | 도원 코드리뷰 반영 — 전체 로직 오류 수정 | 버그픽스 |
| 2 | KPI 달성률 기능 전체 제거 후 재설계 시작 | 리팩토링 |
| 3 | Service Worker 캐시 강제 초기화 (KPI 카드 잔상 제거) | 버그픽스 |
| 4 | KPI 달성률 UI 재설계 및 기능 재구현 | 신규 |
| 5 | KPI 달성률 월별 탭 UI + 800만원 미만 제외 로직 | 신규 |
| 6 | 광고주 종료 월 비활성화 및 KPI 항목 수정/추가 기능 | 신규 |
| 7 | 당분기 취급고 상세 팝업 (돋보기 버튼) | 신규 |
| 8 | KPI 평가 제외 기준 700만원으로 수정 | 수정 |
| 9 | KPI 카드 이관 정보 표시 · 광고주 목록 '이관'→'내부담당자 변경' | 수정 |
| 10 | KPI 월 탭 담당자 변경 사유 표시 | 신규 |
| 11 | 광고주 목록 모달 운영중/종료 구분 · 월평균 소진액 정렬 | 신규 |
| 12 | 종료 광고주 버튼 붉은색 스타일 구분 | 디자인 |
| 13 | 재직상태 '임시' 추가 (퇴사와 동일하게 평가 제외) | 신규 |
| 14 | 광고주 목록 팀별 필터 탭 (전체/1팀/2팀) | 신규 |
| 15 | 평가기간 변경 후 NaN 오류 수정 + 광고주 목록 팀버튼 디자인 통일 | 버그픽스 |
| 16 | 전분기 취급고 자동 모드에 돋보기(상세 팝업) 추가 | 신규 |
| 17 | 대시보드에서 임시 상태 팀원 제외 | 수정 |
| 18 | KPI 달성률 광고주별 제외(삭제) 기능 (빨간 제외 버튼) | 신규 |
| 19 | 제외 광고주 태그 표시 + 다시 추가(복원) 기능 | 신규 |
| 20 | KPI 없을 때 5개 항목 자동 환산 (분모 30→25 자동 전환) | 신규 |
| 21 | 성과강화 NaN 오류 수정 (calcPerform DOM null 체크 강화) | 버그픽스 |
| 22 | master 브랜치 보호 설정 (GitHub API) | 인프라 |
| 23 | 로컬 pre-push 훅 + new-feature.sh 스크립트 | 인프라 |
| 24 | Supabase Auth 로그인 시스템 구축 (직접 fetch, supabase-js 미사용) | 신규 |
| 25 | 신규 가입 → 관리자 승인 후 로그인 허용 (approval_status) | 신규 |
| 26 | 가입 거절 버튼 + 가입 인원 목록 (이름·메일·삭제) | 신규 |
| 27 | 권한 드롭다운 (admin/team1_lead/team2_lead/head_lead/member) | 신규 |
| 28 | 팀원(member) 평가입력 — 본인 이름만 표시 | 수정 |
| 29 | 팀원(member) 매출강화·성과강화 수정 잠금 | 수정 |
| 30 | 역량강화 1점/2점 배점 버튼 팀원 비활성화 | 수정 |
| 31 | 1팀장/2팀장 승인 체크박스 역할 분리 | 수정 |
| 32 | RLS SECURITY DEFINER 함수(`get_my_role()`)로 재귀 참조 해결 | 버그픽스 |
| 33 | 새로고침 시 로그인 화면 플래시 제거 (loading-overlay 패턴) | 버그픽스 |
| 34 | 새로고침 후 마지막 페이지 복원 (sessionStorage) | 신규 |
| 35 | 팀장이 평가 입력으로 이동 시 역량강화 섹션 자동 스크롤 | 신규 |
| 36 | 승인 완료 항목 상단 고정 + 체크 해제 잠금 (수정은 허용) | 신규 |
| 37 | 역량강화 항목별 승인 요청/승인하기 버튼 구현 | 신규 |
| 38 | 대시보드 승인 대기 카드 N개 대기 뱃지 추가 | 수정 |

---

## 6. 주요 버그 해결 이력

| 버그 | 원인 | 해결 방법 |
|------|------|-----------|
| Supabase 500 에러 | id 자동증가 컬럼 충돌 | UPSERT 대신 delete+insert 패턴으로 전환 |
| Supabase 데이터 덮어씌움 | 페이지 로드/언로드 시 빈 데이터로 save 호출 | loadAppState · beforeunload 내 불필요한 save 제거 |
| KPI 카드 잔상 | Service Worker 캐시 | 캐시 버전 업 강제 초기화 |
| 평가기간 변경 후 NaN | kpiScore · budgetGrowthScore 미초기화 상태로 연산 | `_restoreAfterQuarterSwitch()` 진입 시 0으로 초기화 |
| updateTotal() NaN | salesPt/performPt/skillPt 중 하나가 NaN | safe() 헬퍼 함수로 방어 처리 |
| calcSkill() NaN | item.points undefined 시 덧셈 연산 | `Number(item.points) \|\| 0` 방어 추가 |
| 광고주 팀버튼 동작 안 함 | HTML id `adv-team-tab-all` ↔ JS 조회 `adv-team-tab-전체` 불일치 | id를 `adv-team-tab-전체`로 통일 |
| 성과강화 점수 실시간 미반영 | calcPerform() 내 DOM null 접근 시 TypeError로 함수 중단 | 모든 DOM 접근에 null 체크 추가 |
| RLS 재귀 참조 오류 | profiles_select_admin 정책이 profiles 테이블 자기참조 | SECURITY DEFINER 함수 `get_my_role()`로 우회 |
| 로그인 화면 플래시 (F5) | login-overlay가 기본 표시 상태 | loading-overlay 추가, login-overlay를 hidden으로 초기화 |
| 로그인 버튼 멈춤 | supabase-js CDN 버전 충돌 | 직접 fetch() 호출로 전환 |

---

## 7. 데이터 구조

```
localStorage / Supabase (app_state)
├── appState
│   ├── TEAM_MEMBERS[]       — 팀원 목록 (이름·직급·상태·팀)
│   ├── ADVERTISERS[]        — 광고주 목록 (이름·팀·상태·이관이력)
│   ├── budgetData{}         — 광고주별 월별 취급고 (2023~2026)
│   ├── evalData{}           — 팀원별 평가 데이터 (분기별)
│   ├── kpiState{}           — KPI 달성률 상태 (분기·담당자별)
│   │   └── [분기_담당자ID]
│   │       ├── [광고주ID][]  — KPI 항목 배열
│   │       └── __excluded[] — 제외된 광고주 ID 목록
│   └── skillItems{}         — 역량 강화 항목 (팀원별)

Supabase 별도 테이블
├── profiles                 — 사용자 프로필 (role, team, approval_status)
└── skill_approvals          — 역량강화 항목별 승인 요청 상태
    ├── member_name
    ├── team_member_id
    ├── team
    ├── half_key             — 평가 반기 식별자
    ├── requested_at
    ├── skill_data (JSONB)   — 팀원이 제출한 역량강화 항목 스냅샷
    └── requested_item_ids (JSONB) — 승인 요청된 항목 ID 목록
```

---

## 8. 배포 구성

```
레포지토리: ddome7/PM_Evaluation (GitHub)
배포 방식:  GitHub Actions → GitHub Pages
배포 URL:   https://ddome7.github.io/PM_Evaluation/
진입점:     index.html (단일 파일)
리다이렉트: performance_evaluation.html → index.html (하위 호환)

브랜치 보호:
  - master 직접 push 차단
  - force push 차단
  - PR 필수 (리뷰 없이도 셀프 merge 가능)
```

---

## 9. 향후 고려 사항

- feature 브랜치 → PR → merge 워크플로우 정착
- 평가 결과 PDF 내보내기
- 팀원 본인 열람용 뷰 (읽기 전용 링크)
- 분기별 히스토리 비교 뷰
