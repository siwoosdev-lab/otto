# SEO · 검색엔진 등록 가이드

> `https://nolly.co.kr` 배포 직후 진행. 한국 시장이라 **네이버 등록이 구글만큼 중요**합니다.
> 이 문서를 위에서 아래로 그대로 따라가면 됩니다. 모든 사전 준비는 코드에 적용되어 있습니다.

---

## 0. 사전 체크 — 도메인 정상 동작 확인

등록 시작 전 5가지 모두 ✓ 인지 확인:

```bash
# 터미널에서 한 번에 확인
curl -I https://nolly.co.kr/                  # HTTP/2 200
curl -I https://nolly.co.kr/sitemap.xml       # HTTP/2 200
curl -I https://nolly.co.kr/robots.txt        # HTTP/2 200
curl -I https://nolly.co.kr/privacy           # HTTP/2 200
curl -I https://nolly.co.kr/terms             # HTTP/2 200
```

전부 `200` 떠야 진행. 하나라도 404·5xx면 Vercel DNS 반영부터 확인.

---

## 1. Google Search Console 등록 (10분)

### 1.1 속성 추가

1. https://search.google.com/search-console 접속 (Google 계정 로그인)
2. 좌상단 속성 선택 → **속성 추가**
3. 두 가지 옵션 중 선택:

| 옵션 | 추천 여부 | 차이 |
|------|---------|------|
| **도메인** (`nolly.co.kr`) | ✅ 추천 | 서브도메인·http/https 모두 포함. DNS TXT 인증만 가능 |
| **URL 접두어** (`https://nolly.co.kr/`) | 차선 | HTML 메타·파일 등 다양한 인증 가능. apex 만 커버 |

**도메인 옵션 권장.** 한 번 등록으로 향후 `app.nolly.co.kr`, `blog.nolly.co.kr` 같은 서브도메인도 자동 포함.

### 1.2 소유 확인 — DNS TXT 방식 (도메인 옵션 선택 시)

Google이 보여주는 TXT 레코드 값을 복사 (예: `google-site-verification=abc123xyz...`).

가비아 DNS 설정 진입:
1. https://my.gabia.com → **My가비아** → **서비스 관리** → 도메인 통합관리
2. `nolly.co.kr` → **관리** → **DNS 설정 → DNS 관리** → **DNS 레코드 수정**
3. **레코드 추가**:

| 타입 | 호스트 | 값 | TTL |
|------|--------|------|-----|
| TXT | @ | `google-site-verification=발급받은_값` | 600 |

4. 저장 → 가비아에서 반영 5~30분
5. Search Console 페이지로 돌아가서 **확인** 버튼 클릭

> TXT 인증은 Vercel 의 다른 DNS 설정(A, CNAME)과 공존 가능. 기존 레코드 건드리지 마세요.

### 1.3 (URL 접두어 옵션 선택 시) — HTML 메타 방식

이 방식 선택한 경우만 해당. Google이 발급한 토큰을 받아서:

`public/index.html` [11번째 줄 근처](../public/index.html#L15) 의 주석을 풀어 토큰 붙여넣기:

```html
<!-- BEFORE -->
<!-- <meta name="google-site-verification" content="여기에_구글_토큰_붙여넣기"> -->

<!-- AFTER -->
<meta name="google-site-verification" content="실제_토큰_값">
```

그 후 git commit & push → Vercel 자동 배포 → Search Console 의 **확인** 클릭.

### 1.4 Sitemap 제출

소유 확인 성공 후:

1. 좌측 메뉴 **Sitemaps**
2. "새 사이트맵 추가" 입력란에 `sitemap.xml` 입력 (앞에 https://nolly.co.kr/ 자동)
3. **제출** 클릭
4. 상태가 **성공** 으로 뜨면 끝. (24~48시간 후 검색결과 색인 시작)

### 1.5 URL 검사 (즉시 색인 요청)

1. 좌상단 검색바에 `https://nolly.co.kr/` 입력 → 엔터
2. 결과 화면에서 **색인 생성 요청** 클릭
3. 큐 대기 후 보통 수 시간 내 색인됨

같은 작업 `/privacy`, `/terms` 에도 1회씩 반복하면 빠름.

---

## 2. 네이버 서치어드바이저 등록 (10분, 한국 검색의 핵심)

### 2.1 사이트 추가

1. https://searchadvisor.naver.com 접속 (네이버 계정 로그인)
2. **웹마스터도구** 진입
3. 우상단 사이트 입력란에 `https://nolly.co.kr` 입력 → **추가**

### 2.2 소유 확인 — HTML 태그 방식 (가장 쉬움)

네이버가 보여주는 메타태그 값을 복사 (예: `content="abc123..."` 의 abc123 부분).

`public/index.html` 의 **이미 준비된 주석 라인** 활성화:

```html
<!-- BEFORE (현재 코드에 이미 존재) -->
<!-- <meta name="naver-site-verification" content="여기에_네이버_토큰_붙여넣기"> -->

<!-- AFTER -->
<meta name="naver-site-verification" content="네이버가_준_토큰">
```

그 후:
```bash
git add public/index.html
git commit -m "네이버 서치어드바이저 소유 확인 메타 추가"
git push
```

Vercel 자동 배포 후 (~15초), 네이버 페이지에서 **소유확인** 클릭 → "확인 완료" 떠야 성공.

### 2.3 사이트맵 제출

1. 등록된 사이트 클릭 → 좌측 **요청 → 사이트맵 제출**
2. 입력란에 `sitemap.xml` 입력 (앞에 도메인 자동)
3. **확인** 클릭

### 2.4 RSS·robots.txt 자동 확인

좌측 **검증 → robots.txt** 메뉴에서 `https://nolly.co.kr/robots.txt` 가 정상 인식되는지 확인. 이미 설정돼 있어 추가 작업 없음.

### 2.5 웹페이지 수집 요청 (즉시 색인)

좌측 **요청 → 웹페이지 수집** 메뉴에서 다음 URL 3개를 한 번씩 제출:
- `https://nolly.co.kr/`
- `https://nolly.co.kr/privacy`
- `https://nolly.co.kr/terms`

보통 1~3일 내 네이버 검색 색인 반영.

---

## 3. 빙(Bing) 등록 — 선택사항 (5분)

한국에서 점유율 낮지만 ChatGPT·Copilot 검색이 빙 인덱스 사용. 들이는 시간 대비 이득 큼.

1. https://www.bing.com/webmasters 접속
2. **Google Search Console에서 가져오기** 버튼 → GSC 계정 연동 → 자동으로 사이트맵까지 끌어옴 (30초 컷)

별도 메타태그·DNS 설정 불필요.

---

## 4. 등록 후 사이트에 이미 적용된 SEO 자산

코드는 이미 다음을 갖추고 있습니다. 추가 작업 없음.

### 4.1 메타 태그 (index.html `<head>`)

- `<title>` — 60자, 핵심 키워드 포함
- `<meta description>` — 130자, 업종+가치제안+CTA
- `<meta keywords>` — 네이버 보조 신호용 (Google 무시지만 Naver/Bing 일부 활용)
- `<meta robots>` — 색인+추적 허용 + 스니펫·이미지 미리보기 제한 없음
- `<meta author>` — 널리(Nolly)
- `<link canonical>` — `https://nolly.co.kr/` 정규 URL

### 4.2 OpenGraph + Twitter Card

- og:image (1200×630 PNG, 절대경로) — 카카오톡·페이스북·X 미리보기
- 도메인 변경에 따라 OG PNG 도 재생성 완료

### 4.3 JSON-LD 구조화 데이터

`<script type="application/ld+json">` 안에 3가지 스키마:

| 스키마 | 용도 |
|--------|------|
| **Organization** | 검색결과 우측 지식패널·로고 노출 (널리/Nolly, 사업자 정보) |
| **WebSite** | 검색결과의 사이트 링크·검색 박스 노출 (OttO 서비스명) |
| **Service** | 서비스 카드형 결과 (30분 무료 진단 offer 포함) |

Google Rich Results Test 로 검증 가능: https://search.google.com/test/rich-results?url=https%3A%2F%2Fnolly.co.kr%2F

### 4.4 sitemap.xml + robots.txt

- 3개 URL 등록 (`/`, `/privacy`, `/terms`)
- robots.txt 는 모든 크롤러 허용 + sitemap 위치 명시

---

## 5. 등록 후 1주일 모니터링

| 항목 | 도구 | 확인 주기 |
|------|------|---------|
| Google 색인 여부 | Search Console → 색인 → 페이지 | 매일 |
| Google 노출 키워드 | Search Console → 실적 → 검색결과 | 매주 |
| 네이버 색인 여부 | 네이버에서 `site:nolly.co.kr` 검색 | 매일 |
| 네이버 노출 키워드 | 서치어드바이저 → 통계 → 사이트별 통계 | 매주 |
| 핵심 키워드 순위 | 네이버 검색 직접: "AI 업무 자동화", "소상공인 자동화", "학원 카톡 챗봇" | 매주 |

### 색인이 안 되면?

- **3일 지나도 색인 안 됨** → robots.txt / canonical / meta robots 점검
- **Google 검색에서 사이트 안 보임** → Search Console URL 검사로 거부 사유 확인
- **네이버 검색에서 사이트 안 보임** → 네이버는 원래 신규 사이트 색인 보수적. 7~14일 기다린 후 "웹페이지 수집" 재요청

---

## 6. (Phase 2) 콘텐츠 SEO 로드맵

지금은 단일 랜딩 페이지라 SEO 한계 명확. 검색 유입을 본격적으로 늘리려면 콘텐츠 발행 필요. Phase 2 진입 시 검토:

### 6.1 블로그·사례 페이지 추가

`/blog/학원-카톡-챗봇-구축-가이드` 같이 **롱테일 키워드 한 개에 한 페이지** 발행. 첫 시범 고객 사례 3편이 시작점.

### 6.2 업종별 랜딩 페이지 분기

`/industries/학원`, `/industries/병원` 식으로 분기. 각 페이지에 해당 업종 모듈만 노출 → 업종명 검색에 강력.

### 6.3 FAQ 스키마 (FAQPage)

자주 묻는 질문 섹션에 `FAQPage` JSON-LD 추가하면 검색결과에 펼침형 FAQ 노출. 사이트에 FAQ 섹션 추가되면 적용.

### 6.4 Naver 검색 등록 신청

네이버는 일반 색인 외에 **네이버 비즈니스 등록** (place.map.naver.com) 도 별도. 첫 유료 고객 확보 후 진행 권장.

---

## 변경 이력

| 날짜 | 변경 |
|------|------|
| 2026-05-13 | 초안 작성 — 도메인 nolly.co.kr 연결 직후 |
