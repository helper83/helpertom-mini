# 의견 보내기 → 구글 드라이브 전송 (v1.8)

## 기본 동작 (설정 불필요)
- 검색 결과의 `의견 보내기` → 내용 입력 → `의견 보내기` 버튼. 기기에는 저장하지 않습니다.
- 전송 우선순위: ① 휴대폰/지원 브라우저: 공유 창에서 **드라이브** 선택 → helper83@gmail.com 계정에 CSV 저장
  ② 미지원(PC 등): CSV 파일 다운로드 + 구글 드라이브 업로드 안내 + 메일 보내기 버튼.

## 선택: 시트에 자동 기록 (Apps Script, 1회 설정)
브라우저 앱은 구글 계정 로그인 없이 드라이브에 직접 쓸 수 없으므로, 아래 웹앱을 만들면 버튼 한 번으로 시트에 쌓입니다.
1. helper83@gmail.com으로 구글 시트 `헬퍼탐 희망분야 메모` 생성 → 주소의 ID 복사
2. 확장 프로그램 → Apps Script에 아래 코드 붙여넣기 (SHEET_ID 수정)
3. 배포 → 새 배포 → 웹 앱 / 실행: 나 / 액세스: **모든 사용자** → URL 복사
4. index.html의 `WISH_WEBHOOK_URL`에 URL 입력 후 배포 (**2026-10-14 연결 완료**, 시트 `헬퍼탐 희망분야 메모`)

```js
const SHEET_ID = '여기에_시트_ID';
function doPost(e) {
  const d = JSON.parse(e.postData.contents);
  if (d.app !== 'helpertom-mini' || !Array.isArray(d.items)) return ContentService.createTextOutput('bad');
  const sh = SpreadsheetApp.openById(SHEET_ID).getSheets()[0];
  if (sh.getLastRow() === 0) sh.appendRow(['접수시각','구분','희망 분야','하고 싶은 일·이유','가까운 항목','검색어']);
  d.items.slice(0, 5).forEach(function (x) {
    sh.appendRow([x.at, x.kind, String(x.field).slice(0,80), String(x.why||'').slice(0,300), String(x.near||'').slice(0,100), String(x.query||'').slice(0,60)]);
  });
  return ContentService.createTextOutput('ok');
}
```

## 유의
- 웹앱 URL을 아는 사람은 누구나 전송할 수 있어 스팸 가능성이 있습니다(위 코드는 길이·건수만 제한). 필요하면 공유 비밀값 검사를 추가하세요.
- 브라우저는 no-cors로 보내 **성공 여부를 확인할 수 없습니다**. 시트에서 직접 확인하세요.
- 앱 소스는 공개 저장소라 WISH_WEBHOOK_URL과 helper83@gmail.com이 공개됩니다.
- 동의어 사전: index.html의 `INDUSTRY_SYN`, `JOB_SYN`(쉼표 목록)에 단어를 추가하면 됩니다. 영문 약어는 완전 일치만, 한글은 부분 포함 매칭.
