# 내부자거래 공통 UI (Insider Ownership Common UI)

KR DART **D002**(임원·주요주주 특정증권등 소유상황보고서) + US SEC **Form 3/4/5**(3/A·4/A·5/A 포함)를 하나의 공통 스키마·공통 UI로 표현하는 다크 프리미엄 금융 데이터 화면.

> 범위: KR D002 + US Form 3/4/5. **KR 거래계획보고서(planned transaction)는 out-of-scope.**

## 구성

| 파일 | 설명 |
|---|---|
| `내부자거래 공통 UI.dc.html` | 메인 UI (4-View: List/Search · Ticker History · Owner History · Filing Detail) |
| `Handoff-필드매핑.dc.html` | 개발자 핸드오프 (공통 스키마 `insider-ownership-common-v1` · UI 규칙 · 추출 프롬프트 · QA) |
| `support.js` | 런타임 (Design Component) |

## 실행

정적 파일이므로 별도 빌드가 필요 없습니다.

```bash
# 로컬 정적 서버 예시
npx serve .
# 또는
python3 -m http.server
```

브라우저에서 `내부자거래 공통 UI.dc.html`을 엽니다. 두 화면은 상단 탭으로 연결됩니다.

## 4-View 흐름

```
List / Search ─(ticker)→ Ticker History ─(보고자)→ Owner History
      └─────────────(최신 공시 보기 / 거래목록 보기)─────────────→ Filing Detail
```

- **List / Search**: 종목(issuer.cik / KR corp_code) 그룹, 최신 공시일 DESC 정렬, 그룹 내 5개 row + 더보기 아코디언
- **Filing Detail**: Page Meta Card + 6섹션(보고자·내부자 정보 / 보유 변화 요약 / 거래·변동 내역 / 보유 내역 / 권리성·파생성 증권 / 주석·비고)

## 데이터

업로드된 실제 공시 원본에서 추출한 공통데이터로 채워져 있습니다 — KR D002 다수 종목 + US Form 3/4/5(MSFT·COST·META·TSLA·AMZN·NVDA·WMT·Symbotic·LLY·AAPL·Alphabet·Aktis 등). 새 데이터는 핸드오프 문서의 추출 프롬프트로 `insider-ownership-common-v1` 스키마에 맞춰 정규화합니다.

## 색상 관례 (한국 금융)

- 취득·증가·상승 = **Red**
- 처분·감소·하락 = **Blue**
- 중립·미기재 = Gray
