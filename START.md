# 시작 명령 — 과제 폴더(과제 README가 있는 곳)로 이동한 뒤 한 줄씩

git -C /tmp/kit log -1 --oneline
pwd; ls -la
[ -e AGENTS.md ] && echo "AGENTS.md 있음: 직접 합치기" || cp /tmp/kit/AGENTS.md .
mkdir -p docs && cp -rn /tmp/kit/templates/. /tmp/kit/skills /tmp/kit/checklists docs/
git rev-parse --git-dir >/dev/null 2>&1 || git init -q
printf '.*\n!.gitignore\n!.env.example\n__pycache__/\n.pytest_cache/\n' >> .gitignore
git add -A && git status --short
git commit -m "시작: 과제 파일 + 하네스(codex-kit v4) 반입"
codex

# 메모
# - 첫 줄의 해시를 docs/HANDOFF.md '사용한 외부 자료'에 적는다.
# - git status에 설정·키 파일이 보이면 커밋하지 말고 .gitignore부터 확인한다.
# - 커밋할 때 사용자 정보 오류가 나면:
#   git config user.name "shdudgns" && git config user.email "shdudgns@users.noreply.github.com"
# - cp -n 경고 문구는 무시해도 된다.
# - codex가 열리면 /status. 일정표는 README.md '본선 일정표'.
