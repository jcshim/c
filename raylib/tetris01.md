#include "raylib.h"

#define SCREEN_WIDTH  400
#define SCREEN_HEIGHT 600
#define BLOCK_SIZE    30
#define FALL_SPEED    10  // 낮을수록 빨리 떨어짐

typedef struct {
    int x, y;
} Block;

int main(void) {
    InitWindow(SCREEN_WIDTH, SCREEN_HEIGHT, "Mini Tetris");
    SetTargetFPS(60);

    Block block = { SCREEN_WIDTH / 2 / BLOCK_SIZE, 0 };  // 블록 시작 위치 (가운데)
    int fallCounter = 0;

    while (!WindowShouldClose()) {
        // 이동 입력
        if (IsKeyPressed(KEY_LEFT))  block.x--;
        if (IsKeyPressed(KEY_RIGHT)) block.x++;
        
        // 블록 떨어뜨리기
        fallCounter++;
        if (fallCounter >= FALL_SPEED) {
            block.y++;
            fallCounter = 0;
        }

        // 화면 바닥에 도달하면 초기화 (간단 구현)
        if ((block.y + 1) * BLOCK_SIZE >= SCREEN_HEIGHT) {
            block.x = SCREEN_WIDTH / 2 / BLOCK_SIZE;
            block.y = 0;
        }

        // 화면 그리기
        BeginDrawing();
        ClearBackground(RAYWHITE);

        DrawRectangle(block.x * BLOCK_SIZE, block.y * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE, BLUE);

        EndDrawing();
    }

    CloseWindow();
    return 0;
}
