# GitHub Pages 퍼블리싱 설정

이 페이지는 정적 사이트로 GitHub Pages에 올리고, 접속자 미니미 위치 공유는 Firebase Realtime Database를 사용합니다. Firebase 익명 인증과 Realtime Database를 한 번 설정해야 방문자 간 동기화가 동작합니다.

## 1. Firebase 프로젝트 준비

1. Firebase Console에서 프로젝트를 만들고 웹 앱을 등록합니다.
2. Authentication에서 익명(Anonymous) 로그인 제공자를 켭니다.
3. Realtime Database를 만들고 URL을 기록합니다.
4. Firebase Authentication의 승인된 도메인 목록에 `<계정>.github.io`를 추가합니다. 사용자 지정 도메인을 연결한다면 그 도메인도 추가합니다.
5. `firebase-config.js`의 apiKey, authDomain, databaseURL, projectId, appId를 Firebase 웹 앱 설정값으로 교체합니다.
6. `database.rules.json` 내용을 Realtime Database의 Rules 탭에 게시합니다. 인증된 익명 사용자는 접속자 목록을 읽을 수 있고, 자신의 UID 하위 접속 데이터만 쓸 수 있도록 제한합니다.

## 2. GitHub Pages 켜기

저장소 루트에 `index.html`, `firebase-config.js`, `.nojekyll`을 올리고, 저장소 Settings → Pages에서 배포할 브랜치와 루트 폴더를 선택합니다. GitHub Pages는 정적 파일을 게시하므로 이 구성에는 Node 서버가 필요하지 않습니다.

Firebase SDK는 공식 CDN 모듈을 사용합니다. Firebase 웹 설정값은 브라우저에 공개되므로 비밀 키로 취급하지 않습니다. 대신 제공된 보안 규칙을 적용하고 Firebase Console에서 사용량과 예산 알림을 확인하세요.

브라우저가 연결되면 해당 탭의 미니미가 등록되고, 캐릭터 이동은 다른 방문자에게 동기화됩니다. 정상 퇴장이나 네트워크 단절 시 Firebase 서버 측 onDisconnect가 해당 탭의 캐릭터 데이터를 지웁니다.
