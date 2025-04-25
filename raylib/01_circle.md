Visual Stduio
### Nuget에서 raylib 설치
- 프로젝트>Nuget패키지관리>찾아보기>raylib  설치 

```source code
#include "raylib.h"
int main() {
	InitWindow(320, 240, "Hi");
	while (!WindowShouldClose()) {
		BeginDrawing();
		DrawEllipse(320/2, 240/2, 80, 80, BLUE);
		EndDrawing();
	}
}
```
