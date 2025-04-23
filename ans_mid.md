## 중간시험 프로그래밍 첫 걸음 2025-1
```
1. C 언어에서 프로그램 시작점은 ( ) 함수이다.
2. 값을 변경할 수 없는 상수를 선언할 때 사용하는 키워드는 ( ) 또는 ( )이다.
3. 산술 연산자 중에 나눗셈 결과에서 나머지를 구하는 연산자는? ( ) 연산자
4. if-else 문은 조건에 따라 ( ) 개의 선택지 중 하나를 실행하고, switch 문은 ( )에 따라 분기한다.
5. 1차원 배열은 메모리에 ( )적으로 저장되고, 배열 요소는 ( )로 구분한다.
6. 편집기로 작성한 test.c를 (  )파일이라 하고 이 파일을 (  )하여 생성된 test.exe 파일을 (  )이라 한다.
7. 동일한 자료형의 데이터 여러 개를 (  )로 접근할 수 있는 데이터 구조를 (  )이라 한다.
8. 컴퓨터에서 8비트를 묶은 단위를 ( )라 한다.
9. 다음 수식에서 사용된 연산자를 우선순위가 높은 것부터 차례로 3개를 적으시오.
   z = x * y++;
10. 무한 반복하는 코드 2가지를 적으시오.
11. for 문 괄호 사이 a, b, c는 무엇이 들어가는가? for(a; b; c){  }
12. C언어에서 사용하는 제어문 중에 대표되는 4개를 적으시오.
13. 마일로 표현된 float d=5.0; 를 Km로 float km; 로 변환하는 부분 코드를 적으시오. (1마일 = 1.6Km)
14. 다음 실행 결과를 적으시오. int a=1; printf(“%d, ”, ++a); printf(“%d\n”, a--);  
15. 다음 실행 결과는
   #include <stdio.h>
   #define GKNU 5 
   int Add(int a, int b){ return (a+b);}
   int main(){
	int a=1, r = Add(a, GKNU);
	printf("%d", r);
   }
16. 다음 중 변수 이름으로 부적절한 것을 모두 선택하시오.
   1) #w 2) _2t 3) for 4) to@of
17. 결과 값은? int a=2, b=3, c=a/b; printf(“%d\n”, c);
18. 양쪽 값이 a, 와 b는 같다고 표현하는 연산자는?
19. [예/아니오] if( 0<= x <= 10){ ; } 코드는 오류가 난다.
20. 다음 코드의 결과는?
    #include<stdio.h>
    void main(){
	int i=1;
	while(i<=6){
		printf("%d\t", i); i++;
		if(i==3) break;
      }
    }
[C언어 코딩-실행이 되도록 확실하게]
21. 두 정수 11, 22를 메모리에 저장하고 더하여 화면에 출력하는 코드 작성하시오.
22. 1부터 10까지 더하는 프로그램을 for() 문을 활용하여 C언어로 작성하시오.
23. 두 개의 수를 입력받고, 큰 수를 찾아 화면에 출력하는 코드를 적으시오.
24. 메모리에 저장된 두 수를 함수를 통해 더하고, 결과를 반환하여 화면에 출력하는 코드를 작성하시오.
[다음 중 단 1개만 선택하여 코딩하시오. 하나 이상은 감점해요.]
25. 키보드로 온도를 읽어서 30도보다 크면 '수영장' 그렇지 않으면 '카페'를 출력하는 코드를 작성하시오.
26. switch 문으로 숫자를 키보드에서 읽고, 1이면 "하나", 2면 "둘", 3이면 "셋"을 화면 출력하고, 그 외 숫자는 "다시"를 출력하시오. 이것을 반복하며 입력값이 0이면 종료된다.
27. 두 수 3, 4를 my.txt에 저장하는 코드를 작성하시오.
● 이 수업의 질문이나 건의, 소감 또는 감사의 글을 적으시오.
```
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
9. ++, * , =  
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
20. 1   	2	

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
