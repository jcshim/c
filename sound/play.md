```
#include <windows.h>
#include <stdio.h>
// winmm.lib 라이브러리를 링크합니다. (Visual Studio 전용)
#pragma comment(lib, "winmm.lib")
void main() {
    // 1. 파일 열기
    mciSendString(L"open littlestar.mp3 type mpegvideo alias s", NULL, 0, NULL);
    // 2. 재생
    mciSendString(L"play s", NULL, 0, NULL);

    printf("재생 중... 엔터를 누르면 종료합니다.\n");
    char ch = getchar(); // 재생되는 동안 프로그램이 종료되지 않게 대기
}
```
