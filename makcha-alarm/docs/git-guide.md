# VS Code로 Git 쓰는 법 (터미널 명령어 없이 버튼으로)

## A. 팀장: 이 폴더를 GitHub 레포로 올리기 (처음 한 번)
1. zip을 풀고 VS Code → **File > Open Folder** 로 `makcha-alarm` 폴더 열기
2. 왼쪽 **Source Control** 아이콘(가지 모양) 클릭 → **Initialize Repository**
3. 변경 목록 위 메시지 칸에 `첫 커밋: 프로젝트 구조 추가` 입력 → **Commit** (Stage 할지 물으면 Yes)
4. **Publish Branch** 클릭 → GitHub 로그인(Authorize) → 레포 이름 `makcha-alarm`, **private** 선택
5. 올릴 파일 목록에서 `.env` 가 **없는지** 확인 후 확인 (`.env.example` 은 있어도 됩니다)
6. github.com → 레포 → **Settings > Collaborators > Add people** 에서 팀원 초대

## B. 팀원: 레포 가져오기 (처음 한 번)
1. 초대 이메일/GitHub 알림에서 초대 **수락**
2. VS Code **Source Control** → **Clone Repository** → **Clone from GitHub** → `makcha-alarm` 선택 → 저장 폴더 선택 → **Open**

## C. 작업 시작 전 (매번)
1. 왼쪽 아래 브랜치 이름 클릭 → `main` 선택
2. Source Control 탭 `...` 메뉴 → **Pull**
3. 왼쪽 아래 브랜치 클릭 → **Create new branch...** → `feature/기능명` 입력

## D. 작업 후 올리기
1. Source Control 탭에서 변경 파일 확인 (**`.env` 가 보이면 올리지 말 것!**)
2. 파일 옆 **+** (또는 Changes 옆 **+**) 클릭 = Stage
3. 위 메시지 칸에 커밋 메시지 입력 → **Commit**
4. **Publish Branch** (이미 올린 브랜치면 **Sync Changes**)
5. github.com → **Compare & pull request** → **Create pull request**
6. 팀원 1명 이상 확인 후 **Merge pull request**

## E. 충돌 났을 때
파일에 `Accept Current Change / Accept Incoming Change / Accept Both Changes` 버튼이 뜨면
내용을 읽어보고 맞는 걸 클릭 → 저장 → Stage → Commit.
