다음은 C언어 중간시험 문항 번호와 정답만 간단히 정리한 내용입니다:

```
1. main  
2. const, #define  
3. %  
4. 두, 값  
5. 연속, 인덱스  
6. 소스, 컴파일, 실행파일  
7. 인덱스, 배열  
8. 바이트  
9. *, ++, =  
10. while(1), for(;;)  
11. 초기식, 조건식, 증감식  
12. if, switch, while, for  
13. km = d * 1.6;  
14. 2, 2  
15. 6  
16. 1), 3), 4)  
17. 0  
18. ==  
19. 예  
20. 1	2	

21. `int a=11,b=22; printf("%d", a+b);`  
22. `int sum=0; for(int i=1;i<=10;i++) sum+=i; printf("%d", sum);`  
23. `int a,b; scanf("%d%d",&a,&b); printf("%d", a>b?a:b);`  
24. `int add(int x,int y){return x+y;} int main(){printf("%d", add(3,4));}`  
25. `int t; scanf("%d",&t); printf(t>30?"수영장":"카페");`  
26. `int n; while(scanf("%d",&n)&&n){ switch(n){case 1:puts("하나");break;case 2:puts("둘");break;case 3:puts("셋");break;default:puts("다시");} }`  
27. `FILE* f=fopen("my.txt","w"); fprintf(f,"3 4"); fclose(f);`  
```

다음은 각 문항에 대한 **완성된 실행 가능한 C 코드**입니다. 각 코드는 별도로 실행 가능합니다.

---

**21. 두 정수를 더하여 출력**
```c
#include <stdio.h>

int main() {
    int a = 11, b = 22;
    printf("%d\n", a + b);
    return 0;
}
```

---

**22. 1부터 10까지의 합**
```c
#include <stdio.h>

int main() {
    int sum = 0;
    for (int i = 1; i <= 10; i++) sum += i;
    printf("%d\n", sum);
    return 0;
}
```

---

**23. 두 수 중 큰 수 출력**
```c
#include <stdio.h>

int main() {
    int a, b;
    scanf("%d %d", &a, &b);
    printf("%d\n", a > b ? a : b);
    return 0;
}
```

---

**24. 함수로 두 수를 더한 결과 출력**
```c
#include <stdio.h>

int add(int x, int y) {
    return x + y;
}

int main() {
    printf("%d\n", add(3, 4));
    return 0;
}
```

---

**25. 온도에 따른 출력**
```c
#include <stdio.h>

int main() {
    int t;
    scanf("%d", &t);
    printf("%s\n", t > 30 ? "수영장" : "카페");
    return 0;
}
```

---

**26. switch문 반복 숫자 출력**
```c
#include <stdio.h>

int main() {
    int n;
    while (scanf("%d", &n) && n) {
        switch (n) {
            case 1: puts("하나"); break;
            case 2: puts("둘"); break;
            case 3: puts("셋"); break;
            default: puts("다시"); break;
        }
    }
    return 0;
}
```

---

**27. 두 수를 파일에 저장**
```c
#include <stdio.h>

int main() {
    FILE *f = fopen("my.txt", "w");
    if (f != NULL) {
        fprintf(f, "3 4");
        fclose(f);
    } else {
        printf("파일 열기 실패\n");
    }
    return 0;
}
```
