좋습니다!  
바로 **OpenCL과 CUDA로 벡터 덧셈**하는 간단한 예제를 비교해서 보여드릴게요.  

---

# 🚀 목표
**두 벡터 A, B를 더해서 결과를 C에 저장하는 병렬 코드**입니다.

공식:  
\[
C[i] = A[i] + B[i]
\quad \text{for all} \quad i
\]

---

# 🧩 1. OpenCL 벡터 덧셈 예제

**(A, B, C는 float 배열이고 OpenCL 커널 코드)**

### ✅ OpenCL 커널 코드 (`vector_add.cl`)
```c
__kernel void vector_add(__global const float* A,
                         __global const float* B,
                         __global float* C)
{
    int i = get_global_id(0);
    C[i] = A[i] + B[i];
}
```

### ✅ OpenCL 호스트 코드 (C++)
```cpp
#include <CL/cl.h>
#include <iostream>

int main() {
    const int N = 1024;
    float A[N], B[N], C[N];
    for (int i = 0; i < N; i++) { A[i] = i; B[i] = 2*i; }

    // (1) 플랫폼, 디바이스, 컨텍스트, 큐 생성 (생략)
    // (2) 프로그램 빌드: vector_add.cl (생략)
    // (3) 버퍼 생성
    cl_mem bufferA = clCreateBuffer(context, CL_MEM_READ_ONLY | CL_MEM_COPY_HOST_PTR, sizeof(float)*N, A, NULL);
    cl_mem bufferB = clCreateBuffer(context, CL_MEM_READ_ONLY | CL_MEM_COPY_HOST_PTR, sizeof(float)*N, B, NULL);
    cl_mem bufferC = clCreateBuffer(context, CL_MEM_WRITE_ONLY, sizeof(float)*N, NULL, NULL);

    // (4) 커널 설정
    cl_kernel kernel = clCreateKernel(program, "vector_add", NULL);
    clSetKernelArg(kernel, 0, sizeof(cl_mem), &bufferA);
    clSetKernelArg(kernel, 1, sizeof(cl_mem), &bufferB);
    clSetKernelArg(kernel, 2, sizeof(cl_mem), &bufferC);

    // (5) 실행
    size_t globalSize = N;
    clEnqueueNDRangeKernel(queue, kernel, 1, NULL, &globalSize, NULL, 0, NULL, NULL);

    // (6) 결과 읽기
    clEnqueueReadBuffer(queue, bufferC, CL_TRUE, 0, sizeof(float)*N, C, 0, NULL, NULL);

    std::cout << "C[0] = " << C[0] << std::endl; // 결과 확인

    // (7) 해제 (생략)
}
```
(※ OpenCL은 준비 코드가 길고 복잡한 편입니다.)

---

# 🧩 2. CUDA 벡터 덧셈 예제

### ✅ CUDA 커널 코드
```cpp
__global__ void vector_add(const float* A, const float* B, float* C, int N) {
    int i = blockIdx.x * blockDim.x + threadIdx.x;
    if (i < N)
        C[i] = A[i] + B[i];
}
```

### ✅ CUDA 호스트 코드 (C++)
```cpp
#include <iostream>

int main() {
    const int N = 1024;
    float *A, *B, *C;
    cudaMallocManaged(&A, N * sizeof(float));
    cudaMallocManaged(&B, N * sizeof(float));
    cudaMallocManaged(&C, N * sizeof(float));

    for (int i = 0; i < N; i++) { A[i] = i; B[i] = 2*i; }

    // (1) 커널 실행
    int threadsPerBlock = 256;
    int blocksPerGrid = (N + threadsPerBlock - 1) / threadsPerBlock;
    vector_add<<<blocksPerGrid, threadsPerBlock>>>(A, B, C, N);

    // (2) GPU 작업 완료 대기
    cudaDeviceSynchronize();

    std::cout << "C[0] = " << C[0] << std::endl; // 결과 확인

    // (3) 메모리 해제
    cudaFree(A); cudaFree(B); cudaFree(C);
}
```

(※ CUDA는 OpenCL보다 준비 코드가 훨씬 간결합니다.)

---

# 🎯 정리 비교

| 항목 | OpenCL | CUDA |
|:---|:---|:---|
| 준비 작업 | 플랫폼/디바이스/컨텍스트/큐 설정 필요 | 디바이스 자동, 메모리만 설정 |
| 커널 호출 | 명령 큐 사용 (`clEnqueueNDRangeKernel`) | <<<블록 수, 스레드 수>>> 로 호출 |
| 메모리 관리 | 버퍼 생성/복사 필요 | Unified Memory (`cudaMallocManaged`) 사용 가능 |
| 코드 복잡도 | 상대적으로 복잡 | 상대적으로 간결 |
| 하드웨어 호환성 | 다양한 제조사 디바이스 | NVIDIA GPU 전용 |

---

# ✨ 최종 한 줄 요약

> OpenCL은 **범용성**은 높지만 **복잡**하고,  
> CUDA는 **NVIDIA 전용**이지만 **간단하고 최적화**되어 있습니다.

---

**추가로**  
👉 "OpenCL과 CUDA 성능 차이 벤치마크 방법"  
👉 "OpenCL을 더 간단하게 쓰는 C++ Wrapping 방법"  
같은 것도 해볼까요? 🚀

필요하면 "벤치마크 알려줘" 또는 "OpenCL 간단하게 쓰는 방법 알려줘"라고 답해주세요! ✨  
(진짜 실습 가능한 코드까지 만들어드릴게요)
