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

## Agent-driven TDD & 학습 문서 (2026-11-22 추가)
- [엔지니어링 원칙: 선언적 코드·가독성·예측 가능성·응집도·결합도](docs/19_ENGINEERING_PRINCIPLES.md)
- [코드 읽기와 실습 순서](docs/20_LEARNING_PATH.md)
- [테스트 증거 기반 품질 게이트](docs/21_QUALITY_GATES.md)
- [PR 단위 구현 백로그](docs/22_IMPLEMENTATION_SPRINTS.md)
- [홈서버 / Vercel / 도메인 운영](docs/23_OPERATIONS_AND_COST.md)
- [Agent 개발 규약](agent/AGENT_RULES.md)
- [작업 템플릿](agent/TASK_TEMPLATE.md)

## 디자인 콘셉트 (오리지널 SVG)
![PublicAuction 디자인 콘셉트](assets/auction-concept.svg)

예전 채팅에 표시된 외부 사진과 동일한 이미지가 아닌 **이 저장소를 위해 새로 작성한 콘셉트 SVG**이다. 실제 DB나 작동하는 화면의 스크린샷이 아니다.

## 개발 진행 상태
- 완료: 요구사항/기술 규약/학습 계획 문서 및 오리지널 SVG
- 미완료: 앱 코드, 실제 데이터 계약 검증, TDD 실행 로그, Three.js 실제 렌더러, 자동화 CI, Vercel 배포
- 개발 프로세스: Agent가 테스트 작성→실패 로그→구현→테스트 통과 증명→코드 읽기 문서 제공. 창업자는 기술 승인 부담 없이 학습 및 제품 의견 제공.
