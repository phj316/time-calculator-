# 막차연산알림 (Last Train Backward Calculator)

술자리 위치에서 집까지 가기 위해 **"몇 시 몇 분에 일어나야 하는지"**를 막차 기준으로 거꾸로 계산해 주는 귀가 도우미입니다.
자세한 기능은 [SPEC.md](SPEC.md)를 읽어주세요.

## 팀원 처음 시작하기

1. Python 3.10 이상 설치 (python.org)
2. VS Code에서 이 레포를 Clone
3. VS Code 상단 메뉴 `Terminal > New Terminal` 열고 아래 실행

```
python -m venv venv
venv\Scripts\activate        # Mac은: source venv/bin/activate
pip install -r requirements.txt
```

4. `.env.example` 파일을 복사해서 `.env` 로 이름 바꾸고 ODsay API 키 입력 (`.env`는 절대 커밋 금지)
5. 서버 실행

```
uvicorn app.main:app --reload
```

브라우저에서 http://127.0.0.1:8000/docs 를 열면 API 테스트 화면이 나옵니다.

6. 테스트 실행

```
pytest
```

## 폴더 설명

| 폴더 | 역할 |
|------|------|
| `app/core/` | 핵심 계산 로직 (외부 API 없이 동작하는 순수 함수) |
| `app/services/` | ODsay API 호출, 그룹 정렬 등 외부 연동 |
| `app/models/` | 데이터 구조 정의 (SPEC 3.2 출력 JSON과 동일) |
| `app/routers/` | API 엔드포인트 |
| `tests/` | 테스트 코드 |
| `docs/` | 팀 규칙, API 분석 메모 |
| `frontend/` | 화면 (추후) |

## 팀 규칙

- `main` 브랜치에 직접 작업하지 않기. `feature/기능명` 브랜치에서 작업 후 Pull Request
- 작업 시작 전에 항상 Pull
- 자세한 방법은 [docs/git-guide.md](docs/git-guide.md)
