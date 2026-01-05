```
// #define UNICODE
#include <windows.h>
#include <gdiplus.h>
#pragma comment(lib,"gdiplus.lib")
using namespace Gdiplus;

static Image* img;

LRESULT CALLBACK WndProc(HWND h, UINT m, WPARAM w, LPARAM l) {
    if (m == WM_PAINT) {
        PAINTSTRUCT ps; HDC dc = BeginPaint(h, &ps);
        Graphics g(dc);
        g.DrawImage(img, 0, 0, img->GetWidth(), img->GetHeight());
        EndPaint(h, &ps); return 0;
    }
    if (m == WM_DESTROY) { delete img; PostQuitMessage(0); return 0; }
    return DefWindowProc(h, m, w, l);
}

int main() {
    GdiplusStartupInput in; ULONG_PTR t; GdiplusStartup(&t, &in, 0);
    img = new Image(L"shim.png");

    HINSTANCE i = GetModuleHandle(0);
    WNDCLASSW wc = { 0 }; wc.lpfnWndProc = WndProc; wc.hInstance = i; wc.lpszClassName = L"P";
    RegisterClassW(&wc);

    HWND h = CreateWindowW(L"P", L"P", WS_OVERLAPPEDWINDOW | WS_VISIBLE,
        100, 100, img->GetWidth(), img->GetHeight(), 0, 0, i, 0);

    MSG msg; while (GetMessageW(&msg, 0, 0, 0)) TranslateMessage(&msg), DispatchMessageW(&msg);
    GdiplusShutdown(t);
    return 0;
}
```
