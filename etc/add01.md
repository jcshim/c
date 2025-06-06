## 두 수를 더하기 퀴즈 문항
1. 키보드로 두 정수를 입력받아, 그 합을 화면에 출력하시오.
2. 두 정수를 배열에 저장하고, 각각 2와 3으로 초기화한 후, 그 합을 화면에 출력하시오.
3. 두 정수를 변수에 저장한 뒤, 함수를 통해 전달하고, 함수에서 더한 결과를 반환받아 화면에 출력하시오.
4. 두 정수를 구조체의 멤버로 저장하고, 이 값을 더한 결과를 화면에 출력하시오.
5. 텍스트 파일 my.txt의 첫 줄과 둘째 줄에 저장된 정수(2와 3)를 읽어, 그 합을 화면에 출력하시오.

---
## 퀴즈 답


### ✅ 1. 키보드로 두 정수를 입력받아 합 출력
```c
#include <stdio.h>
int main() {
    int a, b;
    scanf("%d%d", &a, &b);
    printf("%d", a + b);
}
```

---

### ✅ 2. 배열에 2와 3을 저장하고 합 출력
```c
#include <stdio.h>
int main() {
    int a[2] = {2, 3};
    printf("%d", a[0] + a[1]);
}
```

---

### ✅ 3. 함수를 통해 두 수를 더하고 출력
```c
#include <stdio.h>
int f(int x, int y) { return x + y; }
int main() {
    int a = 2, b = 3;
    printf("%d", f(a, b));
}
```

---

### ✅ 4. 구조체 멤버에 저장하고 합 출력
```c
#include <stdio.h>
struct S { int x, y; };
int main() {
    struct S s = {2, 3};
    printf("%d", s.x + s.y);
}
```

---

### ✅ 5. 파일 my.txt에서 정수 읽고 합 출력
> `my.txt`에는 아래처럼 저장되어 있어야 합니다.  
> ```
> 2
> 3
> ```

```c
#include <stdio.h>
int main() {
    int a, b;
    FILE *f = fopen("my.txt", "r");
    fscanf(f, "%d%d", &a, &b);
    fclose(f);
    printf("%d", a + b);
}
```

---

# 중간시험 예상 문제

### **퀴즈 1. 키보드 입력을 받아 더하는 프로그램에서 사용하는 함수는?**

A. `printf()`  
B. `scanf()`  
C. `fopen()`  
D. `fgets()`

> **정답:** B. `scanf()`  

---

### **퀴즈 2. 배열을 선언하고 2와 3을 저장하려면 어떤 문장이 맞는가?**

A. `int arr = {2, 3};`  
B. `int arr[2] = 2, 3;`  
C. `int arr[2] = {2, 3};`  
D. `int arr(2, 3);`

> **정답:** C. `int arr[2] = {2, 3};`

---

### **퀴즈 3. 함수를 사용해 두 수를 더할 때, 올바른 함수 선언은?**

A. `int sum(int a, int b);`  
B. `void sum(int a, int b);`  
C. `float sum();`  
D. `int sum = add(a, b);`

> **정답:** A. `int sum(int a, int b);`

---

### **퀴즈 4. 구조체를 사용하여 두 수를 저장할 때 올바른 선언은?**

A. `struct Num { int a, b };`  
B. `struct Num = {2, 3};`  
C. `Num a = struct {2, 3};`  
D. `define struct Num(a, b)`

> **정답:** A. `struct Num { int a, b };`

---

### **퀴즈 5. 파일에서 숫자를 읽기 위해 사용하는 함수는 무엇인가?**

A. `scanf()`  
B. `fscanf()`  
C. `printf()`  
D. `gets()`

> **정답:** B. `fscanf()`

---

이 퀴즈들은 학생들이 단순히 코드를 따라치는 것이 아니라 **개념적으로도 이해했는지 확인**하는 데 유용합니다.  
필요하시다면 OX 문제나 빈칸 채우기, 또는 실행 결과 예측 문제도 드릴 수 있어요!
