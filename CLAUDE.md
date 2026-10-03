# 아이아이노트 (iiNote) 프로젝트 안내

## 어떤 앱인가
가족 단위로 쓰는 공유 일기 웹앱. 나중에 Capacitor로 안드로이드 앱으로 포장할 예정.
- 가족 그룹 만들기, 6자리 초대 코드로 합류. 어른은 각자 구글 로그인, 아이는 계정 없이 프로필(선택: 4자리 PIN). PIN 바꾸기(2026-09-30, pinForm): 부모는 우리 가족 > 가족 구성원의 🔑 비밀번호(지금 PIN 없이 새로 · 없애기), 아이는 계정에서 스스로(지금 PIN 확인). profiles.pinAt
- 서로의 일기 보기, 댓글 · 답글(여러 단계) · 내 댓글 수정
- 어린이 일기: 다홍펜 선생님(공부방 다홍펜과 같은 워커 family-spell) → 화면에서는 "AI 다홍펜 선생님" · "AI 선생님"으로 부름 → 최소 글자 수를 채우면 검사 → 노란 형광펜(고칠 곳) · 초록(고친 곳), 힌트 · 정답 보기 · 이대로 두기, 1 · 2학년은 자동 고침, 칭찬 · 응원 한마디, 다 고쳐야 제출(검사는 일기당 3번까지) → 틀린 낱말을 quizWords에 저장 → 받아쓰기 퀴즈(빈칸 객관식, OX, 3번 연속 정답이면 익힘)
- 칭찬 도장 · 하트: 부모가 아이의 제출한 일기에 도장 하나(STAMPS 4가지, diaries.stamp · stampBy), 하트는 가족 누구나(diaries.hearts = {프로필id:true}). 예전 일기장의 도장 · 하트도 가져오기 때마다 합쳐 옴
- admin.html: 운영 현황판 (bellachord 계정만, 파이어베이스 getCountFromServer 로 개수만 셈. 가족은 '가족 1 · 2…', 사람은 '보호자 / 아이 N학년'으로만). 처음 쓰려면 콘솔 Firestore 규칙에 isIinoteAdmin() 줄을 넣어야 함 (현황판 화면이 붙여 넣을 규칙을 보여 줌). 사람별 최근 7일은 색인이 한 번 필요 (화면에 링크)
- 일기 PDF로 저장 (일기 탭 위 '📄 PDF로 저장'): 새 라이브러리 없이 새 창 + 브라우저 인쇄(PDF로 저장). A4 컬러, 표지 + 쪽 구성 10가지(PDF_LAYOUTS: 기본 · 하루 모음 · 위 사진/아래 글 · 위 사진/세로 글 · 왼쪽 사진/오른쪽 글(A4 가로) · 왼쪽 글/오른쪽 사진(A4 가로) · 사진+원고지 · 원고지 · 두 편씩 · 앨범). 인쇄소 찾기(카카오맵 · 네이버 검색 링크, 제휴 아님), 종이 일기책은 '출시 예정' 안내만. iinote에는 그림 그리기 기능이 없음 (그림일기라고 부르지 않기)
- 첫 화면: 어른은 프로필마다 모양 선택(일기장형 · 달력형, 예정: 4분할형 · 앨범형), 아이는 일기장형 고정. 일기장형 = 오늘의 일기 현황, 이번 주 기록표, 가족/나만 일정, 즐겨찾기(부모만 추가)
- 지우기: 일기 · 댓글(쓴 사람), 아이 프로필과 기록(부모), 내 계정과 기록(우리 가족 > 계정). 플레이스토어 요구 사항
- 사진 1~9장(장수별 분할 레이아웃 + 크게 보기): 2026-09-30부터 아이 일기에도 (5~7세는 보호자가 넣어 줘도 됨, FEATURES.photos="all"). 가족 채팅 사진은 어른만
- 어른: 위치(현재 위치 약 100m 범위 또는 직접 입력)
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
- 이름: 화면 · 약관 · 방침 · 공유 그림에서는 'AI 다홍펜 (선생님)' (2026-09-29 빨간펜 → 다홍펜). 워커 family-spell 지시문과 공부방(jsun)은 아직 '빨간펜'
- 파이어베이스 프로젝트: iinote (Authentication 구글 로그인, Firestore 서울, Storage는 Blaze 전환 후 사용 예정)
- 배포: 깃허브 저장소 github.com/vaca-at/iinote (루트에 index.html · icon.png · privacy.html · CNAME) → https://iinote.co.kr. 로컬 복제본: Downloads\iinote-repo
- 예전 주소 jsun.site/iinote (저장소 vaca-at/jsun 의 iinote 폴더, 로컬 Downloads\jsun)는 옮기기 전 버전
- FIREBASE_CONFIG의 apiKey가 "여기에"로 시작하면 localStorage로 도는 체험 모드. 👀 로그인 없이 둘러보기(2026-10-03): 첫 화면 단추(act tour) 또는 iinote.co.kr/?demo → sessionStorage iinote-tour 가 있으면 TOUR=DEMO (예시 가족, 엄마로 바로 들어감, 맨 위 .tour-bar '진짜로 시작하기'=tour-exit, 둘러보기 중 로그아웃도 tour-exit). 로그인된 기기(ls iinote-login)에서 ?demo 로 오면 TOUR_ASK → '로그아웃하고 둘러볼까요?' 창(tour-logout · tour-no), 실제로는 로그아웃 상태면 바로 둘러보기. 검색 로봇용: #app 안에 정적 소개 글(.seo-intro, 앱이 뜨면 바뀜) + JSON-LD WebApplication
- 데이터: users/{uid}, invites/{code}, groups/{gid} 아래 profiles(homeStyle 포함), diaries, comments, schedules, links, quizWords, aiReports(이상한 AI 답변 신고)
- 🎙 이야기로 일기 쓰기 (5~7세, 2026-09-28): 아이 일기 쓰기 위 칸(talkOK). 엄마 · 아빠와 대화 녹음(최대 3분, recStart(true)) → 녹음은 d.audio 로 일기에 남음 → toWav16k 로 16kHz WAV → 워커 POST /transcribe (Workers AI @cf/openai/whisper-large-v3-turbo, 워커에 AI 바인딩 필요) → 워커 mode "talk" (Claude, TALK_BASE: 아이가 한 말만으로 "나는~" 일기, dialog · diary · mood · note) → 일기 칸에 넣고 부모가 고쳐 제출. diaries.talk = {transcript, dialog, note, diary, made}. 이야기 일기는 글자 수 · 다홍펜 조건 없이 제출. 일기당 만들기 3번(TALK_MAX_MAKE)
- 날씨 · 기분 (2026-09-29): 일기 쓰기 위에 날씨 7(WEATHERS: 맑음 · 구름 조금 · 흐림 · 비 · 천둥 번개 · 눈 · 무지개 → diaries.weather) · 기분 7(MOODS: 좋아요 · 신나요 · 행복해요 · 그저 그래요 · 속상해요 · 화나요 · 피곤해요, 예전 5개 키 그대로) 이모지로 고르기. moodChip(mood, weather)
- 이름이 같은 어른 프로필 합치기: dupBanner(부모 화면 위 노란 띠) → mergeSameName: 일기 많은 쪽을 남기고 다른 쪽 uid 를 uids 에 넣은 뒤 mergeProfiles 로 기록 옮김 (다른 로그인 확인 없이)
- 목소리 · 소리 칸: 아이는 7세 이하만 (allow("audio")), 어른은 그대로. 비밀번호 칸은 모두 .pin-in 으로 ●●●●
- 초6까지 아이 일기 쓰기 화면(kidUI): 공부방 일기장처럼 머리(○○의 일기장 · 🔥 연속) · 8시 알림 · 오늘/어제 · 💡 오늘의 질문(KID_QS, 고르면 diaries.prompt) · 공책 줄(빨간 여백선) · ✏️ 글자 수 막대(kwProgress/syncLen) · 🔒 다홍펜(글자 다 채우면 열림) · 💾 다 썼어요!
- 💬 가족 채팅 (2026-09-29): 넓은 화면(1100px~)은 종이 판 오른쪽 바깥의 좁은 채팅 기둥(viewChatSide, 300px, 접기 · 펼치기 ls iinote-chat-side), 좁은 화면은 탭 "가족 채팅"(chat). 채팅 목록은 .chat-list 클래스로 두 곳, 보이는 것만 씀(chatBoxes). groups/{gid}/chats {profileId,text,photo,createdAt,deleted}, onSnapshot 으로 최근 150개(CHAT_LIMIT) 실시간. 사진은 어른만, 일기 사진처럼 줄여서 groups/{gid}/diaries/chat/ 에 올림(저장소 규칙 그대로). 90일(CHAT_KEEP_DAYS) 지난 것은 어른이 채팅 열 때 정리. 새 메시지는 탭에 빨간 숫자(ls iinote-chatseen-…). 새 메시지가 와도 다른 화면은 다시 그리지 않음(쓰던 일기 보호). 엔터 = 보내기(한글 조합 중 제외)
- ✍️ 다홍펜 고치는 방법 (2026-09-29): 7세 이하 · 초1 · 초2(penYoung) 아이마다 보호자가 우리 가족 → 'AI 다홍펜 고치는 방법'에서 고름 → profiles.penAuto (true 바로 고쳐 줌 · false 스스로, 없으면 바로 고쳐 줌 = 예전과 같음). 안 고른 아이가 있으면 보호자 홈 맨 위에 안내 띠(penAskBanner → pen-set). 초3부터는 늘 스스로. 마침표만 붙이는 고침은 끝 네 글자만 보여 줌(penTrim), 이모지 뒤 붙여쓰기는 띄어쓰기 오류로 안 잡음. 7세(age7)는 family-spell 워커 LEVELS.age7 지시문을 고쳐서(2026-09-29, 39efb10) 소리 나는 대로 · 붙여 쓴 말도 하려던 말을 찾아 덩어리째 고침(최대 6개)
- 휴대폰 화면 자동 점검 (2026-09-30): 체험 복사본 + Edge --remote-debugging-port 로 Emulation.setDeviceMetricsOverride(320 · 360 · 390px) → 3명(엄마 · 아이 둘) × 13화면에서 가로 넘침 · 글자 겹침 · 큰 체크 상자 검사, 모두 통과. 단 Edge 는 아이폰 사파리의 체크 상자 늘어남을 재현 못 함 → 전역 input{width:100%} 아래 체크 상자 · 라디오는 width:auto 규칙으로 막음
- 테스트 팁: Edge 헤드리스 --virtual-time-budget 에서는 홈 · 채팅 화면의 createImageBitmap 이 멈춤(가상 시간 탓). 사진은 --remote-debugging-port 로 실제 시간 실행 뒤 Runtime.evaluate 로 확인
- 검색 등록: naver…html · google…html 확인 파일, robots.txt(다음 확인 코드 포함) · sitemap.xml
- 🔔 새 소식(newsItems): 내 일기의 댓글 · 내 댓글의 답글 · 하트(hearts 값 = 누른 시각) · 도장(stampAt), 어른은 아이가 쓴 일기. 최근 2주, 안 본 것 수는 ls iinote-news-{gid}-{pid}. 따로 저장하는 컬렉션 없음
- 🏅 스티커판(STICKERS 14개, stickerStats): 아이의 내 일기 탭 위 + 아이들 현황 카드. 제출하면 새 스티커 토스트. 계산만 하고 저장 안 함
- 채팅 반응: chats.reactions = {프로필id: 이모지} (REACTS 6개, 한 사람당 하나)
- 채팅 답장 (2026-09-29): 메시지 시간 아래 '답장' → S.chatReply, 글 칸 위 띠(chatReplyBar · drawReplyBars, 쓰던 글 유지) → chats.replyTo = {id, profileId, text(앞 60자), photo}. 말풍선 안 인용(chatQuote)을 누르면 원래 메시지로 스크롤 + 반짝(chat-jump, #cm-{id})
- 휴대폰 점검 (2026-09-29): 아래 메뉴는 640px 이하에서 짧은 이름(NAV_SHORT: 채팅 · 현황 · 하루 · 리포트 · 퀴즈 · 설정), .st-name flex:1 1 auto(짧은 이름 '지..' 잘림 고침), 아이 쓰기 화면 .kw-prog 줄바꿈(가로 넘침 고침). 체험 모드로 390 · 360px 전 화면 가로 넘침 없음 확인
- PWA: manifest.json + icon-192/512.png (서비스 워커는 없음, 캐시 문제 피하려고)
- 다홍펜 꼭 고칠 낱말 (2026-09-30): 워커 src/index.js 의 MUST_FIX = [[틀린 말, 바른 말, 힌트, 설명]] (첫 줄 홈럭볼 → 홈런볼). 지시문에 목록으로 들어가고, AI 가 빠뜨리면 워커가 직접 errors 에 넣음. 보호자가 새 기준을 말하면 여기에 한 줄 더하고 deploy. (Git Bash curl 은 한글이 깨지니 시험은 node fetch 로). 문맥에 안 맞는 낱말(아무거나 섰다 → 썼다)도 고치게 지시문에 넣음 (정말 선 '섰다가'는 그대로 두는 것 확인)
- 🌸 따뜻한 일기장 디자인 (2026-09-30, data-design="warm"): 가족일기장(jsun.site/diary) 크레파스 테마처럼 분홍 책상 · 파스텔 인덱스 탭(아래로 살짝 내려앉음) · 색 띠 카드. 하루 펼쳐보기(dayCard)는 모든 디자인에서 도장을 본문 위 동그란 도장(.day-stamp), 아이 일기 다홍펜 고친 곳 · 칭찬 · 처음 쓴 글, 댓글 · 답글 모두 + 바로 한마디(form data-diary) · 하트 · 도장
- 운영 현황판 (2026-09-30): 글씨 윤탱체, 접속 현황 최근 7일 + 펼쳐 보기, 13주 접속 잔디, 로그인 계정 나눠 보기(users.kind = google · kakao · both · kid, 앱이 가족에 들어간 계정에만 markKind 로 적음)
- 예전 가족일기장 jsun.site/diary 에서 iinote 같이 보기 (2026-09-30, 저장소 jsun): 두 번째 파이어베이스 앱 "iinote" 에 bellachord 구글로 로그인('🌐 iinote 일기 같이 보기') → 도윤 · 도진 프로필이 있는 가족의 제출한 일기 · 댓글 · 하트 · 도장을 읽기만 해서 S.entries 에 합침(같은 날 같은 사람이면 예전 일기장 것이 먼저, importKey old: 는 뺌). 쓰기 · 도장 · 댓글은 iinote 에서
- 예전 주소 jsun.site/iinote 는 iinote.co.kr 로 넘기는 안내 페이지만 (쿼리 그대로). 더는 거기에 올리지 않음
- 주소 · 뒤로 가기 (2026-10-03): 탭마다 주소 / · /mine · /chat · /kids · /day(?date=) · /shelf · /report · /quiz · /family, 일기 읽기 ?d=id, 쓰기 /write, PDF /pdf. 화면이 바뀌면 syncUrl 이 pushState, popstate 에 applyUrl (쓰던 일기가 저장 전이면 물어봄). 깃허브 페이지는 없는 주소를 404.html 로 보여 주므로 404.html 이 /?p=원래주소 로 넘기고 앱이 첫 줄에서 replaceState 로 되돌림. 하위 주소가 한 단계라 icon.png 같은 상대 주소도 그대로 됨
- 첫 화면 4분할 (2026-10-03): ① 달력 미니(quadCal) ② 일기 히스토리 ③ 성장 기록(아이마다 위아래 growBlock) ④ 앨범(albumPhotos, 사진 수에 맞춰 4×3 · 3×2 · 2×2로 칸을 꽉, 누르면 갤러리 album-lb). 앨범형: 왼쪽 ⭐ 베스트(글 가장 긴 3편) · 오른쪽 3×8 격자, 🔀 섞기(S.albumSeed). 책장 · 내 일기 카드는 feedHTML(넓으면 3 · 2 · 1 기둥, 높이 어림해 짧은 기둥에), 사진은 원래 비율(phRatio). 🔔 새 소식은 오른쪽 위 떠 있는 창(.news-panel fixed + .news-bg)
- 📸 가족 사진첩 (2026-10-03, 탭 album · 주소 /album): groups/{gid}/album {url,path,thumb,thumbPath,w,h,date,people,place,by,createdAt}. 사진 파일은 무료 5GB 되는 미국 버킷 ALBUM_BUCKET="gs://iinote"(규칙 원본 _local/storage-album.rules, 사진만 10MB) 에 groups/{gid}/album/ 로 (B.uploadMedia(path,blob,"album")). 긴 변 1600px(ALBUM_BIG) + 미리보기 400px. 연도 = photoDate(EXIF DateTimeOriginal, 없으면 파일 날짜, 올릴 때 고칠 수 있음), 사람 = 올릴 때 가족 고르기, 사진마다 찍은 날 · 장소(datalist) · 동네(area, 장소 이름으로 Nominatim 검색 → areaOf "서울 송파구", 못 찾으면 직접. 사진 속 GPS(photoMeta · exifInfo)가 있으면 gpsArea 가 역검색으로 먼저 채움, 좌표는 저장 안 함 · 아이폰 사파리는 보통 위치를 빼고 줌) · 메모(memo 100자, 크게 보기에서 노란 쪽지 .lb-memo) 따로, 사람은 올릴 때 모두 같이. 첫 사진 장소 · 날짜 모두에(al-same). 연도별 · 사람별 · 함께 찍은(with: 고른 사람 조합으로 저절로, 온 가족 · 둘이 · 셋이) · 장소별 · 동네별(S.albumBy, ls iinote-album-by), 누르면 크게 보기(caps.al → ✏️ 고치기). 가족 모두 보고 올림, 고치기 · 지우기는 올린 사람 · 부모. 탭 처음 열 때만 불러옴(loadAlbum). 올리기는 한 장씩 크게(.al-car, ◀ 1/3 ▶ · 아래 작은 사진 줄 · 밀어 넘기기 .al-swipe), 크게 보기에서 ✏️ 고치기 = 그 자리 고치기 칸(.lb-edit, 휴대폰 아래 · 컴퓨터 오른쪽, S.alEdit.inLb), 넘기면 albumSaveEdit 로 저절로 저장. 크게 보기 사진 왼쪽 반/오른쪽 반 누르면 이전/다음(lb-tap). 얼굴 인식 없음(무료 아님). 방침에 국외 이전(미국) 적음
- 첫 화면(2026-10-03): 단추 4개 2×2(구글 · 카카오 · 👀 데모 버전 둘러보기 · 📲 홈 화면에 추가(a2hs: beforeinstallprompt 있으면 설치 창, 없으면 기기별 방법 창)) + 데모 주소 iinote.co.kr/?demo · 복사는 데모 박스(.tour-box) 안에, 약속 배지에 가족 채팅 · 가족 사진첩 추가(이런 것도 할 수 있어요 칸은 뺌). 앱 안 브라우저(INAPP) 안내 · SELF_AUTH(로그인 도우미 __/auth/ 를 사이트에, 켜려면 구글 OAuth · 카카오 리다이렉트 URI 에 https://iinote.co.kr/__/auth/handler 등록 필요, 홈 화면 앱은 redirect 로그인)
- 레이아웃(첫 화면 모양) 고르기는 홈에서만
- 일정 순서 (2026-10-03): 시간과 상관없이 먼저 넣은 일정이 위(bySch: schedules.order ?? createdAt). 하루 목록 아래 '↕ 순서 · 고치기'(S.schEdit=날짜) → ▲▼(sch-move, 이웃과 order 맞바꿈, 가족 누구나) · ✏️ 고치기(sch-edit 폼, 쓴 사람 · 부모) · ✕ 지우기. 공통 그리기 schList(달력 카드 · 다가오는 일정)
- 일기장 이름 바꾸기 (2026-10-03): 우리 가족 설정 > 초대 카드 아래(부모만, data-form="group-name") → groups/{gid}.name (최대 20자, 위쪽 '○○ 일기장'에 보임)
- 백업: 우리 가족 설정(부모만) → 일기 · 댓글 CSV (엑셀용 BOM, =+-@ 로 시작하는 칸은 ' 붙임)
- 자동 로그아웃: 첫 화면 · 계정에 '이 기기에서 로그인 유지'(iinote-keep). 로그인은 늘 local 저장(새 탭 · 공부방 ?write 링크에서도 유지). 켜면 30일(KEEP_DAYS) 뒤, 끄면 30분(IDLE_MIN) 안 쓰면 autoLogoutCheck 가 로그아웃 (iinote-login · iinote-active 시각으로 판단). 일기 쓰는 중엔 기다림. 익명(아이 휴대폰 연결) 계정은 제외
- 위쪽 머리: 맨 위 홈 단추(.home-btn) · 휴대폰은 1줄(홈 · 제목 · 🎨) + 2줄(나 · 즐겨찾기 · 로그아웃), 🎨 누르면 화면 구성 → 디자인 → 글씨
- 운영 현황판: 워커 GET /usage 로 AI 횟수 · 토큰 · 예상 금액(Opus 5 입력 $5 · 출력 $25 /100만 토큰, 1달러≈1,400원). /usage 는 아이 이름 없이 who {iinote, other} 만
- 카카오 로그인 (2026-09-28): 파이어베이스 Identity Platform 업그레이드 + 로그인 방법 OpenID Connect(이름 kakao → oidc.kakao, 코드 흐름, 발급자 https://kauth.kakao.com, 클라이언트 ID = 카카오 REST API 키, 비밀번호 = 카카오 클라이언트 시크릿). 카카오 쪽: 카카오 로그인 · OpenID Connect ON, 리다이렉트 URI https://iinote.firebaseapp.com/__/auth/handler, 동의항목 닉네임만(이메일 안 받음). 코드: signInKakao = signInWithPopup(OAuthProvider("oidc.kakao")), reauth도 카카오 계정이면 카카오로. 구글 · 카카오는 서로 다른 계정. OIDC는 한 달 로그인 50명까지 무료
- 카카오톡 공유: 카카오 JS SDK(t1.kakaocdn.net, 사용자가 요청해서 넣음) + KAKAO_JS_KEY(공개용 JavaScript 키) → Kakao.Share.sendScrap(SHARE_URL). 카드 내용은 OG 태그 (og.png 1200×630: 버터 판 2분할 · 왼쪽 제목 + 키워드 4개(매일 일기 습관 · AI 다홍펜 · 맞춤법 공부 · 성장 기록), 오른쪽 로고 + iinote.co.kr. 원본 _local/og/final.html + chips.css · url.css. 아이콘: 📒 매일 일기 습관 · 👩‍🏫 AI 다홍펜 · ✏️ 맞춤법 공부 · 🌱 성장 기록, 주소는 흰 알약 + 🌐 + iinote(코랄).co.kr). 바꾸면 카카오 공유 디버거(developers.kakao.com/tool/debugger/sharing)에서 캐시 초기화. 카카오 개발자 > 플랫폼 > Web에 https://iinote.co.kr, https://jsun.site 등록 필요
- 아이 일기 규칙: 말로 쓰기(한 번에 10글자 이상 들어오면 되돌림) · 붙여넣기 · 끌어다 놓기 막음, 브라우저 맞춤법 밑줄 끔
- 설정값: FEATURES(기능별 어른만/모두), MAX_PHOTOS=9, MAX_AUDIO_SEC=180, SPELL_API(다홍펜 워커 family-spell.bellachord.workers.dev, 키 없이 { text, level:"gradeN", name } → { errors:[{wrong,right,kind,hint,why}], praise, cheer }), GRADES(profiles.grade 숫자: -2~0 = 5~7세, 1~6 = 초1~6, 7~9 = 중1~3, 10~12 = 고1~3, gradeName · gradeLevel(워커에 age5~7 · grade1~6 · middle1~3 · high1~3)), PEN_GOAL_DEFAULT(5세 10 · 6세 20 · 7세 30 · 초1 40 · 초2 60 · 초3 80 · 초4 100 · 초5 150 · 초6 150 · 중 300 · 고 400 (2026-09-29 낮춤), 부모가 우리 가족 탭에서 아이마다 바꿈 → profiles.diaryGoal), SITE_ADDRESS
- 디자인 4가지 (2026-10-03 노트 디자인 뺌, 원고지와 겹쳐서): warm(따뜻한 일기장, 기본) · clean · bright · wongoji. 기본 글씨는 손글씨(pen). 예전에 note 를 고른 기기는 warm 으로
- 디자인: 색은 모두 :root CSS 변수. 아이보리 #FBF6EE, 네이비 #1F2A44, 코랄 #EF6F53, 버터 #F4BE45, 모눈 배경, 컴퓨터 화면의 노트 여백선
- 글꼴: Pretendard(화면), 일기 글씨 4가지(바탕 · 손글씨 · 고딕 · 동글). 손글씨를 고르면 화면 전체가 손글씨: 제목 · 큰 글자(h1 · h2) = 온글잎 언즈체(2026-10-03 다예쁨체에서 바꿈), 일기 본문 = 온글잎 윤탱체, 작은 글 · 단추 = 카페24 아네모네 에어 (눈누, 셋 다 웹 임베딩 허용 확인). 디자인 제목 글씨 G마켓 산스. 일기 제목 기능은 쓰지 않음(사용자 결정 2026-09-28, 화면에서 모두 뺌)

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
- **약관 · 방침은 기능과 함께 바로 고치기** (사용자 요청 2026-09-28): 다루는 정보, 보내는 곳(외부 서비스), 누가 보는지, 아이 일기 규칙, AI 기능이 바뀌면 같은 커밋에서 privacy.html · terms.html 도 고치고 시행일을 그날로. 원칙: 운영자는 일기 내용을 보지 않고 가족 수 · 일기 편수 · 다홍펜 횟수 같은 숫자만 봄 (admin.html 운영 현황판도 이름 · 내용 없이 숫자만)
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
   - ✅ 예전 가족일기장 가져오기 (우리 가족 탭, 운영자 bellachord 계정일 때만 보임: S.isOperator = 이메일 SHA-256 비교. 다른 가족에게 도윤 · 도진 이름이 보이지 않게): 예전 일기장(jsun.site/diary)과 공부방 아이 일기는 파이어베이스 moon-15f88의 diary/{날짜}_{me|doyun|dojin} 에 있음. OLD_DIARY_CONFIG로 두 번째 앱을 열어 bellachord 계정으로 읽고, 일기 · 댓글을 importKey("old:문서id")와 함께 복사 → 다시 눌러도 새 것만 + 예전 일기장에서 고친 일기(updatedAt > importAt)는 새 내용으로 바꿈. 다홍펜 기록(spellFixes · spellFeedback)은 aiResult로(어떻게 고쳤는지는 explain에), 아이 일기의 고친 낱말은 quizWords에. 하트 · 도장 · 날씨는 안 옮김. moon-15f88 승인된 도메인에 iinote.co.kr 추가 필요
   - ✅ 디자인 4가지 (화면 위 맨 왼쪽 견본 단추, 기기마다 기억 · html[data-design]): note(노트, 기본) · clean(깔끔: 공부방과 같은 흰 카드 · 파랑 · 밑줄 탭) · bright(산뜻: 색 카드 · 주황 · 알약 탭, 4분할 네 칸이 과목 카드처럼 색) · wongoji(시안 20 레드 원고지). 제목 글씨는 G마켓 산스(noonfonts jsdelivr, 공부방과 같은 파일). 시안 모음은 _local/designs, _local/themes, _local/themes2
3. ✅ 다홍펜 연결 (2026-09-28): 새 워커 대신 공부방이 쓰던 family-spell 워커를 그대로 씀 (Claude Opus 5 · effort medium · json_schema · server-side fallback, KV USAGE 로 월별 사용량, GET /usage). 워커 원본은 비공개 저장소 github.com/vaca-at/family-spell (src/index.js, 가족 정보가 들어 있어 꼭 비공개 유지 · 둘째 PC 로컬 Downloadsamily-spell). 고칠 때는 그 저장소에서 고치고 npx wrangler deploy → push, 대시보드 Edit code 로 직접 고치지 않기. wrangler.toml 에 USAGE KV id 까지 들어 있어 어느 PC에서든 clone 후 바로 deploy 가능. 첫 PC의 예전 워커 폴더에서는 절대 deploy 하지 않기 (오늘 고친 내용이 덮어써짐) (iinote.co.kr 요청이면 FAMILY 대신 일반 안내 · 1~6학년 LEVELS · 사용량에 아이 이름 대신 "iinote" · 오류 type 로그). 캐싱은 요청 간격이 5분보다 길어 오히려 손해라 안 넣음. ⚠ 워커의 Access-Control-Allow-Origin 이 https://jsun.site 로 고정이라 클라우드플레어 대시보드에서 https://iinote.co.kr 도 허락하게 고쳐야 실제로 동작함 (워커 코드는 깃허브에 없음). 워커가 가끔 502(안쪽 AI 403)를 내서 앱이 한 번 더 부름. 공부방 일기 단추는 https://iinote.co.kr/?write 로 (들어오면 오늘 일기 쓰는 칸이 바로 열림)
4. ✅ (2026-09-28) Blaze 전환(예산 알림 5,000원) + Storage 버킷 iinote.firebasestorage.app 만듦 + storage.rules 게시 → MEDIA_READY=true 로 사진 · 소리 올리기 열림. (이전 기록:) Blaze 전환 후 Storage 켜고 storage.rules 적용 (사진 · 소리 저장). 2026-09-28 확인: Storage 버킷이 아직 없어서(404) 사진 업로드 실패 → index.html MEDIA_READY=false 로 사진 · 소리 칸을 숨기고 "곧 열려요" 안내. 켜면 MEDIA_READY=true 로 바꾸기. 둘째 PC에 새로 쓴 규칙: _local/storage.rules (가족 구성원만 읽기 · 쓰기, 10MB 이하 image/audio)
5. Capacitor 안드로이드 포장, 앱용 구글 로그인으로 교체 (사용자 계획: 2026년 10월 중 출시 — 첫 화면 · 약관 · 방침에 "10월 중 출시 예정"으로 안내 중)