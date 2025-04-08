## Visual Studio, 콘솔기반 프로젝트

### Nuget에서 raylib 설치
- 프로젝트>Nuget패키지관리>찿아보기>raylib 

## 창을 하나로 만드려면
1. 프로젝트 > 속성> 링커 > 시스템 > 하위 시스템 Windows (/SUBSYSTEM:WINDOWS)
2. 프로젝트 > 속성> 링커 > 고급 > 엔트리 지점 mainCRTStartup
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
