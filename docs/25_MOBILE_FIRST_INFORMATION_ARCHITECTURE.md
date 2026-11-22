# Mobile-first Information Architecture & Independent App Boundary (v1.0)

> Status: product architecture specification. The attached visual mockups are **desktop concept references**, not literal requirements to cram every panel into a mobile screen.

## 0. Product identity
**PublicAuction is its own independent application, brand, GitHub repository, frontend, navigation, release pipeline, domain deployment, data contract and user journey.** Do not merge routes, databases, authentication sessions, product analytics or UI branding with the other two products simply because they have a common author. A reusable package may be copied or versioned only if a real shared requirement is demonstrated and its compatibility is tested.

공매 탐색은 쇼핑형 정보 아키텍처. 3D 장면은 필수가 아니며 물건 검색·상태·가격·공식출처가 우선.

## 1. Decision architecture
Choose progressively: one primary task per mobile screen; reveal details only when requested. Do not render the desktop dashboard at 40% width. "Progressive disclosure" means routes/states, not hidden content that is inaccessible on mobile. A map is not a substitute for an accessible information list.

## 2. Page-by-page journey
| Stage | Page | Mobile primary content | Proposed route |
|---|---|---|---|
| 01 | 발견 | 검색창, 차량·기계·부동산 카테고리, 최근 관심도가 높은 **공식 집계 가능 유형**. 복잡한 통계 차트는 넣지 않는다. | `/` |
| 02 | 목록 | 유형·지역·금액·입찰기간 필터, 물건 카드, 출처 및 갱신일. 모바일 필터는 별도 sheet. | `/auctions` |
| 03 | 물건 상세 | 물건 이미지(이용 허가 시), 감정가/최저입찰가/기간/공고 상태, 공식 공고 연결. 필수 위험·추가 비용 표시. | `/auctions/[id]` |
| 04 | 비교 | 선택한 물건의 조건을 동일 단위로 비교. 가격 종류 및 결측을 구분. | `/compare` |
| 05 | 분석 | 실제 입찰 참가·유찰·낙찰 등 공식 집계의 범위/표본/기간을 드릴다운. 조회수는 별도. | `/insights` |
| 06 | 관심 항목 | 즐겨찾기 및 사용자가 원한 공고 업데이트. 알림은 명시적 opt-in. | `/saved` |

홈은 검색+카테고리+대표 물건 3요소로 구성. 실시간 참가자 수와 현재 수익 기대치를 추측해 보여주지 않는다. 기존 한 화면에 몰린 시장 분석 리포트는 /insights에서 제공. 비교는 /compare로 분리.

## 3. Three distinct responsive compositions
| Viewport guidance | Composition | Navigation | Data visibility |
|---|---|---|---|
| Mobile <= 599 CSS px | Single-column, 1 primary CTA, stacked wizard, separate detail route | Bottom nav; native back / persistent route params | 1 main metric group, progressive expansion; full-screen or sheet filters |
| Compact tablet 600–1023 px | Two panes when usable; otherwise mobile stack | Sidebar or tabs | Selected item + one detail panel |
| Desktop >= 1024 px | 2–3 column explorer/detail/analysis workspace | Sidebar or top nav | Rich summary panels, but still grouped by task |
These widths are initial design tokens, not a hard breakpoint to force layout on all devices. Also respond to container width, zoom and touch/hover capabilities. Portrait tablet may use mobile behavior.

## 4. Route-state contract
- Refresh and deep-link must preserve region/category/search/filter/selected item via safe URL query params or persistent session state.
- Browser back returns to the exact previous step with prior selections retained, not back to home.
- No invalid ID silently redirects to an unrelated listing. Offer explicit recovery.
- Loading/empty/error/stale/permission-denied states have unique screens and tests.
- Shareable pages contain server-rendered explanatory content for SEO, not solely WebGL canvas.
- Auth gates only for private personalized features; public analysis stays browsable without signup.

## 5. Design reference migration
- Images in `assets/` or `docs/` should be cataloged as `desktop-concept`, `mobile-flow`, `tablet-split-view`, `3d-scene-reference`.
- Existing beautiful desktop mockups define palette, atmosphere, card treatment, typography, material, and visual hierarchy. Their multiple panels must be mapped into **separate mobile routes**.
- UI copyright/assets: only use images actually present in repo or licensed content, never treat generated demo numbers as real data.
- Mobile illustration must be planned for each key stage independently. Day/night modes change atmospheric lighting, not data truth or navigation structure.

## 6. Agent implementation sequence and TDD
1. Write route-state tests for stage progression, back navigation, and preservation of query params.
2. Implement mobile component skeletons and loading/empty/error states; Playwright viewport screenshots.
3. Add tablet split layout and desktop workspace **without duplicate business logic**.
4. Link domain layer through typed view models. Business rules are independent of CSS or Three.js.
5. Add 3D as an enhancement where applicable with tested 2D fallback and WebGL disabled scenario.
6. Add accessibility tests for keyboard, screen readers, focus order, reduced motion, dynamic zoom up to 200%.
7. Visual regression at widths 375, 430, 768, 1024, 1440 CSS px; use curated stable fixtures.
8. Regression: switching viewport must not lose user-selected state; DOM and selected 3D feature ID match.

### Acceptance scenarios
- Starting on a 375px phone, the user can perform the full journey without horizontal scrolling and without needing dashboard knowledge.
- The user can exit/re-enter each step and retrieve prior selection through route state.
- Critical factual data and source links are readable with no WebGL.
- Desktop supports simultaneous panels but each panel is purposeful, not obligatory.
- No product routes or names from the other two apps appear in this app's main navigation.
- Analytics measure task completion and useful return visits, not time wasted in multi-panel complexity.

## 7. Navigation rules
Mobile bottom nav: 탐색 / 관심 / 분석 / 가이드. 상세 화면에는 하단 '공식 공고 확인' 단일 primary CTA. 입찰대리 또는 결제 CTA는 초기 출시에서 제공하지 않는다.

## 8. Business safety / correctness
경매·공매 구별; 초기 온비드 공매. 유료 광고·대리·수수료 모델은 별도 법적 검토 후 범위 확정. 인기 품목은 공식 과거 입찰 통계와 사이트 내부 조회를 구분하고, 확률적 큰 차익을 보장하는 듯한 문구/타이머를 배제.

## 9. Engineering ownership
No technical approval required from the founder. Agent supplies tests, screenshots, performance reports, architecture rationale and teachable file-reading order. Founder reviews the **experience** and business direction; CI and source data checks verify implementation correctness. Do not claim a user preview or WebGL test passed before running it.

## 10. Work packages
- UX-IA-001: independent routing / page inventory / route graph / breadcrumb specification
- UX-IA-002: mobile shell and navigation + Playwright back/forward tests
- UX-IA-003: each screen one goal: search/select/detail/report component suite
- UX-IA-004: tablet split-view
- UX-IA-005: desktop concept decomposition
- UX-IA-006: 3D adaptive rendering + fallback where applicable
- UX-IA-007: responsive + accessibility regression + screenshots
Each PR contains a task-focused demo, test-first evidence, and learning guide.
