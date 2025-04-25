```
#include "raylib.h"
int main() {
	int static h;
	InitWindow(320, 240, "Hi");
	SetTargetFPS(30);
	while (!WindowShouldClose()) {
		BeginDrawing();
		ClearBackground(BLACK);
		DrawEllipse(160, h++, 80, 80, BLUE);
		if (h > 240) h = 0;
		EndDrawing();
	}
}
```
