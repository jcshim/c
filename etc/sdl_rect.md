### ✅ SDL2를 NuGet으로 설치하는 가장 간단한 방법

#### 1. 프로젝트 생성
- Visual Studio에서 **C 또는 C++ 콘솔 애플리케이션** 프로젝트 생성  
  (예: `rect`)
---
#### 2. NuGet 패키지 설치
1. **솔루션 탐색기**에서 프로젝트를 우클릭 → `NuGet 패키지 관리`
2. **[찾아보기] 탭에서 "SDL2"** 검색
3. 아래 패키지 중 하나 선택 후 설치:
   - `SDL2` by `flibitijibibo` (많이 사용됨)
   - 또는 `SDL2-static` (정적 링크를 원할 경우)
✅ 설치되면 `SDL2.dll`, `include`, `lib` 등이 자동으로 구성됩니다.
---
### ✅ 창을 하나로 만드려면
1. 프로젝트 > 속성> 링커 > 시스템 > 하위 시스템	Windows (/SUBSYSTEM:WINDOWS)
2. 프로젝트 > 속성> 링커 > 고급 > 엔트리 지점	mainCRTStartup

#### 3. 코드에 추가
```
#include <SDL.h>

int main() {
    SDL_Window* w = SDL_CreateWindow("Box", 100, 100, 640, 480, 0);
    SDL_Renderer* r = SDL_CreateRenderer(w, -1, 0);

    SDL_SetRenderDrawColor(r, 0, 255, 0, 255);
    SDL_RenderFillRect(r, &(SDL_Rect){220, 165, 200, 150});

    SDL_RenderPresent(r);

    SDL_Event e;
    while (SDL_WaitEvent(&e)) {
        if (e.type == SDL_QUIT) break;
    }

    SDL_DestroyRenderer(r);
    SDL_DestroyWindow(w);
    SDL_Quit();
    return 0;
}
```
```
#define SDL_MAIN_HANDLED
#include <SDL.h>

int main() {
    SDL_Init(SDL_INIT_VIDEO);
    SDL_Window* w = SDL_CreateWindow("Green", 100, 100, 640, 480, 0);
    SDL_Renderer* r = SDL_CreateRenderer(w, -1, 0);

    SDL_SetRenderDrawColor(r, 196, 196, 196, 255);
    SDL_RenderClear(r);
    SDL_Rect rect = { 220, 140, 200, 150 };
    SDL_SetRenderDrawColor(r, 0, 255, 0, 255);
    SDL_RenderFillRect(r, &rect);
    SDL_RenderPresent(r);

    SDL_Event e;
    while (SDL_WaitEvent(&e) && e.type != SDL_QUIT);

    SDL_DestroyRenderer(r);
    SDL_DestroyWindow(w);
    SDL_Quit();
    return 0;
}
```

```
#define SDL_MAIN_HANDLED
#include <SDL.h>

int main() {
    SDL_Init(SDL_INIT_VIDEO);
    SDL_Window* w = SDL_CreateWindow("Drop", 100, 100, 800, 600, 0);
    SDL_Renderer* r = SDL_CreateRenderer(w, -1, 0);

    int y = 0;

    while (1) {
        SDL_Event e;
        if (SDL_PollEvent(&e) && e.type == SDL_QUIT) break;

        y += 2;
        if (y > 600) y = 0;

        SDL_SetRenderDrawColor(r, 0, 0, 0, 255); SDL_RenderClear(r);
        SDL_SetRenderDrawColor(r, 0, 255, 0, 255);
        SDL_Rect rect = { 350, y, 100, 100 };
        SDL_RenderFillRect(r, &rect);
        SDL_RenderPresent(r);

        SDL_Delay(16);
    }

    SDL_DestroyRenderer(r); SDL_DestroyWindow(w); SDL_Quit();
    return 0;
}
```
