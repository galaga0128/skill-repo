---
name: skills-db-client
description: "스킬 DB API에서 작업에 맞는 스킬을 찾아 내려받아 사용한다. 코드·글·디자인·분석 등 어떤 작업을 시작하기 전에 이 스킬을 먼저 적용해, 이미 검증된 스킬이 있는지 확인한다."
---

# 스킬 DB 클라이언트

스킬 DB는 361개 스킬(2026-11-14 기준, v1.14)의 목록을 JSON API로 제공한다.
이 스킬은 그 API를 조회해 작업에 맞는 스킬을 고르고, 원본 레포에서 SKILL.md를 직접 받아 적용하는 방법을 정한다.

## API

- 스킬 목록: `https://raw.githubusercontent.com/galaga0128/skill-repo/main/skills.json`
  - 회사 공용(358개): `https://raw.githubusercontent.com/galaga0128/skill-repo/main/skills-shared.json`
  - 참조 자료: `https://raw.githubusercontent.com/galaga0128/skill-repo/main/references.json`
- 목록은 매주 월요일에 갱신된다. 갱신 직후가 아니면 최신 상태가 아닐 수 있다.
- 응답은 JSON 한 파일이다. `skills` 배열의 각 항목은 아래 필드를 갖는다.
  - `id`, `name`, `category`, `purpose_ko`(무엇을 하는 스킬인지), `when_to_use`(언제 쓰는지), `example`, `keywords`
  - `source_repo`, `skill_md_url`(SKILL.md 원본 위치), `install`(설치 방법), `license`, `compatible`(쓸 수 있는 도구), `requires`(필요 조건, 배열), `priority`(core/recommended/optional)
  - `stars`(source_repo 레포 전체 스타 수 — 개별 스킬 수치가 아님), `citations`(수집 출처 기준 외부 추천 수 — 전 세계 인용 횟수가 아님)

## 워크플로우

1. **목록 받기**: 작업이 시작되면 먼저 API에서 skills.json 전체를 받아온다. 목록을 하드코딩하지 않는다. API가 소스 오브 트루스다.
2. **고르기**: 작업 내용과 `purpose_ko`·`when_to_use`·`keywords`·`category`를 대조해 2–3개를 고른다. 기준은 다음과 같다.
   - 작업과 직접 맞는 것을 먼저 고른다. 비슷한 스킬이 여러 개면 `priority`가 높은 것, `citations`가 있는 것을 우선한다.
   - `compatible`에 지금 쓰고 있는 도구(Claude Code 또는 Codex)가 포함된 것만 고른다. 한쪽 전용이면 설치 전에 사용자에게 알린다.
   - `requires`에 유료 API 키·별도 CLI·특정 계정이 있으면, 설치 전에 사용자에게 알린다.
   - 한국어 전용 스킬(세법·근로기준법·개인정보보호법 등)은 DB에 거의 없으니, 관련 작업에서는 초안·검토용으로만 쓰고 전문가 검토가 필요하다고 알린다.
3. **SKILL.md 받기**: 고른 스킬의 `skill_md_url`에서 SKILL.md 본문을 직접 받아 읽는다. 목록의 요약만으로 적용하지 않는다.
   - URL이 `github.com/<owner>/<repo>/blob/<branch>/<path>` 형태면 `raw.githubusercontent.com/<owner>/<repo>/<branch>/<path>` 로 받는다.
   - URL이 레포 루트만 가리키면 후보 경로를 순서대로 시도한다: 루트 `SKILL.md`, `skills/<id>/SKILL.md`, `.claude/skills/<id>/SKILL.md`.
   - 컬렉션(k-skill, gbrain)이나 플러그인(claude-mem)은 SKILL.md 하나가 아니라 설치·설정 절차가 따로 있으니, 해당 항목의 `install` 안내를 그대로 따른다.
4. **적용하기**: 받은 SKILL.md의 지침을 작업에 적용한다. 설치가 필요한 스킬은 아래 안전 규칙을 먼저 확인한다.
5. **보고하기**: 사용자에게 고른 스킬 이름·고른 이유·적용 결과를 한 줄씩 알린다. 고른 스킬이 없으면 "DB에 맞는 스킬이 없다"고 알리고, 가장 가까운 대안 하나를 제시한다.

## 안전 규칙 (반드시 지킬 것)

- 설치 전에 SKILL.md와 번들 스크립트를 직접 읽고, 무엇을 실행하는지 확인한다. 스킬은 임의 코드를 실행할 수 있다.
- 버전(커밋·태그)을 고정하고, 최신 main을 무조건 믿지 않는다.
- 별점(5점 만점)은 어떤 출처에도 없으니 지어내지 않는다. 신뢰도는 스타 수·최근 활동·중복 추천·장단점으로 판단한다.
- ui-ux-pro-max와 taste-skill, humanizer와 stop-slop은 같이 쓰지 않는다.
- `trust: 낮음`으로 평가된 스킬은 직접 테스트한 뒤에만 쓴다.
- 스킬을 자동으로 설치하지 않는다. 설치는 사용자가 승인한 뒤에 한다.

## 한계

- API는 정적 JSON 파일이다. 검색 파라미터(`?category=` 같은 것)는 없다. 전체를 받은 뒤 이쪽에서 필터링한다.
- 목록은 매주 월요일에 갱신된다. 갱신 직후가 아니면 최신 상태가 아닐 수 있다.
