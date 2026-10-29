# codex-kit

Codex(터미널 코딩 에이전트)로 데이터 문제 해결 과제를 할 때 쓰는 개인 작업 하네스입니다.
작업 규칙, 문서 템플릿, 체크리스트, 스킬 문서만 있고 실행 코드는 없습니다.

- 작성: 2026-10-30
- 이 저장소에는 시험 문제, 과제 파일, 결과물이 들어 있지 않습니다.
- SK하이닉스 AI 해커톤 2차 예선 결과 발표 전까지 공개 상태를 유지하고 내용을 바꾸지 않습니다.

## 구성

| 경로 | 용도 | 복사할 위치 |
| --- | --- | --- |
| `AGENTS.md` | Codex 작업 규칙 | 과제 폴더 루트 |
| `templates/` | AGREEMENT(담당자 합의문), DECISIONS(결정 기록), HANDOFF(인계 메모) | `docs/` |
| `skills/` | data-check(데이터 확인), falsifiable-experiments(개선 주장 검증) | `docs/skills/` |
| `checklists/` | 담당자 인터뷰 질문, 제출 전 점검 (사람이 읽는 용도) | `docs/checklists/` |

## 사용법

```bash
# 1) 가져오기
git clone https://github.com/shdudgns/codex-kit /tmp/kit
git -C /tmp/kit log -1 --oneline     # 이 해시를 docs/HANDOFF.md '사용한 외부 자료'에 적는다

# 2) 과제 폴더(과제 README가 있는 곳)로 이동한 뒤 복사. pwd, ls -la로 먼저 확인
[ -e AGENTS.md ] && echo "AGENTS.md 이미 있음: 덮어쓰지 말고 직접 합치기" || cp /tmp/kit/AGENTS.md .
mkdir -p docs && cp -rn /tmp/kit/templates/. /tmp/kit/skills /tmp/kit/checklists docs/   # -n: 같은 이름 파일은 덮어쓰지 않음(경고가 떠도 정상)

# 3) 기록 시작 (git 사용자 정보가 없으면 채운다)
git rev-parse --git-dir >/dev/null 2>&1 || git init -q
git config user.name >/dev/null || git config user.name "shdudgns"
git config user.email >/dev/null || git config user.email "shdudgns@users.noreply.github.com"
printf '.*\n!.gitignore\n__pycache__/\n.pytest_cache/\n' >> .gitignore
git add -A && git status --short     # 설정·키 파일이 섞여 있지 않은지 확인
git commit -m "시작: 과제 파일 + 하네스(codex-kit) 반입"
```

4) AGENTS.md를 복사한 다음에 `codex`를 실행한다. `/status`로 모델·권한을 확인한다.
5) 담당자 인터뷰(`docs/checklists/interview.md`) → `docs/AGREEMENT.md` 작성 → Codex에 계획부터 요청한다.

과제 README와 과제 안내가 이 하네스보다 우선합니다.
