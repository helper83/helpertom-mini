# 배포 전 QA (tools/qa)

한 줄 요약: `python3 -I tools/qa/run_all.py` 한 번으로 전 탭·산업·직무 렌더, 코드 링크, 도움말 선택자, 390/768/1280px 가로 넘침·구분 버튼 클릭을 점검합니다(실패 시 종료 코드 1).

- 필요: `pip install playwright` + Chrome (`CHROME_PATH`로 경로 지정, 기본 `/usr/bin/google-chrome`)
- 자동 실행: `.github/workflows/qa.yml` — `index.html`·`sw.js`·`data/`·`tools/qa/`를 올리면 GitHub Actions가 실행하고, 실패하면 알림이 옵니다. Pages 배포 자체를 막지는 않습니다.
