좋아요! C언어로 “바로 컴파일해서 돌려볼 수 있는” 초간단 시뮬레이션 5가지를 골라 짧은 설명과 함께 각각 별도 프로그램으로 드릴게요. (모두 `gcc file.c -O2 -o app`로 컴파일 가능)

---

# 1) 동전 던지기 확률 추정 (베르누이 시뮬레이션)

```c
// coin.c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void){
    const int N = 1000000;
    int heads = 0;
    srand((unsigned)time(NULL));
    for(int i=0;i<N;i++){
        if (rand() % 2 == 0) heads++; // 앞면
    }
    printf("시행=%d, 앞면 비율=%.6f\n", N, (double)heads/N);
    return 0;
}
```

---

# 2) 두 주사위 합 분포 (확률분포 근사)

```c
// dice.c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void){
    const int N = 500000;
    int cnt[13]={0};
    srand((unsigned)time(NULL));
    for(int i=0;i<N;i++){
        int a = rand()%6 + 1;
        int b = rand()%6 + 1;
        cnt[a+b]++;
    }
    for(int s=2;s<=12;s++){
        printf("합=%2d : 빈도=%7d  비율=%.5f\n", s, cnt[s], (double)cnt[s]/N);
    }
    return 0;
}
```

---

# 3) 1차원 랜덤 워크 (끝점과 최대거리)

```c
// random_walk.c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void){
    const int STEPS = 100000;
    int x = 0, maxabs = 0;
    srand((unsigned)time(NULL));
    for(int i=0;i<STEPS;i++){
        x += (rand()%2==0) ? +1 : -1;
        if (abs(x) > maxabs) maxabs = abs(x);
    }
    printf("총 걸음=%d, 최종 위치=%d, 최대 |위치|=%d\n", STEPS, x, maxabs);
    return 0;
}
```

---

# 4) 원주율(π) 몬테카를로 추정 (다트보드)

```c
// monte_pi.c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void){
    const int N = 2000000;
    int hit = 0;
    srand((unsigned)time(NULL));
    for(int i=0;i<N;i++){
        double x = (double)rand()/RAND_MAX; // [0,1]
        double y = (double)rand()/RAND_MAX; // [0,1]
        if (x*x + y*y <= 1.0) hit++;
    }
    double pi = 4.0 * hit / N;
    printf("표본=%d, 추정 pi=%.6f (오차 ~ %.6f)\n", N, pi, pi-3.141593);
    return 0;
}
```

---

# 5) 단위시간 이산 대기열(1서버) 시뮬레이션

* 매 틱마다 손님이 올 확률 `p_arr`
* 서버가 바쁠 땐 처리 중, 틱마다 완료될 확률 `p_srv`
* 평균 대기시간과 평균 큐 길이를 추정

```c
// queue_sim.c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

static int bernoulli(double p){ return ((double)rand()/RAND_MAX) < p; }

int main(void){
    const int T = 1000000;   // 총 시뮬레이션 틱
    const double p_arr = 0.30; // 도착 확률
    const double p_srv = 0.50; // 서비스 완료 확률(서버가 바쁠 때)
    int queue_len = 0;
    int server_busy = 0;

    long long total_served = 0;
    long long cum_queue_time = 0; // 각 틱의 큐 길이 합(= 대기시간 총합)
    int max_queue = 0;

    srand((unsigned)time(NULL));
    for(int t=0; t<T; t++){
        // 1) 도착
        if (bernoulli(p_arr)) queue_len++;

        // 2) 서비스 진행
        if (server_busy){
            if (bernoulli(p_srv)){
                // 현재 작업 완료
                total_served++;
                server_busy = 0;
            }
        }

        // 3) 서버가 비었고 대기열이 있으면 즉시 한 명 시작
        if (!server_busy && queue_len > 0){
            queue_len--;
            server_busy = 1;
        }

        // 통계
        cum_queue_time += queue_len;       // 현재 대기 중인 모든 손님의 대기에 1틱 추가
        if (queue_len > max_queue) max_queue = queue_len;
    }

    double avg_queue_len = (double)cum_queue_time / T;
    // 리틀의 법칙 근사: 평균 대기시간 ≈ 평균 큐 길이 / 처리율
    double throughput = (double)total_served / T;
    double avg_wait = (throughput > 1e-12) ? (avg_queue_len / throughput) : 0.0;

    printf("틱=%d, 도착확률=%.2f, 서비스완료확률=%.2f\n", T, p_arr, p_srv);
    printf("처리 건수=%lld (처리율=%.5f/틱)\n", total_served, throughput);
    printf("평균 큐 길이=%.5f, 최대 큐 길이=%d\n", avg_queue_len, max_queue);
    printf("평균 대기시간(틱)=%.5f\n", avg_wait);
    return 0;
}
```

---

원하시면 위 5개를 하나의 메뉴형 프로그램으로 합쳐서(옵션 번호 입력) 실행되게도 묶어드릴게요. 또, 난수 분포를 더 사실적으로 쓰고 싶다면 `rand()` 대신 간단한 LCG나 xorshift를 넣어주는 것도 추천합니다.
