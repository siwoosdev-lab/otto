# 작업 진행 핸드오프

> 다음 세션에서 이 파일을 먼저 읽고 이어가세요.
> 매 세션 종료 시 이 문서를 갱신합니다.

**마지막 갱신**: 2026-05-06 (개인정보 동의 안내 추가)
**현재 상태**: 라이브 정상. Tally 알림 동작 확인. 개인정보 수집·이용 안내 박스 추가됨. Vercel Analytics·Tally 동의 체크박스·OG PNG·도메인 대기.

---

## 1. 한 줄 현황

`otto-jet.vercel.app` 라이브. Tally 폼 + 알림 동작. 폼 영역에 PIPA 4항목 안내 박스(수집 항목·목적·기간·거부 권리) + privacy.html 링크 강조.

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

## 3. 🚧 지난 이슈 — 해결 완료

### Vercel Hobby Plan의 commit author 차단

`90cac19` push 후 자동 재배포 차단됨. Vercel 대시보드 메시지:
> "Deployment was blocked because the commit author did not have contributing access. The Hobby Plan does not support collaboration for private repositories."

**원인**: GitHub repo가 Private이었고, commit 메시지의 `Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>` 트레일러를 Vercel이 외부 collaborator로 인식 → Hobby plan 정책상 차단.

**해결**: GitHub repo를 **Public으로 전환** (Settings → Danger Zone → Change repository visibility). Public repo는 Vercel Hobby에서 collaboration 제한 없음. 빈 commit (`4690177 Public 전환 후 Vercel 재배포 트리거`) push로 webhook 재트리거 → 15초 만에 새 빌드 라이브.

**향후 권고**:
- 이 repo는 Public 유지 (콘텐츠 어차피 공개됨, 비밀값 없음)
- 또는 옵션 C: `git config user.email`을 Vercel 계정 이메일과 일치시키기 (Private 전환 시 필요)
- Co-Author 트레일러는 Private 전환 시에만 다시 문제. Public 동안은 무관.

---

## 4. 다음 작업 우선순위

### 완료
- [x] **Tally 알림** (이메일 알림 동작 확인)
- [x] **개인정보 수집·이용 안내 박스** (폼 위에 PIPA 4항목 + privacy 링크)

### 진행 중·대기 (사용자 액션 필요)

1. **Tally 폼에 필수 동의 체크박스 추가** — Tally 대시보드 → 폼 편집 → "Multiple Choice" 또는 "Yes/No" 필드 추가
   - 라벨: `[필수] 개인정보 수집·이용에 동의합니다`
   - Required: ON
   - 폼 가장 위 또는 가장 아래에 배치 권장
   - Tally는 외부 API 미지원 → Claude가 못 함, 사용자만 가능
2. **Vercel Web Analytics 활성화** — Vercel 대시보드 → otto → Analytics 탭 → Enable
   - 활성화 끝나면 Claude가 `<script defer src="/_vercel/insights/script.js"></script>` 인젝션
3. **OG 이미지 (1200x630 PNG)** — 디자인 작업 (Figma/Canva)
4. **도메인 `otto.kr` 구매·연결** (마지막)

### Privacy 페이지 placeholder 채우기 (사업자 등록 후)

[public/privacy.html](public/privacy.html) 의 다음 항목은 사업자 등록·법무 검토 후 갱신 필요:
- 시행일 (현재 `2026년 ○월 ○일`)
- 책임자 이름·연락처 (현재 `○○○`, `010-○○○○-○○○○`)
- 회사 정식 명칭 (현재 단순히 "OttO")

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
