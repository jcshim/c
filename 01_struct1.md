## ✅ 예제 1: 구조체 이름이 있는 경우 (`struct Vector2`)

```c
#include <stdio.h>

typedef struct Vector2 {
    float x, y;
} Vector2;

int main() {
    // typedef 이름 사용
    Vector2 a = {1.0f, 2.0f};

    // struct 이름 사용도 가능
    struct Vector2 b = {3.0f, 4.0f};

    printf("a: %.1f, %.1f\n", a.x, a.y);
    printf("b: %.1f, %.1f\n", b.x, b.y);
}
```

### 🔍 결과

```
a: 1.0, 2.0
b: 3.0, 4.0
```

> `Vector2`와 `struct Vector2` 둘 다 사용 가능합니다.

---

## ✅ 예제 2: 구조체 이름이 없는 경우 (익명 구조체)

```c
#include <stdio.h>

typedef struct {
    float x, y;
} Vector2;

int main() {
    Vector2 a = {1.0f, 2.0f};

    // struct Vector2 b = {3.0f, 4.0f}; // ❌ 오류! 이름이 없음

    printf("a: %.1f, %.1f\n", a.x, a.y);
}
```

### 🔍 결과

```
a: 1.0, 2.0
```

> `Vector2`만 사용할 수 있고, `struct Vector2`는 **존재하지 않으므로 오류**가 납니다.

---

## ✅ 결론 요약

| 항목     | 구조체 이름 O (`struct Vector2`) | 구조체 이름 X (익명 구조체) |
| ------ | --------------------------- | ----------------- |
| 타입 이름  | `Vector2`, `struct Vector2` | `Vector2`만 가능     |
| 확장성    | 구조체 포인터나 다른 파일과 연동 시 유리     | 단순한 코드에 적합        |
| 오류 가능성 | 적음                          | 이름 혼동 시 오류 가능     |

---

# 구조체 멤버 더하기
```
#include <stdio.h>

typedef struct Vector2{
    float x, y;
} Vector2;

Vector2 Add(Vector2 a, Vector2 b) {
    Vector2 c;
    c.x = a.x + b.x;
    c.y = a.y + b.y;
    return c;
}
int main() {
    Vector2 v1 = { 1, 1 };
    Vector2 v2 = { 2, 2 };
    Vector2 v3;
    v3 = Add(v1, v2);  // 예시로 호출
    printf("%.0f, %.0f\n", v3.x, v3.y);
}
```
