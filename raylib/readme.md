
## [**raylib** 사각형 그리기](https://youtu.be/PVPMa2k4-SM?si=vwvUTGG5WdYYSQJq)
### C로 만든 **초보자 친화적인 그래픽스 및 게임 개발 라이브러리**
### 간단한 2D/3D 렌더링, 사운드, 입력 처리 

---

## 🎯 raylib 한눈에 보기

| 항목 | 내용 |
|------|------|
| 개발 언어 | C (다른 언어 바인딩도 많음: Python, C++, Go 등) |
| 특징 | 쉽고 직관적, 학습에 최적화, 그래픽스/사운드/물리/입력 지원 |
| 플랫폼 | Windows, Linux, macOS, WebAssembly (HTML5), Android |
| 공식 사이트 | [https://www.raylib.com](https://www.raylib.com) |

---

## 📚 raylib 학습 순서

### 1️⃣ 기본 설치 및 빌드
- Windows: `MSYS2` 또는 `vcpkg`, `raylib installer` 사용
- macOS: `brew install raylib`
- Linux: `apt install libraylib-dev` 또는 빌드
- Visual Studio / VSCode에서도 사용 가능

> 🔹 Hello World 수준으로 창 띄우고 도형 그리기부터 시작

---

### 2️⃣ 기본 렌더링 이해
- `InitWindow()`, `BeginDrawing()`, `DrawXXX()`, `EndDrawing()` 반복 구조
- 주요 함수들:
  - `DrawRectangle`, `DrawCircle`, `DrawText`, `DrawLine`, ...
  - `ClearBackground(Color)` → 배경 색 지우기
- Color 상수: `RED`, `GREEN`, `RAYWHITE`, `BLACK` 등

---

### 3️⃣ 입력 처리
- `IsKeyDown(KEY_RIGHT)` 등으로 키보드 입력
- `GetMousePosition()`, `IsMouseButtonPressed(MOUSE_LEFT_BUTTON)` 등으로 마우스 입력

---

### 4️⃣ 애니메이션과 게임 루프
- 위치 값 갱신 → 화면에 재그리기 → 부드러운 움직임
- `SetTargetFPS(60)`으로 프레임 고정

---

### 5️⃣ 텍스처, 이미지, 사운드
- 이미지: `LoadTexture()`, `DrawTexture()`
- 사운드: `InitAudioDevice()`, `LoadSound()`, `PlaySound()`
- 폰트: `LoadFont()`, `DrawTextEx()`

---

### 6️⃣ 고급 주제 (선택적)
- 3D 렌더링: `Camera`, `Model`, `Mesh` 등
- GUI 개발: `raygui` 라이브러리 사용 가능
- 충돌 감지, 물리: `CheckCollisionRects`, `CheckCollisionCircleRec`

---

## 🧰 추천 자료

### 🔗 공식 튜토리얼 및 문서
- [https://www.raylib.com/examples.html](https://www.raylib.com/examples.html)  
  → 다양한 예제가 GUI로 정리되어 있음 (2D, 3D, 사운드 등)

### 📘 raylib cheatsheet
- [https://github.com/raysan5/raylib-cheatsheet](https://github.com/raysan5/raylib-cheatsheet)  
  → 자주 쓰는 함수 요약표

### 📦 깃허브 예제 모음
- [https://github.com/raysan5/raylib](https://github.com/raysan5/raylib)  
  → 공식 소스코드 및 모든 샘플 포함

---

## 🚀 첫 목표 예제 추천

| 주제 | 예제 |
|------|------|
| 1 | 움직이는 원 (키보드 방향키) |
| 2 | 마우스 클릭 시 원 색상 바꾸기 |
| 3 | 텍스트 출력 및 FPS 표시 |
| 4 | 충돌 감지 (박스 안에 마우스 들어오면 색 바뀜) |
| 5 | 간단한 Pong 게임 만들기 🎾 |

---

## ✨ 왜 raylib이 좋은가요?

- 🎓 **교육 친화적** – 함수 이름 직관적, 구조 단순
- 🧼 **불필요한 설정 없음** – 곧바로 창 열고 그리기 가능
- 💡 **실험하기 쉬움** – 바로바로 수정 & 실행
- 🌐 **웹, 모바일도 빌드 가능** – WebAssembly로 쉽게 배포
