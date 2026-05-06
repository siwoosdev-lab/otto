# CLAUDE.md

> 이 파일은 Claude Code가 OttO 프로젝트에서 작업할 때마다 읽는 **프로젝트 컨텍스트**입니다.
> 작업 전에 반드시 이 파일을 먼저 읽고, 여기 명시된 원칙을 따르세요.

---

## 0. 가장 중요한 규칙 (TL;DR)

다른 모든 규칙보다 우선합니다.

1. **만들지 마세요.** 가능한 한 새 코드를 짜지 말고, 기존 도구·SaaS·노코드를 조립하세요.
2. **무료 티어를 최우선으로.** Vercel, Supabase, Tally, Cloudflare 무료 티어 한도 안에서 해결하세요.
3. **단일 HTML 파일 우선.** Next.js 같은 프레임워크는 정말 필요할 때만. 현재 `otto_landing.html` 한 파일이면 충분합니다.
4. **디자인 토큰 절대 임의 변경 금지.** 색상·폰트·간격은 `OttO_랜딩페이지_설계도.md` §4를 따르세요.
5. **카피 임의 변경 금지.** 헤드라인·CTA·슬로건은 설계도 부록 B를 따르세요. 톤이 흔들리면 신뢰가 무너집니다.
6. **모든 비용 발생 작업 전 사용자에게 확인.** 도메인 구매, 유료 SaaS 가입, AI API 호출 비용이 큰 배치 작업 등.

---

## 1. 프로젝트 개요

### 1.1 정체성
- **이름**: OttO (오토)
- **한 줄 정의**: 소상공인·학원·중소사업장의 반복 업무를 진단하고 AI로 자동화해주는 B2B SaaS + 컨설팅 하이브리드
- **슬로건**: "반복은 OttO에게."
- **타겟**: 직원 1~20명 사업장 운영자 ("디지털은 어렵지만 시간은 부족한 사장님")

### 1.2 운영 형태
- **운영자**: 1인 (대표 = 영업 + 기술 + CS)
- **단계**: 검증 단계 (Phase 1, 0~3개월 차)
- **현재 고객 수**: 0명 → 첫 5팀 시범 운영 목표

### 1.3 사이트의 단일 목적
**무료 진단 신청 폼 제출 = 모든 KPI의 원점.** 다른 모든 것은 부차적입니다.

---

## 2. 프로젝트 파일 구조

```
otto/
├── CLAUDE.md                              ← 이 파일
├── README.md                              ← 사용자/방문자용 (간단)
├── docs/
│   ├── OttO_사업계획서.md                 ← 비즈니스 모델 전체
│   ├── OttO_사이트_개발계획서.md           ← 기술 스택·인프라 계획
│   └── OttO_랜딩페이지_설계도.md           ← 디자인 시스템·컴포넌트 명세
├── public/
│   ├── index.html                        ← 메인 랜딩 페이지 (= otto_landing.html)
│   ├── favicon.ico
│   ├── apple-touch-icon.png
│   ├── og-image.png                      ← 1200x630 소셜 공유 이미지
│   ├── robots.txt
│   ├── sitemap.xml
│   └── privacy.html                      ← 개인정보처리방침
│   └── terms.html                        ← 이용약관
├── scripts/
│   └── (필요 시 빌드·배포 스크립트)
└── .gitignore
```

> Phase 1에서는 `public/` 안에 정적 HTML 파일들만 두고 Vercel/Cloudflare Pages에 그대로 배포합니다. 빌드 단계 없음.

---

## 3. 디자인 시스템 (절대 임의 변경 금지)

### 3.1 컬러 — 코발트 블루 + 살짝 따뜻한 블랙

**브랜드 컬러:**
```
--otto:       #1d4ed8   (메인)
--otto-2:     #2563eb   (그라디언트 시작점)
--otto-3:     #60a5fa   (라이트, 다크 위 강조)
--otto-deep:  #1e3a8a   (배경 글로우)
--otto-soft:  #dbeafe   (형광펜 하이라이트)
--otto-bg:    #eff6ff   (배지 배경)
```

**중성 컬러:**
```
--bg:         #ffffff
--bg-2:       #fafafa
--surface-2:  #f4f4f5
--ink:        #18181b   (Zinc 900, 살짝 따뜻한 블랙)
--ink-2:      #27272a
--muted:      #52525b
--muted-2:    #71717a
--muted-3:    #a1a1aa
--line:       #e4e4e7
--line-strong:#d4d4d8
```

**금지 사항:**
- 그린·오렌지·퍼플 등 다른 강조색 도입 금지 (예외: 자동화 "성공" 시맨틱 그린 #ecfdf5/#047857)
- 배경에 따뜻한 미색(#fafaf7 등) 사용 금지 — 차가운 화이트만
- `#000000` 순수 검정 금지 — `#18181b` 사용

### 3.2 타이포그래피 — Pretendard만

```
--sans: 'Pretendard Variable', Pretendard, 'Inter', system-ui, sans-serif;
--mono: 'JetBrains Mono', ui-monospace, monospace;
```

**금지 사항:**
- **세리프 폰트 절대 사용 금지** (Fraunces, Noto Serif KR 등)
- **이탤릭 사용 금지** — 강조는 굵기(weight)와 컬러로만
- 영문 전용 폰트 단독 사용 금지 (한글이 깨짐) — 항상 Pretendard 우선

**굵기 점프 원칙:** 500 → 600 → 800 (작은 점프 금지)

### 3.3 톤 가이드

**해도 되는 것:**
- 짧고 단호한 문장: "사장님은 두 번 일하지 않습니다."
- 사장님의 고됨 인정: "혼자 하지 마세요."
- 구체적 숫자: "주 12시간 절감", "1~2주 셋업"
- 한국 비즈니스 어휘: "사장님", "가게", "직원", "본업"

**하지 말 것:**
- 영어 마케팅 용어 남발 ("스케일링", "그로스 해킹")
- 과장 ("혁명", "최고", "유일한")
- 한정 모집·긴박감 압박 ("지금 안 하면 손해!")
- 기술 자랑 ("GPT-4 기반의 RAG 아키텍처")
- 학술적 어조 ("귀하의 사업체를 위한 솔루션")

상세는 `docs/OttO_랜딩페이지_설계도.md` §3.3 참조.

---

## 4. 기술 스택 (Phase 1 — 무료 우선)

### 4.1 현재 스택 (월 비용 < 1만 원)

| 영역 | 도구 | 무료 한도 | 월 예상 비용 |
|------|------|----------|------------|
| 호스팅 | **Vercel** Hobby | 100GB 대역, 무제한 사이트 | 0원 |
| 또는 | **Cloudflare Pages** | 무제한 요청, 500 빌드/월 | 0원 |
| 도메인 | 가비아·후이즈 (.kr) | — | 연 1.5~2만 원 (월 1,500원) |
| 폼 백엔드 | **Tally.so** | 무제한 폼·응답 | 0원 |
| DB | **Supabase** Free | 500MB DB, 50K MAU | 0원 |
| 메일 | **Google Workspace** Business Starter | 14일 무료 → 월 7,500원 | 7,500원 |
| 알림톡 | 카카오 비즈메시지 | 건당 7~12원 | 사용량 기반 (월 5천 원 미만) |
| 분석 | **Plausible** Self-hosted 또는 Cloudflare Web Analytics | — | 0원 |
| 에러 모니터링 | **Sentry** Free | 5K 에러/월 | 0원 |
| **합계** | | | **약 9~14,000원/월** |

### 4.2 Phase 1 절대 도입 금지 (비용·복잡도 증가)

- ❌ Next.js (정적 HTML로 충분)
- ❌ React·Vue·Svelte (상호작용은 vanilla JS로)
- ❌ npm 패키지 (CDN 사용)
- ❌ 빌드 시스템 (Webpack, Vite 등)
- ❌ 자체 서버 (AWS, GCP)
- ❌ Notion API 결제 (Tally + Supabase로 충분)
- ❌ 유료 OpenAI/Claude API의 자동 호출 워크플로우 (현재 시점에는 수동 처리)
- ❌ Stripe 결제 (한국 카드 안 됨, 토스페이먼츠도 매출 발생 후 도입)

### 4.3 Phase 2 (4~9개월, 고객 30팀 도달 시) 도입 검토

다음 도구들은 **고객 30팀 이상** 도달했을 때만 검토합니다. 그 전엔 수동으로 처리.

- **n8n** Self-hosted (자동화 워크플로우, 월 1만 원 VPS)
- **Claude API** (진단 리포트 자동 생성)
- **Resend** 트랜잭션 메일 (월 100건 무료, 이후 $20)
- **Cal.com** Self-hosted (예약 시스템)

### 4.4 외부 라이브러리 사용 규칙

랜딩 페이지는 **CDN만** 사용합니다. npm 설치 금지.

```html
<!-- 허용된 CDN -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/variable/pretendardvariable.min.css" />
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600&family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
```

새로운 라이브러리 추가가 필요하면 **사용자에게 먼저 확인**하세요.

---

## 5. 작업 시 따라야 할 규칙

### 5.1 작업 시작 전 필수 체크

Claude Code가 작업을 시작하기 전에 항상 다음을 확인합니다.

1. **이 파일(CLAUDE.md)을 끝까지 읽었는가?**
2. **`docs/OttO_랜딩페이지_설계도.md`를 참조했는가?** (디자인 변경 시)
3. **`docs/OttO_사업계획서.md`를 참조했는가?** (비즈니스 결정 시)
4. **사용자 요청이 명확한가?** 모호하면 질문하기 전에 추측하지 말 것.
5. **이 작업이 비용을 발생시키는가?** 발생시키면 사용자 승인 받기.

### 5.2 코드 작성 규칙

**HTML/CSS/JS:**
- 모든 색상·간격·폰트는 CSS 변수 (`var(--otto)`) 사용. 하드코딩 금지.
- 인라인 스타일 최소화. 부득이한 경우 CSS 변수만 사용.
- 시맨틱 HTML 우선 (`<section>`, `<nav>`, `<main>`, `<article>`).
- JavaScript는 외부 의존성 없이 vanilla로.
- ID는 케밥 케이스 (`saved-hours`), 클래스도 케밥 케이스 (`mock-savings`).
- 모든 인터랙티브 요소는 키보드 접근 가능해야 함.

**파일 분리:**
- 단일 HTML 파일 유지 (현재 `otto_landing.html`). 분리하지 마세요.
- 1500줄을 넘기면 그때 검토.

**주석:**
- 한국어로 작성.
- "왜"를 설명. "무엇"을 설명하지 마세요 (코드가 이미 설명함).
- 섹션 구분 주석은 `/* ========== 섹션명 ========== */` 형식.

### 5.3 변경 작업 우선순위

사용자가 변경을 요청하면 다음 순서로 처리:

1. **`str_replace`로 부분 수정** (가장 안전)
2. **여러 `str_replace` 조합**
3. **파일 전체 재작성** (위 두 방법이 불가능할 때만)

> 파일 재작성은 디자인 일관성을 깨뜨릴 위험이 큽니다. 가능한 한 부분 수정으로 처리하세요.

### 5.4 검증 절차

변경 후 항상 다음을 확인:

```bash
# 그린 색상 흔적 확인 (있으면 안 됨)
grep -n "047857\|10b981\|059669\|ecfdf5\|d1fae5" public/index.html

# 세리프 흔적 확인 (있으면 안 됨)
grep -n "Fraunces\|var(--serif)\|font-style.*italic" public/index.html

# 한정 모집/할인 압박 톤 확인 (있으면 안 됨)
grep -n "한정 모집\|10팀\|7팀\|50% 할인\|지금만" public/index.html
```

이 세 검증은 색상/폰트/톤의 일관성을 지키기 위해 매번 실행합니다.

---

## 6. 비용 최저화 운영 가이드

### 6.1 호스팅 — Vercel Hobby (무료)

```bash
# 1. GitHub 저장소 생성 후 코드 푸시
git init
git add .
git commit -m "Initial OttO landing"
git remote add origin https://github.com/YOUR_USER/otto.git
git push -u origin main

# 2. Vercel 대시보드에서 Import Project → GitHub 저장소 선택
# 3. Framework Preset: "Other" 선택 (Next.js 아님)
# 4. Root Directory: "public"
# 5. Build Command: 비워둠
# 6. Output Directory: 비워둠
# 7. Deploy
```

도메인 연결: Vercel Project Settings → Domains → `otto.kr` 추가 → 가비아에서 네임서버를 Vercel로 변경 또는 A 레코드 추가.

### 6.2 폼 — Tally + Supabase (무료)

**Tally 폼 생성:**
1. tally.so 가입 (무료)
2. "Untitled Form" → 4개 필드 추가 (Name, Phone, Industry, Memo)
3. Settings → Integrations → Webhook 활성화
4. Webhook URL: Supabase Edge Function URL (아래 참조)

**현재 HTML의 폼을 Tally embed로 교체:**

```html
<!-- 기존 <form>...</form> 제거하고 아래로 교체 -->
<iframe data-tally-src="https://tally.so/embed/YOUR_FORM_ID?alignLeft=1&hideTitle=1"
  loading="lazy" width="100%" height="500" frameborder="0"
  marginheight="0" marginwidth="0" title="OttO 무료 진단 신청"></iframe>
<script src="https://tally.so/widgets/embed.js"></script>
```

**Supabase 셋업 (자동 저장):**
```sql
-- Supabase SQL Editor에서 실행
create table leads (
  id uuid default gen_random_uuid() primary key,
  name text not null,
  phone text not null,
  industry text,
  memo text,
  source text default 'landing',
  status text default '신규',
  created_at timestamp with time zone default now()
);

-- RLS는 일단 비활성. Phase 2에서 정책 추가.
```

Tally Webhook → Supabase Edge Function (또는 Tally Notion 연동을 임시로 사용):
- 가장 단순한 방법: **Tally → 이메일 알림** (대표 메일로 자동 발송)
- Phase 2에서 Webhook으로 Supabase 자동 저장 전환

### 6.3 분석 — Cloudflare Web Analytics (무료, privacy-friendly)

```html
<!-- </body> 직전에 추가 -->
<script defer src='https://static.cloudflareinsights.com/beacon.min.js'
  data-cf-beacon='{"token": "YOUR_TOKEN"}'></script>
```

GA4를 안 쓰는 이유: 쿠키 동의 배너가 필요해지고, 한국 개인정보보호법 대응이 복잡합니다. Cloudflare Web Analytics는 쿠키 안 쓰고 무료입니다.

### 6.4 알림 — 카카오 비즈메시지 + 자동 응답

**대표에게 새 신청 알림:**
- Tally → 이메일 (즉시)
- 또는 Tally → Slack/Discord Webhook (즉시 푸시)

**신청자에게 자동 응답:**
- Phase 1: Tally의 "감사 메시지" 페이지로 충분
- Phase 2: 카카오 알림톡 자동 발송 (건당 7~12원)

### 6.5 비용 알림 설정 (필수)

서비스마다 비용 알림을 설정해서 예상 외 청구를 방지합니다.

- **Vercel**: 결제 정보 등록 안 함 (Hobby는 결제 정보 없으면 자동으로 무료 한도 내에서만 동작)
- **Supabase**: Project Settings → Billing → Usage 알림 켬
- **Cloudflare**: 결제 정보 등록 안 함
- **Anthropic API** (Phase 2): Usage Limits 설정, 일일 한도 $5 같은 식으로

### 6.6 무료 한도 초과 시 행동 강령

만약 어떤 무료 티어가 한도에 가까워지면:

1. **즉시 사용자에게 알림** (Claude Code가 작업 중 발견 시)
2. **유료 전환 전, 다른 무료 도구로 마이그레이션 검토**
3. **유료 전환이 불가피하면 가장 저렴한 플랜부터**

예: Vercel Hobby (무료) → Vercel Pro ($20/월) 가기 전에 Cloudflare Pages 또는 GitHub Pages로 마이그레이션 검토.

---

## 7. 자주 하는 작업 패턴

### 7.1 카피 변경

```
"히어로 헤드라인을 X로 바꿔줘"
→ str_replace로 hero h1 부분 직접 수정
→ docs/OttO_랜딩페이지_설계도.md 부록 B의 카피 라이브러리도 함께 업데이트
→ 변경 후 grep으로 다른 곳에 같은 카피 있는지 확인
```

### 7.2 색상 변경

**절대 직접 컬러 헥스를 수정하지 말고, CSS 변수만 수정:**

```css
:root {
  --otto: #1d4ed8;  /* 여기만 수정. 모든 곳에 자동 반영 */
}
```

만약 인라인 rgba가 박혀있다면(과거 변경 흔적), 다음과 같이 검색해서 모두 잡아냄:

```bash
grep -n "rgba(.*),.*[0-9])" public/index.html | grep -v "var("
```

### 7.3 새 섹션 추가

새 섹션 추가 시 반드시:
1. `docs/OttO_랜딩페이지_설계도.md` §6에 사양을 먼저 추가
2. 디자인 토큰만 사용하여 구현
3. 기존 섹션 패턴(`section > .container > .section-head + content`)을 따를 것

### 7.4 모듈 라이브러리 카드 추가

새 업종 카드 추가 시:

```html
<div class="industry-card">
  <div class="ic-head">
    <div class="info">
      <h3>업종명</h3>
      <p>ENGLISH_LABEL</p>
    </div>
    <div class="ic-icon">이모지</div>
  </div>
  <ul>
    <li><svg width="12" height="12" viewBox="0 0 12 12" fill="none">
      <path d="M2 6L5 9L10 3" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
    </svg>모듈 설명</li>
    <!-- ... -->
  </ul>
  <div class="ic-foot">
    <span>모듈 N개</span>
    <strong>주 Nh 절감 ↓</strong>
  </div>
</div>
```

### 7.5 폼 필드 변경

폼 필드는 **추가하지 말고 줄이는 방향으로** 검토. 4개 이상이면 전환율 떨어짐.

만약 추가가 필요하면:
1. 사용자에게 정말 필요한지 확인
2. `optional`로 처리 (required 추가 금지)
3. Tally 폼도 함께 업데이트

---

## 8. 참조 문서 우선순위

작업 중 의문이 생기면 다음 순서로 확인:

1. **CLAUDE.md** (이 파일) — 작업 규칙·비용 정책
2. **docs/OttO_랜딩페이지_설계도.md** — 디자인 시스템·컴포넌트 사양
3. **docs/OttO_사이트_개발계획서.md** — 인프라·기술 결정
4. **docs/OttO_사업계획서.md** — 비즈니스 모델·전략

이 4개 문서끼리 충돌이 있으면 **CLAUDE.md가 우선**합니다.

---

## 9. 절대 하면 안 되는 것

- ❌ `OttO_랜딩페이지_설계도.md`의 디자인 토큰을 임의 변경
- ❌ "혼자 하지 마세요. OttO가 함께합니다." 같은 핵심 카피를 임의 변경
- ❌ 한정 모집·할인 압박 톤 도입
- ❌ 새 npm 패키지·프레임워크 도입
- ❌ 사용자 데이터를 OttO 서버에 직접 저장 (사장님 클라우드에만 저장)
- ❌ 유료 SaaS를 사용자 승인 없이 결제
- ❌ AI API를 대량 호출 (특히 OpenAI GPT-4, Claude Opus 같은 고가 모델)
- ❌ 데이터베이스 스키마 변경 (Phase 2까지)
- ❌ HTML 파일을 여러 개로 쪼개기
- ❌ 영문 단독 페이지 만들기 (현재 한국 시장 집중)

---

## 10. 자주 묻는 질문 (Claude Code용)

**Q. 사용자가 새 기능을 요청했는데 비용이 발생할 것 같습니다.**
A. 먼저 무료로 가능한지 확인. 안 되면 사용자에게 "이 작업은 X원 발생 예상, 진행할까요?" 확인 후 진행.

**Q. 디자인 토큰을 살짝 바꾸면 더 좋을 것 같습니다.**
A. 임의 변경 금지. 사용자에게 제안하되, 승인 후에만 변경. 변경 시 `OttO_랜딩페이지_설계도.md`도 함께 업데이트.

**Q. 사용자가 Next.js로 마이그레이션을 요청했습니다.**
A. 현재 정적 HTML로 충분한지 먼저 확인. Next.js 도입은 다음 중 하나가 충족될 때만 권장:
- 동적 라우팅 필요 (예: /industries/학원 같은 개별 페이지)
- 서버 사이드 로직 필요 (인증, DB 직접 호출)
- 50개 이상의 페이지

**Q. 사용자가 영어 버전을 요청했습니다.**
A. 현재는 한국 시장 집중 단계. 사용자에게 "현재 한국 사장님 100팀 확보가 우선이며, 영문 페이지는 Phase 3에 적절합니다. 그래도 진행할까요?" 확인.

**Q. 폼 필드 추가 요청이 왔습니다.**
A. 4개 초과 시 전환율 저하 경고. 정말 필요한지 확인하고, 가능한 한 진단 인터뷰 단계로 미루도록 권장.

---

## 11. 변경 이력

| 날짜 | 버전 | 변경 |
|------|------|------|
| 2026-05-04 | v1.0 | 초안 작성 |

---

## 끝.

이 파일은 OttO 프로젝트의 **헌법**입니다. 변경하려면 반드시 사용자 승인을 받으세요.
변경 시 §11에 변경 이력을 추가하세요.
