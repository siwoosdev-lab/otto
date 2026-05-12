# OttO

> 사장님은 두 번 일하지 않습니다. 반복은 OttO에게, 본업은 사장님께.

AI 기반 업무 자동화 구독 서비스 OttO의 랜딩 페이지 + 운영 인프라.

## 운영 현황

| 항목 | 값 |
|---|---|
| 라이브 사이트 | https://otto-jet.vercel.app |
| 미래 도메인 | https://nolly.co.kr (미연결) |
| GitHub 저장소 | https://github.com/siwoosdev-lab/otto |
| 폼 | Tally `VLoy9E` |
| 단계 | Phase 1 (검증 단계, 0~3개월 차) |

진행 중인 작업과 다음 단계는 [docs/HANDOFF.md](docs/HANDOFF.md) 참조.

---

## 빠른 시작 (5분 내 배포)

### 1. 로컬에서 미리보기

```bash
# 저장소 클론 후
cd otto

# Python 3가 있다면 (가장 쉬움)
python3 -m http.server 3000 --directory public

# 또는 Node.js가 있다면
npx serve public

# 브라우저에서 http://localhost:3000 접속
```

### 2. Vercel에 무료 배포

```bash
# Vercel CLI 설치 (한 번만)
npm i -g vercel

# 프로젝트 폴더에서
vercel

# 첫 질문에 답:
# - Set up and deploy? Y
# - Which scope? (본인 계정 선택)
# - Link to existing project? N
# - Project name? otto
# - In which directory is your code located? ./public
# - Want to override settings? N

# 끝. 자동으로 URL이 발급됩니다.
```

### 3. 도메인 연결

1. 가비아·후이즈 등에서 `nolly.co.kr` 구매 (연 1.5~2만 원)
2. Vercel Dashboard → Project → Settings → Domains
3. `nolly.co.kr` 추가 → 안내 따라 DNS 설정
4. 5~10분 후 HTTPS 자동 적용

---

## 프로젝트 구조

```
otto/
├── CLAUDE.md                              ← Claude Code 작업 규칙 (필독)
├── README.md                              ← 이 파일
├── docs/
│   ├── OttO_사업계획서.md
│   ├── OttO_사이트_개발계획서.md
│   └── OttO_랜딩페이지_설계도.md
└── public/
    ├── index.html                        ← 메인 랜딩 페이지
    ├── privacy.html                      ← 개인정보처리방침 (작성 예정)
    └── terms.html                        ← 이용약관 (작성 예정)
```

## 기술 스택 (Phase 1)

| 영역 | 기술 | 비용 |
|------|------|------|
| 호스팅 | Vercel Hobby | 0원 |
| 도메인 | nolly.co.kr (가비아) | 연 ~2만 원 |
| 폼 | Tally.so | 0원 |
| DB | Supabase Free | 0원 |
| 분석 | Cloudflare Web Analytics | 0원 |
| 메일 | Google Workspace | 월 7,500원 |
| **총계** | | **약 월 9~14,000원** |

상세는 `CLAUDE.md` §4 참조.

## 개발 시 주의사항

- 새 코드 작성 전 `CLAUDE.md`를 반드시 읽으세요
- 디자인 토큰(색상·폰트)은 임의 변경 금지
- 새 npm 패키지·프레임워크 도입 금지 (현재 정적 HTML)
- 비용 발생 작업은 사전 승인 필수

## 라이선스

Proprietary. © 2026 널리(Nolly). All rights reserved.

## 연락처

- 웹: https://nolly.co.kr
- 이메일: hello@nolly.co.kr (예정)
- 카카오 채널: @otto (예정)
