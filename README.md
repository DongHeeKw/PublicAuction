# PublicAuction

한국 공공자산 **공매 정보 탐색 및 검증 가능한 통계 서비스**. 법원경매와 온비드 공매를 혼동하지 않는다. 초기 범위는 **온비드 공매**, 직접 입찰·매수신청대리·결제는 범위에서 제외한다.

## North Star
공식 공고로 이동하는 *유효한 물건 상세 열람*과 주간 재방문. 광고 클릭수 또는 입찰 금액을 최적화하지 않는다.

## 문서
- [제품 및 사업 요구사항](docs/PRODUCT_REQUIREMENTS.md)
- [데이터·비즈니스 로직](docs/DATA_AND_BUSINESS_LOGIC.md)
- [UX 및 검색·탐색](docs/UX_AND_GROWTH.md)
- [아키텍처와 보안](docs/ARCHITECTURE.md)
- [검증 및 실행 로드맵](docs/TEST_AND_ROADMAP.md)

## 원칙
사실/추정 분리, 데이터 출처/갱신일 공개, 취소 공고 숨김 처리, 가격·경쟁률의 분모 공개, 과거 결과로 미래 수익 보장 금지. 인기 순위는 관측가능한 결과 기반이며 현재 실시간 입찰량인 것처럼 표시하지 않는다.

## 스택
Next.js + TypeScript, Python ETL, PostgreSQL, Vercel(비상업적 파일럿), Ubuntu(배치/DB). API 비밀키는 서버에만 저장.

## 현재 상태
명세 단계. API 실응답, 쿼터, 상업 이용허락 검증 전. 이 저장소 문서는 구현 승인 조건이지 현재 완성된 제품을 의미하지 않는다.
