# 스킬 DB

- DB: `skills.json` — 387개, 28개 카테고리 (v1.17, 2026-11-16 검증 기준; 신규 26개 — Google 공식 modern-web-guidance·chrome-extensions, browser-use, addyosmani/agent-skills, AWS Agent Toolkit, shadcn 공식 스킬 등. health 카테고리 신설(fuck-cancer 1개))
- 회사 공용: `skills-shared.json` — 384개 (rom-qa 2개 + claude-mem 1개 제외)
- 참조 자료: `references.json` — 스킬이 아니지만 스킬을 고르거나 만들 때 참조할 만한 자료 목록 (v1.1, 2026-11-16). skills.json 개수에는 포함하지 않음. 5개 항목: public-apis(무료 API 목록, MIT, ★498,263), public-api-lists, graphql-apis, prompts-chat, awesome-mcp-servers-mcphq
- 신뢰도 표기: 각 스킬에 `stars`(source_repo 레포 전체 GitHub 스타 수, 2026-11-16 확인 — 개별 스킬 수치가 아님)와 `citations` + `citation_sources` 필드가 있음. citations는 우리 DB가 수집한 출처에서 해당 스킬을 언급·추천한 외부 출처 수(출처당 1회, 자신의 홈 레포 제외, 전 세계 인용 횟수 아님). 분포: 0인 스킬 268개(69.3%, 개인 387개 기준), 최다 skill-creator 10곳. 과소집계 가능 4개(figma-implement-design·stop-slop·caveman·last30days)는 요약 목록에만 나와 출처 귀속이 안 돼 0으로 기록됨.
- 검색: `python3 search.py "<작업 요청>"` / `python3 search.py --category ui-design` / `python3 search.py --list`
- 선택기: `~/workspace/custom-skills/skill-selector/SKILL.md` — 작업 전에 이 절차로 2-3개 골라 SKILL.md 본문을 읽고 적용
- 출처 원본: `research/` — 날짜별 md

## 필드
id, name, category, purpose_ko, when_to_use, example, keywords, source_repo, skill_md_url, install, license, compatible, requires, priority(core/recommended/optional)
+ evaluation (핵심 25개에만 있음): stars(레포 전체 수치), last_activity, recommendation_count, verdict(추천/보통/주의), trust(높음/보통/낮음), pros, cons
+ comparisons (7개 쌍): 비슷한 스킬끼리 "언제 A를, 언제 B를 고를지"

## 평가에 대해
- 별점(5점 만점)은 어떤 출처에서도 확인되지 않아 지어내지 않음. 대신 스타 수·최근 활동·중복 추천·장단점으로 신뢰도를 매김.
- 신뢰도 집계(25개): 높음 18개, 보통 6개, 낮음 1개(writing-safe-migrations — ★2·커밋 2개·외부 추천 0건)
- 인기도가 곧 효과 증거는 아님. Placebo 대조 실험에서 Superpowers −2%, Karpathy +9%, planning-with-files +5%로 셋 다 placebo 대비 유의미한 우위를 보이지 못함.

## 카테고리별 개수 (v1.17)
ai-ml 9, automation-meta 24, business 19, coding-plan 10, data-analysis 9, database 7, debugging 3, dev-method 41, docs-office 9, education 12, finance 8, game-dev 18, health 1, hr 6, infra-devops 24, legal 5, marketing 13, mobile 8, notes-productivity 9, research 12, rom-qa 2, science 5, security 14, testing-ci 10, translation 30, ui-design 34, video-audio 39, writing 6

## 새로 추가된 것 (v1.17, +26개 — 주간 갱신 2026-11-16)
- 주요 소스 2개: game-dev-character-model(MengTo, 11-12 신규 — 실사 3D 게임 캐릭터, Fil 크레딧 85–100 유료)·claude-for-manufacturing(Anthropic 공식, 제조업 스킬 17개 묶음 — 레퍼런스 구현이라 적극 유지보수 안 함). 이월 해소: aws-core(AWS 공식 Agent Toolkit, Apache-2.0, ★3,497, ~64개 스킬)·runcomfy-agent-skills(gencraft-labs, MIT, ★3, 33개 — RunComfy CLI·유료 크레딧 전제)·first-commit(MIT, ★2, 70개 킷 스캐폴더)·shadcn(공식, MIT, ★128,255)·invoice-chase(Anthropic KWP small-business, 연체 송장 독촉, Apache-2.0, 경영/회계 우선 반영).
- 발견 출처 19개: modern-web-guidance(GoogleChrome 공식, Apache-2.0, ★17,760, 주간 +15,032, core)·chrome-extensions(동일 레포, Manifest V3+스토어 게시)·socratic-tutor(Jane Street, MIT, ★9,007, HN 714 pts)·browser-use(★121,116, core, CDP 브라우저 자동화)·addyosmani-agent-skills(★117,944, core, 25개 컬렉션)·ste80-humanizer·cinematic-skill·brain-md·chisle·design-to-ui(NAVER, Apache-2.0)·everyinfra-seo-skills·illyism-seo-skills(라이선스 README 표기만)·raea(FSL-1.1-ALv2, 비경쟁 조항)·guizang-game-skill·article2media·chaojifeng-video-content-creator(중국어 전용 3개)·everything-claude-trading(82개 표기이나 factor-model-design 등 일부 트리에 없음)·fuck-cancer(health 카테고리 신설).
- 변경: mcpcheck 경유 19개 스타 수 GitHub API 재확인 완료(xcstrings-localize 2·asc-localize-metadata 1138 채움, srt-translate 0이 실제 값). azure 3종 GitHub Copilot 호환 확정(미확정 문구 제거). anthropics/skills ★195,362, claude-plugins-official ★47,398, MengTo ★7,602, dengyie ★78. openai/skills 4개 항목에 deprecated 표시(후계 openai/plugins). 오케스트레이션 6개 SkillSpector 클론 방식 재스캔: 점수 변화는 orchestrated-execution 52→56(MEDIUM 1건 과탐)뿐, 실제 리스크 0건 — 9d 미결 해결.
- 참조 자료 +4개(v1.1): public-api-lists·graphql-apis·prompts-chat·awesome-mcp-servers-mcphq. 웹용 스킬 73개 → 74개(socratic-tutor 추가 — SKILL.md만 붙여넣으면 챗봇에서도 동작).
- 제외: duoasa-skill(풍자성 메타 스킬, 실용 가치 얇음)·harneo(데스크톱 앱 본체)·grok-search(라이선스 미확인)·obsidian-doc-router(개인 금고 하드코딩)·public-apis-marcelscruz(public-apis와 중복)·sakurai-skill·remotion-dev/skills(라이선스 없음)·GitHub-Copilot-app 스팸 클러스터 22개 이상·cursor-pro-desktop(크랙 의심). 미확인: GeekNews 11-09~15 구간(RSS·Weekly 커버 불가), upstar 주간 1위 레포 이름 추출 누락, skills.sh Trending(24h) 탭 확인 불가. swarms 라이선스 여전히 없음(제외 유지).

## 새로 추가된 것 (v1.13·v1.14, +6개 — 민둥이 영상 11-10 트렌딩 TOP 11, 2026-11-11)
- v1.13 (+5개): astryx(★22,683, Meta 디자인 시스템, CLI 필요)·agent-browser(★49,168, 브라우저 자동화, CLI 필요)·daily-stock-analysis(★69,546, 종목·시장 분석, 중국어 SKILL.md·Gemini API 키 필요·투자 조언 아님)·gbrain(★32,490, 에이전트 메모리 시스템, 컬렉션 1개로 추가 — SKILL.md 실측 112개)·knowledge-wiki(★20,471, Managed Agents 지식 위키, 베타 + API 비용 발생). v1.14 (+1개): first-run(computer-use 온보딩 가이드). 개인 355개 → 361개, 회사 공용 352개 → 358개. 큐레이터 358개로 갱신·재발행 완료(로그인 없이 열림 확인).
- 스캔: SkillSpector 신규 6개(컬렉션 포함 117개 스캔 단위), 발견 329건. CRITICAL 3건·HIGH 43건 전수 검토 결과 실제 리스크 0건. knowledge-wiki 100점은 SEC EDGAR 조회용 User-Agent 과탐, first-run의 .env 관련 건은 금지 문구 자체 탐지. 알아둘 동작: gbrain 설치 시 주의 7건(히스토리 삭제·훅 설치·토큰 저장 등)을 항목에 기록, first-run은 .env 존재 여부만 확인.
- 제외: sherlock(OSINT CLI)·MuJoCo(물리 엔진)·RuView(Wi-Fi 센싱)·ha-mcp(MCP 서버)·OpenMausBot(데스크톱 앱) — 스킬 아님. 중복: anthropics/skills(docx·pdf·pptx·xlsx 4개 이미 DB에 있음).
- 신규 6개는 웹용 분류에 안 들어감(서브에이전트·스크립트·CLI·API 키 필요). 웹용은 73개 그대로.

## 새로 추가된 것 (v1.12, +6개 — 오케스트레이션, 2026-11-10)
- agent-orchestration·multi-agent-orchestration·agent-orchestrator·CCteam-creator·orchestrated-execution·conductor-orchestrator. dev-method 29개 → 35개. 큐레이터 352개로 갱신·재발행 완료(로그인 없이 열림 확인).
- 스캔: SkillSpector 보충 스캔 6개 전수, CRITICAL 0건, 평균 45.7점. 100점 2개(agent-orchestration·CCteam-creator)는 HIGH 전수 검토 결과 과탐. 알아둘 동작 2건(악성 아님): agent-orchestration의 post-commit 훅 설치(매 커밋 briefing.md 재생성, `--uninstall`로 제거), CCteam-creator의 버전 체크(WebFetch + 로컬 플러그인 캐시 읽기).
- 주의: agent-orchestrator는 SKILL.md 중국어, conductor-orchestrator는 AGPL-3.0 + 단독 복사 시 동작 안 할 수 있음, orchestrated-execution은 metaswarm 프레임워크의 일부. 제외: swarms(★247, 라이선스 없음 — 생기면 즉시 후보).

## 새로 추가된 것 (v1.10·v1.11, +35개 — 번역 스킬, 2026-11-09)
- v1.10 (+8개, 사용자 지목 12개 중 검증 통과분): translate-book·oma-translation·baoyu-translate·book-translator·fidelity-translator·create-kr-patch·translate-retro-game·pdf-layout-translator. translation 1개 → 6개, game-dev 11개 → 13개, docs-office 8개 → 9개. 주의: create-kr-patch는 SFC 한글패치 방법론만 있고 실행 도구 미번들, translate-retro-game은 SNES/GBA/NDS/PS1/PSP 스크립트 30여 개 번들(일→영 전제).
- v1.11 (+27개, 용도별 탐색): translation 6개 → 30개, game-dev 13개 → 16개. 일상·일반 5개, 도서·전자책 4개, 게임 3개, 기술 문서 4개, 자막·영상 7개, 웹사이트·앱 6개. 카테고리는 translation 하나로 두고 게임 현지화만 game-dev로 분리, `purpose_ko` 첫머리에 용도 태그.
- 주의: ainiee-translate·translate-docs는 AGPL-3.0(회사 공용 주의 표기), azure 3종은 구독 필요, video-translator·heygen-translate는 유료 API. mcpcheck 경유 19개 중 11개의 스타 수는 mcpcheck 표기값이라 11-16 주간 갱신 때 GitHub API 재확인 예정, xcstrings-localize·asc-localize-metadata 스타 수 미확인.
- 제외: retro-game-korean-localization-qa(레트로 게임 한글 현지화 QA, SFC 패치 작업과 직접 관련이나 라이선스 없음). 보류: skill.game-localization-ko(게임 한글화 전용, SKILL.md 경로 미확인 — 다음 주간 갱신 때 재확인).
- 큐레이터에 웹용 탭 추가: 회사 공용 358개 중 웹에서 쓸 수 있는 순수 지침형 스킬 73개만 따로 모음(v1.12 신규 6개는 서브에이전트·스크립트·CLI 필요라 웹용 아님). SKILL.md 269개 직접 확인 + 77개 필드 기반 보수적 분류. 클로드 웹(Project instructions)·ChatGPT(GPTs Instructions)에 SKILL.md 붙여넣는 방법 안내 포함.

## 새로 추가된 것 (v1.9, +72개 — 주간 갱신 2026-11-09)
- 주요 소스 6개: MengTo 신규 digital-twin·url-to-motion-design, 공식 champion-kit(Anthropic, AI 도입 챔피언 7개 스킬), Codex 전용 codex-security(15개)·public-equity-investing(23개, Proprietary), 로컬 메모리 deja-vu(호스티드 없는 claude-mem 대안)
- GitHub 주간 Top 30 19개: cloudflare security-audit-skill(★66.8k, 주간 +10.2k)·rea-video(라이선스 추가돼 통과)·scenario-labs(86개)·untangle-codebase·skill-video-studio·spec-harness(PolyForm NC)·turkish-anti-ai-skill·flawless-ui-ux·shorts-kit·pwn-skill(CTF·학습용) 등. 중국어 전용 7개는 purpose_ko에 명시·대부분 optional
- skills.sh·HN·Trending 26개: hyperframes(★81k)·azure-skills(Microsoft 88개)·cloudflare/skills(22개)·marketingskills(55개)·emilkowalski/skills(18개)·Donchitos 게임 스튜디오(74개)·jakubkrehel/skills·gstack(★140,791)·supabase·prisma·neon·uizze·stickerforge(유일한 한국어 스킬) 등
- 유튜브 21개: claude-illegal-codes 5종(crash-out·67·aura-farming·china-maxing·larp)·MinerU(Apache-2.0+추가 조건)·OpenMontage(AGPL-3.0)·gsap-skills(공식 8개)·video-shotcraft·motionmaxxing·dream-loop·anatomy·3dviz-pro-max·onetake(PolyForm NC, 상용 불가)·collage-b-roll(Codex용·유료 Gemini 키)·youtube-agent-skill(11개) 등
- 변경: anthropics/skills docx/pdf/pptx/xlsx 라이선스 source-available → **Apache-2.0**(#2146, 2026-11-06). taste-skill 라이선스 미확인 → MIT, ★112,044. create-ai-avatar ★7,324.
- 제외: blender-mcp(SKILL.md 없음, MCP 서버라 스킬 아님), anti-detect-browser-skills(탐지 우회 이중용도), OpenRig·Hindsight(도구, 스킬 아님). 보류: gencraft-labs/skills·first-commit(스타 수와 설치 수 괴리), aws-core(SKILL.md 미확인), autonnel-skills(레포 미특정), Context Mode(라이선스·경로 불일치).
- 스타 수 주의: GitHub Core API 한도 소진으로 이번 주 신규분 스타 수는 upstar.dev·GitHub Search API·레포 페이지 직접 확인 값 사용, 정확한 API 재확인은 다음 갱신 때 수행.

## 새로 추가된 것 (v1.5, +9개 — 사용자 지목 스킬)
diagram-design·imagegen-skills(Leonxlnx 팩)·onyx-ai·hallmark·scrapling·impeccable·ponytail·rea(본체, ★427,237)·agent-desktop. 제외: imagegen(guillaumesimon, 라이선스 없음 — 같은 이름 MIT 팩으로 대체). 중복: last30days-skill(DB의 last30days와 동일). 주의: impeccable은 첫 실행 시 엔진 바이너리 다운로드 + 익명 사용 보고 전송(옵트아웃 가능), onyx-ai 스킬은 Apache-2.0이나 Onyx 플랫폼 본체는 커스텀 라이선스, hallmark는 npx 설치 후 상대 링크 깨짐 이슈 보고 있음, ponytail 저자 주장 벤치마크는 미검증.

## 새로 추가된 것 (v1.4, +37개 — 발견 출처 검증분)
- 유튜브 7개: leanrea(optional, 새 레포)·grill-me·show-me·learning-opportunities(CC-BY-4.0)·claude-video·agent-memory(Apache-2.0)·understand-anything
- 커뮤니티 8개 중 7개: playwright-skill·goose-skills(14개 플레이북 체제)·media-os·ai-act-compliance(EU 전용)·hancomdocs-skills(한국어 HWP)·ai-news-skill·vanilla-web. vc-skills는 SKILL.md 없어 제외
- GitHub 주간 Top 30 중 23개: pixelfx·stack-diagram·elephant-goldfish-model 등. 제외 7개(라이선스 없음 2·SKILL.md 없음 1·스킬이 아닌 것 2·베트남 법률 전용 1·중국 제도 전용 1). 이번 주 Top 30에는 중국어 스킬이 많아 포함된 것도 대부분 optional로 기록, 언어 전용 명시

## 새로 추가된 것 (v1.3, 주간 갱신 +4개)
- 경영/회계 3개: finance-analyst(SaaS 지표·런웨이·가격 분석), accounting-core(복식부기·결산), hr-admin(급여 계산·근태) — me-workspace/orchestrator-operational-agent-skills(MIT, ★3)에서 SKILL.md 직접 확인 후 추가. accounting-core·hr-admin의 세금/급여 규정은 인도네시아 기준이라 한국 적용 시 별도 검증 필요
- 영상 1개: create-ai-avatar(HeyGen 디지털 트윈 + ElevenLabs 클론 음성, MengTo/Skills, MIT, 유료 API 필요)
- 변경 없음: obra/superpowers v7.0.2는 brainstorming 튜닝·버그 수정만(스킬 추가/삭제 없음), openai/plugins 11-03 커밋은 인프라 정리(신규 플러그인 없음). ComposioHQ/awesome-codex-skills는 composio-community로 이동됨(301 확인)

## 새로 추가된 것 (v1.2, +70개)
- 대형 팩 43개: wshobson/agents 12, vercel-labs/agent-skills 7, K-Dense 과학 스킬 3, Composio 앱 자동화 6(Slack/Gmail/HubSpot/Jira/Notion/Stripe — API 키 필요), skills.sh/레지스트리 9
- 경영/회계/재무/인사/법무/운영 27개: business 13, finance 5, hr 5, legal 4 — Anthropic KWP finance/legal/HR/operations, financial-services, SYasJ business-skills 등
- 평가 25개 + 비교 쌍 7개

## 주의
- docx/pdf/pptx/xlsx는 2026-11-06부터 Apache-2.0(오픈소스, #2146)
- openai/skills는 deprecated, 후계는 openai/plugins. gh-fix-ci/gh-address-comments도 같은 사유로 보통/주의 판정
- Codex 경로는 `.agents/skills/` / `~/.agents/skills/` 기준
- 스킬은 임의 코드 실행 가능 — 설치 전 SKILL.md/스크립트 직접 확인, 버전 고정
- ui-ux-pro-max와 taste-skill, humanizer와 stop-slop은 같이 쓰지 않기
- 경영/법무 스킬은 대부분 미국·EU 기준. 한국 세법·K-IFRS·근로기준법·4대보험·개인정보보호법 전용 스킬은 미발견. 재무·세무·법률 스킬 산출물은 전문가 검토용 초안으로 쓸 것
- 미확인 표기가 있는 스킬은 설치 전 원본 README에서 설치 명령을 한 번 더 확인할 것
- SFC/GBA 홈브루 전용·에뮬레이터 QA 전용 범용 스킬은 공개 레지스트리에서 미발견 — emucap 직접 워크플로우가 여전히 필요
