# struct in raylib.h

### 1. `Vector2`

* **설명**: 2차원 공간에서의 위치, 방향, 속도 등을 표현할 때 사용.
* **정의**:

  ```c
  typedef struct Vector2 {
      float x;
      float y;
  } Vector2;
  ```
* **예제**:

  ```c
  Vector2 position = { 100.0f, 200.0f };
  DrawCircleV(position, 30, RED);  // 위치를 Vector2로 전달
  ```

---

### 2. `Rectangle`

* **설명**: 사각형 영역(충돌 판정, 버튼 영역 등)을 정의할 때 사용.
* **정의**:

  ```c
  typedef struct Rectangle {
      float x;
      float y;
      float width;
      float height;
  } Rectangle;
  ```
* **예제**:

  ```c
  Rectangle box = { 50, 50, 100, 40 };
  DrawRectangleRec(box, BLUE);
  ```

---

### 3. `Color`

* **설명**: 색상을 정의하는 구조체. RGBA로 표현.
* **정의**:

  ```c
  typedef struct Color {
      unsigned char r;
      unsigned char g;
      unsigned char b;
      unsigned char a;
  } Color;
  ```
* **예제**:

  ```c
  Color myColor = { 255, 100, 0, 255 };
  DrawRectangle(10, 10, 100, 100, myColor);
  ```

---

이 세 구조체는 **화면에 무언가를 그릴 때 가장 자주 쓰이며**, Raylib의 대부분 함수에서 인자로 사용됩니다.
