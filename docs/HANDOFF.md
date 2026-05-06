# 작업 진행 핸드오프

> 다음 세션에서 이 파일을 먼저 읽고 이어가세요.
> 매 세션 종료 시 이 문서를 갱신합니다.

**마지막 갱신**: 2026-05-06
**현재 상태**: 라이브 배포 1차 완료, 두 번째 push의 자동 재배포가 적용 안 됨 (사용자 Vercel 대시보드 확인 대기 중)

---

## 1. 한 줄 현황

`otto-jet.vercel.app` 에 첫 commit (`d107f0d`) 라이브 중. 두 번째 commit (`90cac19`, SEO·OG·favicon 보강) 은 GitHub에는 push됐으나 Vercel production에 반영 안 됨 — 원인 진단 대기.

---

## 2. 이번 세션 완료 작업 (2026-05-04 ~ 2026-05-06)

### 인프라·배포
- [x] CLAUDE.md §5.4 검증 통과 (그린/세리프/압박 톤 0건)
- [x] `.gitignore` 추가 (macOS·Vercel·node·env 패턴)
- [x] git 저장소 초기화 (`main` 브랜치)
- [x] GitHub repo 연결 → `https://github.com/siwoosdev-lab/otto`
  - 인증 헤맨 이력: 대소문자 (`Otto` → `otto`), 토큰 권한 (`repo` 스코프 누락 가능성), 최종적으로 사용자 토큰 재발급 후 push 성공
  - keychain helper: `osxkeychain`, 한 번 인증 후 자동 사용
- [x] 첫 commit `d107f0d 초기 OttO 랜딩페이지 셋업` (14 files, +5172)
- [x] Vercel 첫 배포 → `https://otto-jet.vercel.app` (사용자가 GitHub Import 방식으로 직접 진행)

### 콘텐츠·코드 변경
- [x] 푸터 약관/개인정보처리방침 링크 wiring (`#` → `/terms`, `/privacy`)
- [x] 자체 `<form>` → Tally embed 교체 (form_id `VLoy9E`, dynamicHeight, transparentBackground, hideTitle, alignLeft)
- [x] 죽은 `handleSubmit` placeholder 함수 제거
- [x] OG / Twitter Card 메타 일괄 추가 (type, site_name, title, description, url, locale, twitter:card 등)
- [x] canonical URL 추가 (`https://otto.kr/`) — sitemap·robots와 일관
- [x] theme-color `#1d4ed8` 추가 (모바일 주소창)
- [x] `public/favicon.svg` 생성 (OttO 두 원 로고, viewBox 28x28)
- [x] 푸터 `사업자정보` placeholder 링크 제거 (CLAUDE.md §7.5: 줄이는 방향)
- [x] 두 번째 commit `90cac19 SEO·공유 메타 보강 + favicon 추가` push

### 라이브 점검 (otto-jet.vercel.app, 첫 commit 기준)
- ✅ HTTP 200 + vercel.json 보안 헤더 (HSTS, X-Frame-Options, X-Content-Type-Options, Permissions-Policy, Referrer-Policy)
- ✅ cleanUrls (`/privacy`, `/terms` 모두 200, `.html` 없이 동작)
- ✅ Tally embed 마크업 라이브 반영
- ❌ favicon.ico, og-image.png, apple-touch-icon.png 모두 404 (자산 미생성)
- ⚠️ robots.txt / sitemap.xml은 `https://otto.kr` 가리킴 (도메인 연결 후 일관됨)

---

## 3. 🚫 지금 막혀있는 것

### Vercel 자동 재배포 미작동

두 번째 commit `90cac19` push 후 7시간+ 지났는데도 `otto-jet.vercel.app` edge가 옛 콘텐츠 응답 (etag 그대로, age 26637+).

**진단 가설** (확률 순):
1. Vercel ↔ GitHub webhook 연결 끊김
2. 빌드 실패 (Build Logs 미확인)
3. Production Branch 설정이 `main` 아님

**다음 세션 시작 시 사용자에게 확인 부탁할 항목**:
- Vercel 대시보드 → otto 프로젝트 → **Deployments** 탭
- 최상단 row의 commit hash와 status
  - `90cac19` Ready → Promote to Production
  - `90cac19` Error/Failed → Build Logs 첫 에러
  - `d107f0d` 한 줄만 → webhook 미수신 (Project Settings → Git → Connected Repo 확인 + Redeploy 강제)

**우회 옵션**: Deployments 탭 우측 상단 **Redeploy** 버튼 → Use existing Build Cache 해제 → Redeploy

---

## 4. 다음 세션 시작 시 — 어디서 이어갈지

1. 위 §3 Vercel 진단 결과 받아서 해결
2. `90cac19` 라이브 적용 확인 (OG 메타, favicon.svg, 사업자정보 링크 제거 모두 반영)
3. 그 다음 **Tally 알림 설정** (사용자 액션):
   - Tally 대시보드 → 폼 → Integrations → Email notification (대표 메일로 즉시 알림)
   - 선택: Slack / Discord webhook
4. 그 다음 **분석 도구 연결** — Cloudflare Web Analytics 또는 Vercel Analytics (CLAUDE.md §6.3)
5. 그 다음 **OG 이미지(1200x630 PNG) 제작** — 텍스트 카드만으로는 카카오톡 공유 임팩트 약함
6. 마지막에 **도메인 `otto.kr` 연결** (사용자가 명시적으로 "도메인은 마지막"이라 함)

---

## 5. 운영 정보 한 페이지

| 항목 | 값 |
|---|---|
| 라이브 URL | https://otto-jet.vercel.app |
| 미래 도메인 | https://otto.kr (미구매·미연결) |
| GitHub repo | https://github.com/siwoosdev-lab/otto |
| Vercel 프로젝트 | otto |
| Tally form_id | VLoy9E (https://tally.so/r/VLoy9E) |
| Tally embed URL | https://tally.so/embed/VLoy9E |
| 미래 운영 메일 | hello@otto.kr (미셋업) |
| 카카오 채널 | @otto (미개설) |
| Phase 1 월 비용 목표 | < ₩14,000 |
| 디자인 토큰 | CLAUDE.md §3 (코발트 블루 #1d4ed8, Pretendard, Zinc 900 ink) |

---

## 6. 알려진 미완 자산 / 잠재 이슈

- ⚠️ `public/favicon.ico` 없음 — SVG favicon은 모던 브라우저만, IE/구형은 404. 큰 문제 아님
- ⚠️ `public/og-image.png` (1200x630) 없음 — 현재 OG 카드는 텍스트만. 카카오톡 공유 시 이미지 카드 안 뜸
- ⚠️ `public/apple-touch-icon.png` 없음 — iOS 홈 화면 추가 시 기본 아이콘 사용됨
- ⚠️ vercel.app 도메인이 검색엔진에 인덱싱될 가능성 — 도메인 연결 전까지 SEO 분산. 정 신경 쓰이면 `X-Robots-Tag: noindex` 헤더 추가 가능
- ⚠️ Tally 폼 알림(이메일·Slack) 미설정 — 신청 와도 사용자가 모름

---

## 7. 다음 세션 시작 시 자동 진행 가능 작업 (사용자 액션 불필요)

배포 문제 해결되면 즉시:
- README.md 업데이트 (실제 라이브 URL, repo URL 반영)
- TODO.md 추가 항목 체크
- 최신 main 동기화
- §6 미완 자산 중 코드/SVG로 가능한 것 (예: apple-touch-icon SVG 변형 등)

사용자 액션이 필요한 작업:
- Vercel 대시보드 확인 (§3)
- Tally 알림 설정 (§4-3)
- 도메인 구매·연결 (§4-6)
- OG 이미지 PNG 디자인 (§4-5, 1200x630 디자인 작업 필요)

---

## 부록: 자주 쓰는 명령

```bash
# 로컬 미리보기
python3 -m http.server 3000 --directory public

# CLAUDE.md §5.4 검증
grep -n "047857\|10b981\|059669\|ecfdf5\|d1fae5" public/index.html
grep -n "Fraunces\|var(--serif)\|font-style.*italic" public/index.html
grep -n "한정 모집\|10팀\|7팀\|50% 할인\|지금만" public/index.html

# 라이브 헤더 점검
curl -sI https://otto-jet.vercel.app/

# 라이브 OG 메타 확인
curl -s https://otto-jet.vercel.app/ | grep -E "og:|twitter:|canonical|theme-color|favicon"
```
