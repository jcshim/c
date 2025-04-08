### 🔢 1. 구구단 출력 (for문)
```c
#include <stdio.h>

int main() {
    int dan;
    printf("몇 단을 출력할까요? ");
    scanf("%d", &dan);
    
    for(int i = 1; i <= 9; i++) {
        printf("%d x %d = %d\n", dan, i, dan * i);
    }
    return 0;
}
```
- ✅ *왜 기억에 남는가:* 익숙한 내용 + 사용자 입력 + 반복문 구조가 깔끔하게 보임  
- 👉 확장 아이디어: 전체 단(2단~9단) 출력, 이중 for문 사용

---

### 🎲 2. 숫자 맞히기 게임 (while문)
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    int answer, guess;
    srand(time(NULL));
    answer = rand() % 100 + 1;

    printf("1부터 100 사이의 숫자를 맞혀보세요!\n");

    while (1) {
        printf("숫자 입력: ");
        scanf("%d", &guess);

        if (guess == answer) {
            printf("정답입니다!\n");
            break;
        } else if (guess < answer) {
            printf("더 큰 수입니다.\n");
        } else {
            printf("더 작은 수입니다.\n");
        }
    }
    return 0;
}
```
- ✅ *왜 기억에 남는가:* 게임 요소 + 실시간 피드백 + 무한루프와 break의 의미 설명하기 좋음  
- 👉 확장 아이디어: 시도 횟수 제한, 힌트 범위 제공

---

### 🔁 3. 별찍기 (중첩 for문, 피라미드)
```c
#include <stdio.h>

int main() {
    int i, j, rows;
    printf("몇 줄의 별을 찍을까요? ");
    scanf("%d", &rows);

    for(i = 1; i <= rows; i++) {
        for(j = 1; j <= i; j++) {
            printf("*");
        }
        printf("\n");
    }
    return 0;
}
```
- ✅ *왜 기억에 남는가:* 결과가 시각적으로 확실하고, 중첩 for문의 동작이 직관적으로 보임  
- 👉 확장 아이디어: 역삼각형, 피라미드 형태, 다이아몬드 만들기

---

### 🕹️ 4. 단순한 타이머 (do-while)
```c
#include <stdio.h>

int main() {
    int count = 5;

    do {
        printf("%d초 남았습니다...\n", count);
        count--;
    } while (count > 0);

    printf("타임아웃!\n");
    return 0;
}
```
- ✅ *왜 기억에 남는가:* do-while의 구조를 실감나게 보여줌 (한 번은 반드시 실행됨)

