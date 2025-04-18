```
#include "raylib.h"
#include "rlgl.h" 

int main(void)
{
    const int screenWidth = 800;
    const int screenHeight = 600;

    InitWindow(screenWidth, screenHeight, "Cube");

    Camera3D camera = { 0 };
    camera.position = (Vector3){ 4.0f, 4.0f, 4.0f };
    camera.target = (Vector3){ 0.0f, 0.0f, 0.0f };
    camera.up = (Vector3){ 0.0f, 1.0f, 0.0f };
    camera.fovy = 45.0f;
    camera.projection = CAMERA_PERSPECTIVE;

    SetTargetFPS(60);

    float rotation = 0.0f;

    while (!WindowShouldClose())
    {
        rotation += 0.5f; // 매 프레임마다 0.5도 회전

        BeginDrawing();
        ClearBackground(RAYWHITE);

        BeginMode3D(camera);

        // 육면체(큐브) 그리기
        rlPushMatrix();
        rlTranslatef(0, 0, 0);
        rlRotatef(rotation, 1, 1, 0); // X, Y축 둘 다 회전
        DrawCube((Vector3) { 0, 0, 0 }, 2.0f, 2.0f, 2.0f, SKYBLUE); // 큐브
        DrawCubeWires((Vector3) { 0, 0, 0 }, 2.0f, 2.0f, 2.0f, DARKBLUE); // 큐브 테두리
        rlPopMatrix();

        EndMode3D();

        DrawText("Rotate Cube", 10, 10, 20, DARKGRAY);

        EndDrawing();
    }

    CloseWindow();

    return 0;
}
```
