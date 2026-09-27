# 아이아이노트 (iiNote) 프로젝트 안내

## 어떤 앱인가
가족 단위로 쓰는 공유 일기 웹앱. 나중에 Capacitor로 안드로이드 앱으로 포장할 예정.
- 가족 그룹 만들기, 6자리 초대 코드로 합류. 어른은 각자 구글 로그인, 아이는 계정 없이 프로필(선택: 4자리 PIN)
- 서로의 일기 보기, 댓글 · 답글(여러 단계) · 내 댓글 수정
- 어린이 일기: 제출 전 1회 AI 맞춤법 확인 → 틀린 낱말을 quizWords에 저장 → 받아쓰기 퀴즈(빈칸 객관식, OX, 3번 연속 정답이면 익힘)
- 첫 화면: 어른은 프로필마다 모양 선택(일기장형 · 달력형, 예정: 4분할형 · 앨범형), 아이는 일기장형 고정. 일기장형 = 오늘의 일기 현황, 이번 주 기록표, 가족/나만 일정, 즐겨찾기(부모만 추가)
- 지우기: 일기 · 댓글(쓴 사람), 아이 프로필과 기록(부모), 내 계정과 기록(우리 가족 > 계정). 플레이스토어 요구 사항
- 어른: 사진 1~9장(장수별 분할 레이아웃 + 크게 보기), 위치(현재 위치 약 100m 범위 또는 직접 입력)
- 모두: 녹음 · 소리 파일(최대 3분), #태그(본문 #낱말 자동 추출), 태그 · 검색으로 모아 보기
- 핵심 강조점: 휴대폰 · 아이패드는 앱으로, 컴퓨터는 쉬운 주소(iinote.co.kr)로 같은 일기장에 접속

## 파일
- index.html: 앱 전체 (HTML+CSS+JS 한 파일, script type="module")
- icon.png: 앱 아이콘 (두 개의 i + 모눈 노트)
- privacy.html: 개인정보처리방침 (스타일을 페이지 안에 넣은 단독 파일, 앱의 우리 가족 > 계정과 첫 화면에서 링크)
- mockups/: 첫 화면 모양 미리보기 그림 (배포 안 함)
- _local/ (깃허브에 안 올라감, .gitignore): firestore.rules, storage.rules(파이어베이스 콘솔 규칙 탭에 붙여 넣는 원본), mockups/(첫 화면 모양 미리보기), iinote_icons/(아이콘 시안 10종)
- 작업 폴더: 첫 PC는 Downloads\iinote-repo, 둘째 PC는 Downloads\iinote (둘 다 저장소 루트를 그대로 받은 것)

## 기술 구조
- 파이어베이스 프로젝트: iinote (Authentication 구글 로그인, Firestore 서울, Storage는 Blaze 전환 후 사용 예정)
- 배포: 깃허브 저장소 github.com/vaca-at/iinote (루트에 index.html · icon.png · privacy.html · CNAME) → https://iinote.co.kr. 로컬 복제본: Downloads\iinote-repo
- 예전 주소 jsun.site/iinote (저장소 vaca-at/jsun 의 iinote 폴더, 로컬 Downloads\jsun)는 옮기기 전 버전
- FIREBASE_CONFIG의 apiKey가 "여기에"로 시작하면 localStorage로 도는 체험 모드
- 데이터: users/{uid}, invites/{code}, groups/{gid} 아래 profiles(homeStyle 포함), diaries, comments, schedules, links, quizWords, aiReports(이상한 AI 답변 신고)
- 카카오톡 공유: 카카오 JS SDK(t1.kakaocdn.net, 사용자가 요청해서 넣음) + KAKAO_JS_KEY(공개용 JavaScript 키) → Kakao.Share.sendScrap(SHARE_URL). 카드 내용은 OG 태그. 카카오 개발자 > 플랫폼 > Web에 https://iinote.co.kr, https://jsun.site 등록 필요
- 아이 일기 규칙: 말로 쓰기(한 번에 10글자 이상 들어오면 되돌림) · 붙여넣기 · 끌어다 놓기 막음, 브라우저 맞춤법 밑줄 끔
- 설정값: FEATURES(기능별 어른만/모두), MAX_PHOTOS=9, MAX_AUDIO_SEC=180, AI_WORKER_URL(아직 비어 있음), SITE_ADDRESS
- 디자인: 색은 모두 :root CSS 변수. 아이보리 #FBF6EE, 네이비 #1F2A44, 코랄 #EF6F53, 버터 #F4BE45, 모눈 배경, 컴퓨터 화면의 노트 여백선
- 글꼴: Pretendard(화면), Gowun Batang / Nanum Pen Script(일기 글씨). 앱 안에서 밝게 · 어둡게, 바탕체 · 손글씨 전환 버튼

## 다른 PC에서 이어서 작업하기
- `git clone https://github.com/vaca-at/iinote.git` 로 받은 폴더를 VS Code로 열고 그 폴더에서 바로 작업 (저장소 루트 = 작업 폴더)
- 처음 한 번: `git config user.name vaca-at`, `git config user.email bellachord@gmail.com`
- firestore.rules, storage.rules는 깃허브에 없음 (파이어베이스 콘솔 규칙 탭에 이미 적용됨. 원본은 첫 PC의 Downloads\iinote-repo\_local 폴더)
- terms.html: 이용약관 (privacy.html과 같은 스타일, 첫 화면 · 우리 가족 > 계정에서 링크)
- CLAUDE.md는 _config.yml의 exclude로 사이트(iinote.co.kr)에는 안 보이게 해 둠 (깃허브 저장소 페이지에서는 보임, 비밀 정보 넣지 않기)

## 대화 · 작업 방식
- **한국어로 대화하기**: 모든 대화는 쉬운 한국어로 (사용자는 교사, 바이브코딩)
- **작업하면 바로 푸쉬**: 코드를 고치면 따로 말하지 않아도 매번 푸쉬까지 한다. 중간 확인 없이 끝까지: 수정 → JS 문법 검사(script 부분을 node --check) → 커밋 → 푸쉬 → 사이트 반영 확인 → 결과 요약
- 푸쉬 전 `git pull --ff-only`. CNAME은 건드리지 않기
- 체험 모드 확인: apiKey를 "여기에"로 바꾼 복사본을 임시 폴더에 만들어 Edge 헤드리스로 캡처 (원본은 바꾸지 않기)

## 작업할 때 지켜 줄 것
- index.html 한 파일 구조 유지
- 광고 · 추적(애널리틱스) 라이브러리는 넣지 않기. 새 라이브러리는 추가 전에 먼저 물어보기
- 색은 반드시 CSS 변수로, 사용자 입력은 esc()로 감싸서 출력
- 코드를 크게 바꾸기 전에는 무엇을 왜 바꾸는지 먼저 짧게 설명하기
- 설명은 쉬운 한국어로 (사용자는 교사, 바이브코딩으로 작업)

## 지금 상태와 다음 할 일
1. ✅ 404 해결 (2026-09-27). 저장소 github.com/vaca-at/jsun 의 iinote(소문자) 폴더만 사용, 대문자 iiNote 폴더는 삭제함. 로컬 복제본: Downloads\jsun
2. ✅ 로그인 완료 (승인된 도메인 jsun.site → iinote.co.kr 추가 필요, 구글 로그인 사용 설정). 가족 만들기 · 초대 코드 합류 테스트 성공
   - 아이 합류 방식: 부모가 '아이 프로필 추가' → 휴대폰 없는 아이는 부모 폰에서 프로필 바꾸기로 사용 / 휴대폰 있는 아이는 '휴대폰 연결 링크'(?join=코드&kid=프로필ID&n=이름)를 문자로 받아 크롬에서 익명 로그인(구글 계정 불필요). 구글 계정 있는 아이는 초대 코드 + '아이' 선택으로도 가능
   - 콘솔에서 Authentication > 로그인 방법 > 익명(Anonymous) 사용 설정 필요
   - (선택) 프로젝트 지원 이메일을 전용 계정으로 바꾸기
2-1. iinote.co.kr 이전 마무리: 파이어베이스 승인된 도메인에 iinote.co.kr 추가, 깃허브 Pages에서 Enforce HTTPS 켜기, 예전 jsun.site/iinote는 새 주소로 넘겨 주기
2-2. ✅ 첫 화면 모양 4가지(일기장 · 달력 · 4분할 · 앨범) 완료. 화면 위 오른쪽에서 모양(어른만) · 일기 글씨 4가지(바탕 · 손글씨 · 고딕 · 동글) 고르기. 틀은 위쪽 제목 + 즐겨찾기 줄 + 색깔 폴더 탭 + 흰 종이 판(휴대폰은 아래 메뉴). 4분할형은 가로 화면에서 한 화면에 네 칸, 휴대폰은 세로로 한 칸씩
   - 예전 주소 jsun.site/iinote 에도 같이 올리는 중 (Downloads\jsun\iinote, SITE_ADDRESS만 jsun.site/iinote 로 바꿔서)
   - 다음 후보: 월말 리포트 탭(아이별 글자 수 · 자주 틀린 맞춤법 · 선생님 총평), 하루 펼쳐보기(가족 일기를 한 날짜에 나란히)
   - ✅ (2026-09-28) 4분할형: 가로 화면에서 스크롤 없이 한 화면에. 1 · 3 · 4칸은 글 대신 그림(✓ 도장, 🔥연속, 📔✏️🌱)으로 줄이고, 칸이 낮으면 차트 · 틀린 낱말부터 숨김(@container). 2칸(교환일기)만 스크롤
   - ✅ 모양 · 글씨 고르기를 글자 대신 도형 아이콘과 그 글씨체의 "가"로 (이름은 마우스를 올리면 보임)
   - (웹디자인 시안 1~20번 중 사용자가 1 · 9 · 10 · 20을 좋아함 → 20번은 원고지 디자인으로 들어감)
   - ✅ 예전 가족일기장 가져오기 (우리 가족 탭, 부모만): 예전 일기장(jsun.site/diary)과 공부방 아이 일기는 파이어베이스 moon-15f88의 diary/{날짜}_{me|doyun|dojin} 에 있음. OLD_DIARY_CONFIG로 두 번째 앱을 열어 bellachord 계정으로 읽고, 일기 · 댓글을 importKey("old:문서id")와 함께 복사 → 다시 눌러도 새 것만 + 예전 일기장에서 고친 일기(updatedAt > importAt)는 새 내용으로 바꿈. 빨간펜 기록(spellFixes · spellFeedback)은 aiResult로(어떻게 고쳤는지는 explain에), 아이 일기의 고친 낱말은 quizWords에. 하트 · 도장 · 날씨는 안 옮김. moon-15f88 승인된 도메인에 iinote.co.kr 추가 필요
   - ✅ 디자인 4가지 (화면 위 맨 왼쪽 견본 단추, 기기마다 기억 · html[data-design]): note(노트, 기본) · clean(깔끔: 공부방과 같은 흰 카드 · 파랑 · 밑줄 탭) · bright(산뜻: 색 카드 · 주황 · 알약 탭, 4분할 네 칸이 과목 카드처럼 색) · wongoji(시안 20 레드 원고지). 제목 글씨는 G마켓 산스(noonfonts jsdelivr, 공부방과 같은 파일). 시안 모음은 _local/designs, _local/themes, _local/themes2
3. 클라우드플레어 워커로 AI 맞춤법 확인 만들기 (API 키는 워커에만, 어린이 한 명당 하루 1회 제한은 KV로, 파이어베이스 ID 토큰 검증) → AI_WORKER_URL에 연결
4. Blaze 전환 후 Storage 켜고 storage.rules 적용 (사진 · 소리 저장)
5. 나중에: Capacitor 안드로이드 포장, 앱용 구글 로그인으로 교체