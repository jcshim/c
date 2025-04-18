```
#include "raylib.h"
#include "rlgl.h" 

int main(void)
{
    const int screenWidth = 800;
    const int screenHeight = 600;

    InitWindow(screenWidth, screenHeight, "Tri");

    SetTargetFPS(60);

    while (!WindowShouldClose())
    {
        BeginDrawing();
        ClearBackground(RAYWHITE);

        // 삼각형 꼭짓점 정의
        Vector2 v1 = { screenWidth / 2, 100 };
        Vector2 v2 = { 200, 500 };
        Vector2 v3 = { 600, 500 };

        // 직접 그리기 (색깔 지정해서)
        rlBegin(RL_TRIANGLES);

        rlColor3f(1.0f, 0.0f, 0.0f); // 빨강
        rlVertex2f(v1.x, v1.y);

        rlColor3f(0.0f, 1.0f, 0.0f); // 초록
        rlVertex2f(v2.x, v2.y);

        rlColor3f(0.0f, 0.0f, 1.0f); // 파랑
        rlVertex2f(v3.x, v3.y);

        rlEnd();

        EndDrawing();
    }

    CloseWindow();

    return 0;
}
```
