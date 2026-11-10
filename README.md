# codex-kit

Codex(터미널 코딩 에이전트)로 데이터 기반 과제(분석·최적화·웹 서비스)를 할 때 쓰는 개인 작업 하네스입니다.
작업 규칙, 문서 템플릿, 체크리스트, 스킬 문서만 있고 실행 코드는 없습니다.

- v1·v2: 2026-10-30 작성. 10/31 예선에는 v2(`6c7ced8`, main 브랜치)를 사용했습니다. main은 그대로 둡니다.
- v3: 2026-11-04 작성(`retake` 브랜치, 태그 `v3`). 웹 서비스형 과제와 확인 질문 대비를 보강했습니다.
- v4: 2026-11-11 본선용(`final` 브랜치). 6시간 반 일정표, 제조(FAB) 데이터 확인 절차, 3분 설명 원고 틀을 더했습니다. 본선 시작 이후에는 바꾸지 않습니다.
- 이 저장소에는 시험 문제, 과제 파일, 결과물이 들어 있지 않습니다. 본선 결과 발표 전까지 공개 상태를 유지합니다.

## 구성

| 경로 | 용도 | 복사할 위치 |
| --- | --- | --- |
| `AGENTS.md` | Codex 작업 규칙 | 과제 폴더 루트 |
| `templates/` | REQUIREMENTS(요구사항 추적), AGREEMENT(담당자 합의), DECISIONS(결정·확정/가정), HANDOFF(인계·확인 질문 대비), PITCH(3분 설명 원고) | `docs/` |
| `skills/` | data-check(데이터 확인, 제조 데이터 절 포함), falsifiable-experiments(개선 주장 검증), web-service-ready(배포 전제 웹 서비스) | `docs/skills/` |
| `START.md` | 시작 명령만 모은 파일 (`cat`으로 보고 따라 치기) | 복사하지 않음 |
| `checklists/` | 담당자 인터뷰, Codex 프롬프트 예시, 제출 전 점검 (사람이 읽는 용도) | `docs/checklists/` |

## 사용법

clone한 뒤 `cat /tmp/kit/START.md`를 치면 아래 2)~4) 명령만 짧게 볼 수 있습니다.

```bash
# 1) 가져오기
git clone --branch final https://github.com/shdudgns/codex-kit /tmp/kit
git -C /tmp/kit log -1 --oneline     # 이 해시를 docs/HANDOFF.md '사용한 외부 자료'에 적는다

# 2) 과제 폴더(과제 README가 있는 곳)로 이동한 뒤 복사. pwd, ls -la로 먼저 확인
[ -e AGENTS.md ] && echo "AGENTS.md 이미 있음: 덮어쓰지 말고 직접 합치기" || cp /tmp/kit/AGENTS.md .
mkdir -p docs && cp -rn /tmp/kit/templates/. /tmp/kit/skills /tmp/kit/checklists docs/   # -n: 같은 이름 파일은 덮어쓰지 않음

# 3) 기록 시작 (git 사용자 정보가 없으면 채운다)
git rev-parse --git-dir >/dev/null 2>&1 || git init -q
git config user.name >/dev/null || git config user.name "shdudgns"
git config user.email >/dev/null || git config user.email "shdudgns@users.noreply.github.com"
printf '.*\n!.gitignore\n!.env.example\n__pycache__/\n.pytest_cache/\n' >> .gitignore
git add -A && git status --short     # 설정·키 파일이 섞여 있지 않은지 확인
git commit -m "시작: 과제 파일 + 하네스(codex-kit v4) 반입"
```

4) AGENTS.md를 복사한 다음에 `codex`를 실행한다. `/status`로 모델·권한을 확인한다.

## 본선 일정표 (09:30~16:00, 약 6시간 반)

과제 안내의 시간이 다르면 비율로 맞춘다. 점심(12:00~13:00)은 자리에서 먹는다고 보고 시계는 계속 간다.

| 시각 | 할 일 | 끝났다는 기준 |
| --- | --- | --- |
| 09:30 | 과제 README 정독, 제출물·채점 기준·금지 사항 확인 | REQUIREMENTS.md 초안(프롬프트 E) |
| 09:45 | 하네스 반입·첫 커밋, `codex` → `/status` | 커밋 해시 |
| 09:50 | 데이터 확인(프롬프트 A) + 담당자에게 목적 질문 | DECISIONS.md 데이터 가정 |
| 10:20 | 합의문, 기준선(현행 방식)과 성공 기준 정하기 | AGREEMENT.md, 담당자 확인 |
| 10:30 | 계획(프롬프트 D·F) 승인 | 모듈 목록 |
| 10:40~12:00 | 최소 동작판 + 기준선 측정 | **12:00에 결과 파일과 기준선 숫자 존재** |
| 12:00~13:00 | 점심. 오래 걸리는 실행을 돌려 두고 결과는 먹은 뒤 확인 | |
| 13:00~14:30 | 개선. 기준선 대비, 보지 않은 데이터로 판정 | 개선 숫자 + 판정 방법 |
| 14:30~15:15 | 검증(`submit.md`), 반대 증거, 화면 | 핵심 숫자 2개 직접 재계산 |
| 15:15 | **새 기능 동결** | |
| 15:15~15:40 | 제출 문서, HANDOFF.md, PITCH.md | 빈 칸 없음 |
| 15:40 | 제출 시작(종료 10~20분 전) | 제출 확정 |
| 이후 | PITCH.md와 HANDOFF 숫자 다시 읽기 | 원고 없이 3분 |

## 파일을 쓰는 순서 (3시간 기준)

| 시각 | 파일 | 할 일 |
| --- | --- | --- |
| 0:05 | `docs/REQUIREMENTS.md` | 과제 README의 요구·금지 사항·제출물을 ID별로 옮긴다(프롬프트 E로 초안, 내가 원문과 대조) |
| 0:15 | `docs/DECISIONS.md` 데이터 가정 | 데이터 확인 결과(프롬프트 A)와 파일 간 차이를 적는다 |
| 0:15 | `docs/checklists/interview.md` | 담당자에게 목적·규칙·편의 조건·예외를 2~3개씩 묻는다 |
| 0:35 | `docs/AGREEMENT.md` | 들은 답을 시각과 함께 적고 요약을 담당자에게 확인받는다. 못 들은 것은 '가정' 칸에 |
| 0:45 | 과제가 정한 실행 문서 | 양식의 제목만 먼저 만든다. 기능이 붙을 때마다 한 줄씩 채운다 |
| 0:45~2:35 | `DECISIONS.md` 결정 표 | 결정·AI 제안 처리마다 한 줄. 확정/가정, 출처, 틀리면 영향까지 |
| 단계마다 | `REQUIREMENTS.md` 상태 | 완료/부분/미구현과 검증 결과·미구현 사유를 갱신 |
| 2:35 | `docs/HANDOFF.md` | 최종 검증 결과, 핵심 숫자 5개, 30초 설명을 적는다 |
| 2:45 | 과제가 정한 제출 문서 | REQUIREMENTS·DECISIONS·HANDOFF에서 옮겨 마무리 → `submit.md` 점검 |
| 2:48 | (읽기만) | REQUIREMENTS 상태, DECISIONS 확정/가정, HANDOFF 숫자를 다시 읽는다 |

## 작업 흐름

1. 과제 README에서 제출물, 파일명, 저장 경로, 금지 사항을 먼저 REQUIREMENTS.md로 옮긴다.
2. 하네스를 반입하고 첫 커밋을 한 뒤 `codex` → `/status`.
3. Codex에 데이터 확인을 맡기고(프롬프트 A), 그동안 담당자에게 목적 질문부터 보낸다.
4. 인터뷰 → AGREEMENT.md → 모호한 점 점검(프롬프트 C) → 담당자 확인 → 커밋.
5. 계획 요청(프롬프트 D·F) → 승인 → 최소 동작판(규칙을 지키는 결과 + 독립 검증기)부터 → 기능 → 개선. 모듈마다 실행 결과 확인 후 커밋.
6. 중간 결과를 담당자에게 한 번 보여 주고, 합의문과 다르면 합의문부터 고친다.
7. 검증(`submit.md`) → 제출 문서 → 원장 읽기 → 제출.

과제 README와 과제 안내가 이 하네스보다 우선합니다.
