## Visual Studio, 콘솔기반 프로젝트

### Nuget에서 raylib 설치
- 프로젝트>Nuget패키지관리>찿아보기>raylib 

## 창을 하나로 만드려면
1. 프로젝트 > 속성> 링커 > 시스템 > 하위 시스템 Windows (/SUBSYSTEM:WINDOWS)
2. 프로젝트 > 속성> 링커 > 고급 > 진입점(엔트리 지점) mainCRTStartup
* mainCRTStartup을 엔트리 포인트로 지정하면서 하위시스템을 Windows로 두면, 콘솔 창이 생성되지 않고 프로그램이 백그라운드에서 실행된다.
```
#include "raylib.h"

int main(void)
{
    InitWindow(800, 600, "GKNU");
    while (!WindowShouldClose())  // ESC 또는 창 닫힘 이벤트 감지
    {
        BeginDrawing();
        ClearBackground(RAYWHITE);      // 배경을 흰색으로 초기화 (없으면 깜빡일 수 있음)
        DrawCircle(400, 300, 150, GREEN);  // 초록 원 (x=400, y=300, 반지름=150)
        EndDrawing();
    }
    CloseWindow();  // 창 닫기
}
```
```
#include "raylib.h"
int main() {
    InitWindow(480, 320, "GKNU");
    while (!WindowShouldClose()) {
        BeginDrawing();
        ClearBackground(RAYWHITE);
        DrawRectangle(10, 10, 120, 40, (Color) { 0, 255, 0, 128 });
        DrawText("It works!", 20, 20, 20, BLACK);
        EndDrawing();
    }
    CloseWindow();
}
```
### 위에서 아래로 내려오는 공
```
#include "raylib.h"

int main(void)
{
    InitWindow(800, 600, "GKNU");
    int i = 0;
    SetTargetFPS(60);
    while (!WindowShouldClose())  // ESC 또는 창 닫힘 이벤트 감지
    {
        BeginDrawing();
        ClearBackground(RAYWHITE);      // 배경을 흰색으로 초기화 (없으면 깜빡일 수 있음)

        DrawCircle(400, i++, 150, GREEN);  // 초록 원 (x=400, y=300, 반지름=150)
        if (i >= 600) i = 0;
        EndDrawing();
    }

    CloseWindow();  // 창 닫기
}
```
### 무지게 블록깨기
```
#include "raylib.h"

#define MAX_BLOCKS 36

int main() {
    InitWindow(800, 600, "Block");
    SetTargetFPS(60);

    // 패들과 공
    Rectangle paddle = { 350, 550, 100, 20 };
    Vector2 ball = { 400, 300 };
    Vector2 speed = { 5, -5 };
    float radius = 10;

    // 블록들
    Rectangle blocks[MAX_BLOCKS];
    Color colors[6] = { RED, ORANGE, YELLOW, GREEN, BLUE, PURPLE };
    bool active[MAX_BLOCKS] = { 0 };
    for (int i = 0; i < MAX_BLOCKS; i++) {
        blocks[i] = (Rectangle){ 20 + (i % 12) * 63, 50 + (i / 12) * 30, 60, 20 };
        active[i] = true;
    }

    while (!WindowShouldClose()) {
        // 패들 이동
        if (IsKeyDown(KEY_LEFT)) paddle.x -= 8;
        if (IsKeyDown(KEY_RIGHT)) paddle.x += 8;

        // 공 이동
        ball.x += speed.x;
        ball.y += speed.y;

        // 벽 충돌
        if (ball.x < radius || ball.x > 800 - radius) speed.x *= -1;
        if (ball.y < radius) speed.y *= -1;

        // 바닥
        if (ball.y > 600) ball = (Vector2){ 400, 300 };

        // 패들 충돌
        if (CheckCollisionCircleRec(ball, radius, paddle)) speed.y *= -1;

        // 블록 충돌
        for (int i = 0; i < MAX_BLOCKS; i++) {
            if (active[i] && CheckCollisionCircleRec(ball, radius, blocks[i])) {
                active[i] = false;
                speed.y *= -1;
                break;
            }
        }

        // 그리기
        BeginDrawing();
        ClearBackground(BLACK);
        DrawRectangleRec(paddle, WHITE);
        DrawCircleV(ball, radius, WHITE);
        for (int i = 0; i < MAX_BLOCKS; i++)
            if (active[i]) DrawRectangleRec(blocks[i], colors[i / 12]);
        EndDrawing();
    }

    CloseWindow();
    return 0;
}
```
### 1인 탁구
```
#include "raylib.h"

int main() {
    InitWindow(800, 600, "탁구 게임");
    SetTargetFPS(60);

    // 패들
    Rectangle paddle = { 20, 250, 20, 100 };

    // 공
    Vector2 ball = { 400, 300 };
    Vector2 speed = { 5, 5 };
    float radius = 10;

    int score = 0;

    while (!WindowShouldClose()) {
        // 패들 이동
        if (IsKeyDown(KEY_UP) && paddle.y > 0) paddle.y -= 6;
        if (IsKeyDown(KEY_DOWN) && paddle.y < 600 - paddle.height) paddle.y += 6;

        // 공 이동
        ball.x += speed.x;
        ball.y += speed.y;

        // 위아래 벽 반사
        if (ball.y < radius || ball.y > 600 - radius) speed.y *= -1;

        // 패들 충돌
        if (CheckCollisionCircleRec(ball, radius, paddle)) {
            speed.x *= -1;
            ball.x = paddle.x + paddle.width + radius; // 튕긴 위치 조정
            score++;
        }

        // 오른쪽 벽 반사
        if (ball.x > 800 - radius) speed.x *= -1;

        // 게임 오버 (왼쪽 벽 통과)
        if (ball.x < 0) {
            score = 0;
            ball = (Vector2){ 400, 300 };
            speed = (Vector2){ 5, 5 };
        }

        // 그리기
        BeginDrawing();
        ClearBackground(BLACK);

        DrawText(TextFormat("Score: %d", score), 650, 20, 20, WHITE);
        DrawRectangleRec(paddle, WHITE);
        DrawCircleV(ball, radius, RED);

        EndDrawing();
    }

    CloseWindow();
    return 0;
}
```

