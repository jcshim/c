## raylib에서 한글 출력 
1. nuget에서 raylib 설치
2. 소스코드 넣기
```
#include "raylib.h"

int main() {
    InitWindow(800, 450, "한글 출력");
    Font font = LoadFontEx("C:\\Windows\\Fonts\\malgun.ttf", 30, NULL, 65535);
    const char* text = "\xEC\x95\x88\xEB\x85\x95\xED\x95\x98\xEC\x84\xB8\xEC\x9A\x94"; // "안녕하세요"
    while (!WindowShouldClose()) {
        BeginDrawing();
        ClearBackground(RAYWHITE);
        DrawTextEx(font, text, (Vector2) { 250, 200 }, 30, 2, BLACK);
        EndDrawing();
    }
    UnloadFont(font);
    CloseWindow();
    return 0;
}
```
또는
```
#include "raylib.h"

int main() {
    InitWindow(800, 450, "한글 출력");

    // 한글 지원 폰트 로드 (30pt, 전체 코드포인트 지원)
    Font font = LoadFontEx("C:\\Windows\\Fonts\\malgun.ttf", 30, NULL, 65535);

    // ✅ 일반 한글 문자열 그대로 사용 (단, UTF-8 with BOM + u8 붙이기 필요)
    const char* text = u8"안녕하세요";

    while (!WindowShouldClose()) {
        BeginDrawing();
        ClearBackground(RAYWHITE);
        DrawTextEx(font, text, (Vector2) { 250, 200 }, 30, 2, BLACK);
        EndDrawing();
    }

    UnloadFont(font);
    CloseWindow();
    return 0;
}
```
3. unicode로 저장: 다른 이름으로 저장> 저장옆의 역삼각형> 인코딩하여 저장 >유니코드(서명 없는 UTF-8)  65001 (아래 부분에 숨어 있어요)
줄끝: 윈도우 CR LF
속성 > 구성속성 > 고급 > 문자집합 > 유니코드문자집합 사용
4. 실행

## 문장 "국립경국대학교" 프람프트 만들기
프람프트: 다음 "안녕하세요"를 참고해서 "국립경국대학교" UTF-8 인코딩 만들어줘
const char* text = "\xEC\x95\x88\xEB\x85\x95\xED\x95\x98\xEC\x84\xB8\xEC\x9A\x94"; // UTF-8 "안녕하세요"
