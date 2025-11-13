
# 🎨 Visual Studio + C 언어 + raylib

# **2시간 실습용 프로젝트 안내서 (최종 수정판)**

프로젝트: **Mini 2D Game — C로 만드는 움직이는 플레이어 + 아이템 줍기**

---

# 📦 0. 실습 환경 구성 (15분)

## ✔ 1) Visual Studio 프로젝트 생성

1. Visual Studio 실행
2. **새 프로젝트 만들기 → “빈 프로젝트(Empty Project)”**
3. 언어: **C++**지만, **C 코드도 완전히 호환**됨
4. 프로젝트 이름: `RaylibCPractice`

> Visual Studio는 C 전용 프로젝트 템플릿이 없기 때문에
> C 파일을 추가하면 C 컴파일 규칙이 자동 적용됩니다.

---

## ✔ 2) NuGet으로 raylib 설치

메뉴:

```
도구 → NuGet 패키지 관리자 → 솔루션용 NuGet 패키지 관리자
```

검색창에 입력:

```
raylib-native
```

또는 시스템에 맞는 패키지:

* `raylib-native`
* `raylibpm` (일부 구성)
* `raylib-vcpkg` (NuGet wrapper)

설치하면 다음이 자동으로 프로젝트에 포함됩니다:

* **raylib.h**
* **raymath.h**
* **libraylib.a / raylib.lib**
* **raylib.dll** (런타임)
* include/library 경로 자동 연결

즉, 추가 설정 없이 바로 C 코드에서 `#include "raylib.h"` 사용 가능.

---

## ✔ 3) C 파일 추가

1. `Source Files` 우클릭
2. **Add → New Item → C File (.c)**
3. 이름: `main.c`

---

# 🧭 1단계: 기본 창 띄우기 (20분)

### ✨ main.c (단계 1)

```c
#include "raylib.h"

int main(void) {
    InitWindow(800, 450, "raylib C practice");
    SetTargetFPS(60);

    while (!WindowShouldClose()) {
        BeginDrawing();
        ClearBackground(RAYWHITE);

        DrawText("Hello, raylib (C)!", 20, 20, 24, BLACK);

        EndDrawing();
    }

    CloseWindow();
    return 0;
}
```

### ✔ 빌드 에러 없이 실행되면 환경 구성 완료!

---

# 🧭 2단계: 플레이어 박스 + 이동 (30분)

```c
Rectangle player = { 400, 225, 40, 40 };
float speed = 4.0f;

if (IsKeyDown(KEY_RIGHT)) player.x += speed;
if (IsKeyDown(KEY_LEFT))  player.x -= speed;
if (IsKeyDown(KEY_UP))    player.y -= speed;
if (IsKeyDown(KEY_DOWN))  player.y += speed;
```

그리기:

```c
DrawRectangleRec(player, BLUE);
```

---

# 🧭 3단계: 아이템 + 충돌 판정 (20분)

```c
Rectangle item = { 200, 100, 20, 20 };
bool itemAlive = true;

if (itemAlive && CheckCollisionRecs(player, item)) {
    itemAlive = false;
}
```

그리기:

```c
if (itemAlive)
    DrawRectangleRec(item, RED);
```

---

# 🧭 4단계: 점수 & UI (20분)

```c
int score = 0;

if (itemAlive && CheckCollisionRecs(player, item)) {
    itemAlive = false;
    score++;
}
```

UI:

```c
DrawText(TextFormat("Score: %d", score), 20, 20, 24, DARKGRAY);
```

---

# 🏁 5단계: 최종 완성 코드

```c
#include "raylib.h"

int main(void) {
    InitWindow(800, 450, "Mini Game - raylib & C");
    SetTargetFPS(60);

    Rectangle player = { 400, 225, 40, 40 };
    float speed = 4.0f;

    Rectangle item = { 200, 100, 20, 20 };
    bool itemAlive = true;

    int score = 0;

    while (!WindowShouldClose()) {
        // ------ Update ------
        if (IsKeyDown(KEY_RIGHT)) player.x += speed;
        if (IsKeyDown(KEY_LEFT))  player.x -= speed;
        if (IsKeyDown(KEY_UP))    player.y -= speed;
        if (IsKeyDown(KEY_DOWN))  player.y += speed;

        if (itemAlive && CheckCollisionRecs(player, item)) {
            itemAlive = false;
            score++;
        }

        // ------ Draw ------
        BeginDrawing();
        ClearBackground(RAYWHITE);

        DrawRectangleRec(player, BLUE);

        if (itemAlive)
            DrawRectangleRec(item, RED);

        DrawText(TextFormat("Score: %d", score), 20, 20, 24, DARKGRAY);

        EndDrawing();
    }

    CloseWindow();
    return 0;
}
```

---

# 📘 6. 확장 과제 (심재창 교수님 수업용)

### Level 1 — 난도 낮음

* 화면 경계를 벗어나지 않도록 제한
* 아이템 위치 랜덤 재배치

### Level 2 — 중간 난도

* 적(enemy) 추가
* 적이 플레이어를 따라오도록 구현

### Level 3 — 고급

* 이미지(텍스처)로 캐릭터 표현
* 사운드 효과 추가

### Level 4 — 도전

* 타일맵 기반 2D 스테이지 구성
* 작은 슈팅 게임 제작

---

```
// main.c
#include "raylib.h"

int main(void)
{
    // ---------------- 윈도우 초기화 ----------------
    const int screenWidth = 800;
    const int screenHeight = 450;

    InitWindow(screenWidth, screenHeight, "C + raylib - Move & Pick Item");
    SetTargetFPS(60);   // 60 FPS

    // ---------------- 게임 오브젝트 ----------------
    // 플레이어 (파란 사각형)
    Rectangle player = {
        screenWidth / 2.0f - 20.0f,  // x
        screenHeight / 2.0f - 20.0f, // y
        40.0f,                       // width
        40.0f                        // height
    };
    float playerSpeed = 4.0f;

    // 아이템 (빨간 사각형)
    Rectangle item = {
        200.0f, 100.0f, // x, y
        20.0f, 20.0f    // width, height
    };

    int score = 0;

    // ---------------- 메인 루프 ----------------
    while (!WindowShouldClose())   // ESC 또는 창 닫기 버튼
    {
        // ====== Update (게임 로직) ======
        // 플레이어 이동
        if (IsKeyDown(KEY_RIGHT)) player.x += playerSpeed;
        if (IsKeyDown(KEY_LEFT))  player.x -= playerSpeed;
        if (IsKeyDown(KEY_UP))    player.y -= playerSpeed;
        if (IsKeyDown(KEY_DOWN))  player.y += playerSpeed;

        // 화면 밖으로 못 나가게 제한
        if (player.x < 0) player.x = 0;
        if (player.y < 0) player.y = 0;
        if (player.x + player.width > screenWidth)
            player.x = screenWidth - player.width;
        if (player.y + player.height > screenHeight)
            player.y = screenHeight - player.height;

        // 플레이어가 아이템에 닿았는지 체크
        if (CheckCollisionRecs(player, item)) {
            score++;

            // 아이템을 새 위치로 랜덤 재배치
            item.x = (float)GetRandomValue(0, screenWidth - (int)item.width);
            item.y = (float)GetRandomValue(0, screenHeight - (int)item.height);
        }

        // ====== Draw (그리기) ======
        BeginDrawing();
        ClearBackground(RAYWHITE);

        // 플레이어
        DrawRectangleRec(player, BLUE);

        // 아이템
        DrawRectangleRec(item, RED);

        // 점수 표시
        DrawText(TextFormat("Score: %d", score), 20, 20, 30, DARKGRAY);

        // 간단한 안내 텍스트
        DrawText("Use arrow keys to move", 20, 60, 20, GRAY);
        DrawText("Touch the red box to earn score!", 20, 85, 20, GRAY);
        DrawText("Press ESC to exit", 20, 110, 20, GRAY);

        EndDrawing();
    }

    // ---------------- 종료 처리 ----------------
    CloseWindow();
    return 0;
}
```
