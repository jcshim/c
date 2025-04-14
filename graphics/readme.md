Windows GDI(Graphics Device Interface)는 Windows 운영체제가 제공하는 2D 그래픽 출력 API이다. 주요 특징은 다음과 같다:

- **장치 독립성**: 화면, 프린터 등 다양한 출력 장치에서 동일한 방식으로 그래픽을 출력할 수 있다. 프로그램이 하드웨어 세부 사항을 직접 다룰 필요 없이 GDI를 통해 그래픽을 생성하고 출력할 수 있다[1][5][13].
- **그래픽 출력 기능**: 선, 도형, 텍스트, 이미지 등을 그릴 수 있는 다양한 함수와 객체를 제공한다. 예를 들어, `Rectangle()` 함수로 사각형을 그릴 수 있다[1][13].
- **GDI 객체**: 그래픽 작업에 사용되는 객체로는 펜(Pen), 브러시(Brush), 비트맵(Bitmap) 등이 있다. 이 객체들은 색상, 두께, 패턴 등을 설정하여 그래픽 요소를 제어한다[1][9].
- **디바이스 컨텍스트(Device Context)**: 그래픽 작업을 수행하기 위한 환경을 정의하는 구조체로, 그리기 작업의 속성을 설정하고 관리한다[5][6].

GDI는 간단한 2D 그래픽 작업에 적합하며, 복잡한 애니메이션이나 고성능 그래픽 작업에는 DirectX나 OpenGL 같은 다른 API를 사용하는 것이 더 적합하다[11][13].

### 윈도우 코딩
[주의] 콘솔프로그램에서는, 속성>링커>시스템>하위시스템>창(/SUBSYSTEM:WINDOWS)

### [코딩 1] 메시지 박스 출력
```
#include <windows.h>

int WINAPI WinMain(HINSTANCE h, HINSTANCE _, LPSTR __, int n) {
    MessageBox(0, L"Welcome!", L"Message", MB_OK);
    return 0;
}
```
### [코딩 2] Welcome 출력
```
#include <windows.h>

LRESULT CALLBACK WndProc(HWND h, UINT m, WPARAM w, LPARAM l) {
    if (m == WM_PAINT) {
        HDC d = BeginPaint(h, &(PAINTSTRUCT){});
        TextOut(d, 10, 10, L"Welcome GKNU", 12);
        EndPaint(h, &(PAINTSTRUCT){});
        return 0;
    }
    if (m == WM_DESTROY) PostQuitMessage(0);
    return DefWindowProc(h, m, w, l);
}

int WINAPI WinMain(HINSTANCE h, HINSTANCE _, LPSTR __, int n) {
    RegisterClass(&(WNDCLASS) { .lpfnWndProc = WndProc, .hInstance = h, .lpszClassName = L"G" });
    ShowWindow(CreateWindow(L"G", L"Title", WS_OVERLAPPEDWINDOW, 0, 0, 640, 480, 0, 0, h, 0), n);
    for (MSG msg; GetMessage(&msg, 0, 0, 0); DispatchMessage(&msg));
}
```
