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

`skills-db-client/SKILL.md` — Codex·Claude Code에 설치해서 쓰는 클라이언트용 스킬입니다. API에서 목록을 받아 작업에 맞는 스킬 2–3개를 고르고, `skill_md_url`에서 SKILL.md를 직접 받아 적용하는 방법을 정합니다. 설치 방법:

- Claude Code: `~/.claude/skills/skills-db-client/` 폴더를 만들고 SKILL.md 저장
- Codex: `~/.agents/skills/skills-db-client/` 폴더를 만들고 SKILL.md 저장

## 사용 흐름

1. API에서 `skills.json` 전체를 받는다.
2. 작업 내용과 `purpose_ko`·`when_to_use`·`keywords`·`category`를 대조해 2–3개를 고른다.
3. 고른 스킬의 `skill_md_url`에서 SKILL.md 본문을 직접 받아 읽고 적용한다.
4. 설치 전에는 SKILL.md와 번들 스크립트를 직접 확인하고, 버전을 고정한다.

## 갱신

목록은 매주 월요일에 갱신됩니다. 갱신 주기는 `skills.json`의 `meta.verified_date`로 확인할 수 있습니다.

## 주의

- 스킬은 임의 코드를 실행할 수 있습니다. 설치 전 SKILL.md와 스크립트를 직접 확인하세요.
- 별점(5점 만점)은 어떤 출처에도 없습니다. 지어내지 않습니다.
- 스타 수는 레포 전체 수치이고, 인용 횟수는 수집 출처 기준입니다.
