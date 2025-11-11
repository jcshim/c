## 2) 문자 키(char 1글자) + 직접 주소법(초간단)

가장 쉬운 형태: 해시 없이 바로 인덱스로 사용 (unsigned char 0~255)
충돌이 아예 없음(같은 문자는 같은 칸)
값이 없음을 -1로 표시
```
#include <stdio.h>

int main(void){
    int table[256];
    for (int i=0;i<256;i++) table[i] = -1; // -1: 비어 있음

    // 넣기 (문자 → 점수)
    table[(unsigned char)'A'] = 90;
    table[(unsigned char)'B'] = 75;
    table[(unsigned char)'Z'] = 88;

    // 조회
    unsigned char k = 'A';
    if (table[k] != -1)
        printf("%c -> %d\n", k, table[k]);
    else
        printf("%c not found\n", k);

    // 삭제(값을 -1로)
    table[(unsigned char)'B'] = -1;

    // 덤프(일부만)
    puts("\n=== CHAR TABLE (A..Z) ===");
    for (unsigned char c='A'; c<='Z'; c++) {
        if (table[c] != -1) printf("[%c] = %d\n", c, table[c]);
    }
    return 0;
}
```
## 메모: 
문자 1글자 키는 직접 주소법이 가장 쉽고 교육용으로 직관적입니다.

“해시 + 체이닝”으로도 만들고 싶다면, 위의 정수용 코드를 그대로 쓰고
hash = ((unsigned char)key) % BUCKETS; 로만 바꾸면 됩니다.
(노드의 key 타입을 char로 바꾸면 끝!)

