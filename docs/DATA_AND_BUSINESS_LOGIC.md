# Data & Business Logic — PublicAuction

## 1. 확인된 원천
- 온비드 공고상세: https://www.data.go.kr/data/15157218/openapi.do
- 공고별 물건: https://www.data.go.kr/data/15157220/openapi.do
- 동산 상세: https://www.data.go.kr/data/15157245/openapi.do
- 입찰결과 목록: https://www.data.go.kr/data/15157252/openapi.do
- 입찰결과 상세: https://www.data.go.kr/data/15157254/openapi.do
- 용도별 입찰 통계: https://www.data.go.kr/data/15157757/openapi.do
- 지역 월별 공매 통계: https://www.data.go.kr/data/15140290/fileData.do
- 월별 입찰참가자 현황: https://www.data.go.kr/data/15120358/fileData.do

**구현 전** API 신청, 응답 실제 샘플, 필드 의미·이용허락·쿼터·갱신주기를 수집하여 `DATA_CONTRACTS`에 승인한다. API 명칭이 존재한다는 사실은 모든 물건에 데이터가 있다는 뜻이 아니다.

## 2. 논리 모델
`source_registry(source_id,uri,license,checked_at)`
`ingestion_run(run_id,source_id,started_at,completed_at,status,checksum,error)`
`notice(source_notice_id,source_id,version,status,published_at,changed_at,raw_ref)`
`asset(source_asset_id,source_id,notice_id,asset_type,title,region,location_precision,condition,updated_at)`
`auction_round(source_notice_id,source_asset_id,round_key,start_at,end_at,reserve_amount,currency,status)`
`result(source_asset_id,round_key,result_status,winning_amount,participant_count,disclosure_flag)`
`asset_snapshot(asset_id,observed_at,payload_hash,source_version)`
`category_stat(category,region,period_start,period_end,bid_count,participant_count,win_count,methodology)`
`audit_quality(entity,key,issue,severity,detected_at)`

Use unique indexes on source IDs + round IDs and immutable snapshots. Source-derived data separate from user engagement counts. Never infer identity from similar names alone.

## 3. 인기 카테고리 정의
목적은 **공식 과거 데이터로 관심 유형을 비교**하는 것이다.
- 우선순위 1: 같은 기간/분류 범위의 공개된 실입찰 집계(입찰수, 참가자수, 경쟁률). 제공되지 않는 세부분류의 경우 표시하지 않음.
- 우선순위 2: 서비스 자체 집계된 비식별 상세 조회수(반봇/중복 제거); 반드시 '사이트 내 조회'라고 별도 표기.
- 두 측정값을 합쳐 '입찰 많이 하는 품목'으로 표기 금지.
- `stat_rate = bidder_count / auctioned_count`는 분모가 적합할 때만 계산. 표본 충분성 threshold 설정 후 불충족시 '표본 부족'.
- 기간=최근 완료 월(예시)이며 UI에 실제 시작/종료일/집계 단위를 표시. '실시간 입찰 인기' 문구 금지.
- 공식 원천이 부동산 중심인 통계이면 차량/동산에 범용화 금지.

## 4. 데이터 파이프라인
Extract: fetch with backoff, bounded concurrency, ETag if available, rate-limit aware.
Validate: schema, type, dates, currency units, status enums, required keys.
Normalize: Unicode/region code/type taxonomy; timezone KST to UTC in storage.
Upsert: transactional upsert by official composite key.
Snapshot: changed records append only, report change events.
Publish: cached read-model by category/region/month.
Delete: source retractions marked as retracted, preserve internal trace only.
Fallback: source down -> stale badge & last_success_at, never silently fabricate.

## 5. 가격/위험 모델
감정가≠시세; 최저입찰가≠총비용; 낙찰가≠재판매 가격. 부대비용/상태/권리/계약규정은 미검증일 수 있음. 결측은 null로 저장하고 0으로 대체하지 않는다.

## 6. 품질 관리
중복감소율, orphan notice/asset count, result match success rate, schema drift, missing money/unit rate, source link integrity, snapshot consistency. 합계 일치 검증은 집계 범위가 동일할 때만 수행.
