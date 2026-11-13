# skill-repo — 스킬 DB API

Codex·Claude Code 사용자가 API 요청으로 스킬 목록을 받아, AI가 직접 판단해서 각 스킬을 내려받아 쓰게 하는 시스템의 데이터 레포입니다.

## API (정적 JSON)

| 파일 | 내용 | URL |
|---|---|---|
| `skills.json` | 스킬 전체 (361개, v1.14) | https://raw.githubusercontent.com/galaga0128/skill-repo/main/skills.json |
| `skills-shared.json` | 회사 공용 (358개) | https://raw.githubusercontent.com/galaga0128/skill-repo/main/skills-shared.json |
| `references.json` | 참조 자료 목록 | https://raw.githubusercontent.com/galaga0128/skill-repo/main/references.json |

GET으로 받으면 JSON이 그대로 내려옵니다. 검색 파라미터는 없습니다. 전체를 받은 뒤 받는 쪽에서 필터링하세요.

## 스킬 항목 필드

- `id`, `name`, `category`, `purpose_ko`(무엇을 하는 스킬인지), `when_to_use`(언제 쓰는지), `example`, `keywords`
- `source_repo`, `skill_md_url`(SKILL.md 원본 위치), `install`(설치 방법), `license`, `compatible`(쓸 수 있는 도구), `requires`(필요 조건, 배열), `priority`(core/recommended/optional)
- `stars`(source_repo 레포 전체 스타 수 — 개별 스킬 수치가 아님), `citations`(수집 출처 기준 외부 추천 수 — 전 세계 인용 횟수가 아님)

## 클라이언트 스킬

`skills-db-client/SKILL.md` — Codex·Claude Code에 설치해서 쓰는 클라이언트용 스킬입니다. 설치 방법:

- Claude Code: `~/.claude/skills/skills-db-client/` 폴더를 만들고 SKILL.md 저장
- Codex: `~/.agents/skills/skills-db-client/` 폴더를 만들고 SKILL.md 저장

## 사용 흐름

1. **설치된 스킬 먼저 확인** — 외부 카탈로그를 조회하기 전에 현재 환경의 스킬 목록을 먼저 본다. 설치된 스킬로 충분하면 외부 탐색·설치 없이 그 스킬을 쓴다. 부족한 기능이 있거나 사용자가 대안을 요청한 경우에만 카탈로그를 조회한다.
2. **카탈로그 조회는 필요할 때만** — 목록을 캐시한다(상세 정책은 skills-db-client/SKILL.md 참조). 전체 JSON은 프로그램으로 필터링하고, 선택 개수는 고정하지 않는다(0개·1개도 정상). 네트워크 실패 시 캐시 사용 여부와 시점을 밝힌다.
3. **원본 확인** — 고른 스킬의 `skill_md_url`에서 SKILL.md 본문과 상대 참조(공통 문서·스크립트·의존 패키지)를 직접 확인한다. 원본이 저장소 루트면 README·파일 트리·플러그인 매니페스트에서 실제 진입점을 찾고, 못 찾으면 경로 미확인으로 보고한다. 카탈로그의 `install` 값은 참고 정보이며 실행 명령으로 바로 쓰지 않는다.
4. **상태 분리** — 발견·검증·설치·적용을 구분해서 기록한다(설치 여부와 경로, 원본 확인 여부와 리비전, 호환성 근거, 의존성 충족 여부, 승인, 실행 검증 여부). 후보를 찾았다는 것과 실제 실행 성공은 다르다.
5. **결과 전달** — 선정 스킬과 담당 역할, 설치 경로 또는 확인한 원본 주소·리비전, 선정 근거, 충족되지 않은 의존성, 필요한 승인, 제외한 후보와 이유, 스킬로 충족되지 않는 기능, 다음 행동을 전달한다.

클라이언트의 책임은 선정과 적용 준비까지다. 실제 작업 수행·에이전트 배정·실행 검증은 `dev-orchestrator`가 맡는다. 클라이언트가 별도의 개발 오케스트레이터로 확장되지 않는다.

## 갱신

목록은 매주 월요일에 갱신됩니다. 갱신 주기는 `skills.json`의 `meta.verified_date`로 확인할 수 있습니다.

## 주의

- 스킬은 임의 코드를 실행할 수 있습니다. 설치 전 SKILL.md와 스크립트를 직접 확인하세요.
- 별점(5점 만점)은 어떤 출처에도 없습니다. 지어내지 않습니다.
- 스타 수는 레포 전체 수치이고, 인용 횟수는 수집 출처 기준입니다.
