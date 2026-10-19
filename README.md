# SM엔터테인먼트 OpenDART 재무분석 대시보드

- GitHub 저장소: https://github.com/HSC-Class01/Sy_sm
- 대시보드 주소(첫 배포 후 확인): https://hsc-class01.github.io/Sy_sm/
- 대상 기업: SM엔터테인먼트, 종목코드 `041510`
- 수집 시작연도: 2010년
- 자동 실행: 매월 1일 09:17 KST (UTC 00:17), 또는 Actions에서 수동 실행

## 최초 설정

1. OpenDART에서 API 인증키를 발급합니다: https://opendart.fss.or.kr/
2. GitHub 저장소의 **Settings → Secrets and variables → Actions → New repository secret**에서 이름을 정확히 `DART_API_KEY`로 지정하고 발급받은 키를 저장합니다. 키를 코드나 README에 직접 입력하지 마세요.
3. **Settings → Pages → Build and deployment → Source**를 `GitHub Actions`로 설정합니다.
4. **Actions → SM DART update and Pages → Run workflow**를 실행합니다.
5. 실행 결과에서 수집·검증·배포 단계가 모두 성공했는지 확인합니다. 대시보드는 실제 API 자료를 받은 뒤 채워집니다.

워크플로 파일은 `.github/workflows/sm-dart-pages.yml`입니다. 이 워크플로는 DART 자료를 수집하고, 재무자료 JSON/CSV 및 원문 ZIP을 저장소에 보관한 뒤, `site/`에 정적 대시보드 배포 산출물을 만들어 GitHub Pages로 배포합니다.

## 데이터 및 분석 범위

- OpenDART 종목코드로 법인을 확인하고, 2010년 이후 사업·반기·분기 공시 목록과 원문 ZIP을 저장합니다.
- 단일회사 전체 재무제표 API 기반 수치 추출은 2015년 이후를 대상으로 합니다. 2010–2014년은 원문 보관 대상이며, 구조화 API 수치는 자동 추출되지 않습니다.
- 연결(CFS)을 우선 사용하고 연결 자료가 없으면 별도(OFS)를 사용합니다. 시계열 비교 시 연결/별도 범위 변경에 유의하세요.
- 대시보드 재무항목: 자산총계, 부채총계, 자본총계, 유동자산, 유동부채, 현금및현금성자산, 매출액, 영업이익, 당기순이익, 영업활동현금흐름.
- 재무비율: 부채비율, 자기자본비율, 유동비율, 영업이익률, 순이익률.
- 손익 및 현금흐름은 누적(YTD) 값, 재무상태표 항목은 기말 값입니다. 분기 표는 1분기 및 3분기 보고서이며, Q4 독립 값으로 해석하지 마세요.
- 계정이 모호하거나 값이 확인되지 않는 항목은 임의 추정하지 않고 빈 값으로 둡니다. 피어 기업은 참고 비교군이며 피어 재무수치는 이 저장소에서 수집하지 않습니다.

## 로컬 테스트

Python 3.11 이상에서 실행합니다.

```bash
python -m pip install -r requirements.txt
python -m unittest discover -s . -p 'test_financials.py' -v
```

실제 수집을 실행하려면 환경변수 `DART_API_KEY`를 설정한 뒤 `python main.py`를 실행하세요. API 인증, 실제 수집, Pages 배포는 GitHub Actions 로그에서 별도로 확인해야 합니다.
