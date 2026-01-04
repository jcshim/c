```
#include <windows.h>
#include <mfapi.h>
#include <mfplay.h>

#pragma comment(lib,"mfplat.lib")
#pragma comment(lib,"mfplay.lib")
#pragma comment(lib,"mfuuid.lib")

LRESULT CALLBACK WndProc(HWND h, UINT m, WPARAM w, LPARAM l) {
    if (m == WM_DESTROY) PostQuitMessage(0);
    return DefWindowProcW(h, m, w, l);
}

void main() {  // WinMain 대신 main 사용
    HINSTANCE hInst = GetModuleHandle(NULL);

    WNDCLASSW wc = { 0 };
    wc.lpfnWndProc = WndProc; wc.hInstance = hInst; wc.lpszClassName = L"MF";
    RegisterClassW(&wc);

    HWND hwnd = CreateWindowW(L"MF", L"MP4", WS_OVERLAPPEDWINDOW | WS_VISIBLE,
        100, 100, 800, 600, NULL, NULL, hInst, NULL);

    MFStartup(MF_VERSION, MFSTARTUP_LITE);
    IMFPMediaPlayer* p = NULL;
    MFPCreateMediaPlayer(L"shim.mp4", TRUE, 0, NULL, hwnd, &p);

    MSG msg; while (GetMessage(&msg, NULL, 0, 0))
        TranslateMessage(&msg), DispatchMessage(&msg);

    MFShutdown();
}

```
