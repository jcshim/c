```
#include <windows.h>
#include <dshow.h>
#include <stdio.h>

#pragma comment(lib, "strmiids.lib")
#pragma comment(lib, "ole32.lib")
#pragma comment(lib, "uuid.lib")

DEFINE_GUID(CLSID_SampleGrabber, 0xc1f400a0, 0x3f08, 0x11d3, 0x9f, 0x0b, 0x00, 0xc0, 0x4f, 0xb6, 0xbd, 0x3d);
DEFINE_GUID(IID_ISampleGrabber, 0x6b652fff, 0x11fe, 0x4fce, 0x92, 0xad, 0x02, 0x75, 0x85, 0x4c, 0x83, 0x66);
DEFINE_GUID(IID_ISampleGrabberCB, 0x0579154a, 0x2b53, 0x4994, 0xb0, 0xd0, 0xe7, 0x73, 0x14, 0x8e, 0xf5, 0x66);

typedef struct ISampleGrabber ISampleGrabber;
typedef struct ISampleGrabberCB ISampleGrabberCB;

typedef struct ISampleGrabberVtbl {
    HRESULT(STDMETHODCALLTYPE* QueryInterface)(ISampleGrabber*, REFIID, void**);
    ULONG(STDMETHODCALLTYPE* AddRef)(ISampleGrabber*);
    ULONG(STDMETHODCALLTYPE* Release)(ISampleGrabber*);
    HRESULT(STDMETHODCALLTYPE* SetOneShot)(ISampleGrabber*, BOOL);
    HRESULT(STDMETHODCALLTYPE* SetMediaType)(ISampleGrabber*, const AM_MEDIA_TYPE*);
    HRESULT(STDMETHODCALLTYPE* GetConnectedMediaType)(ISampleGrabber*, AM_MEDIA_TYPE*);
    HRESULT(STDMETHODCALLTYPE* SetBufferSamples)(ISampleGrabber*, BOOL);
    HRESULT(STDMETHODCALLTYPE* GetCurrentBuffer)(ISampleGrabber*, long*, long*);
    HRESULT(STDMETHODCALLTYPE* GetCurrentSample)(ISampleGrabber*, IMediaSample**);
    HRESULT(STDMETHODCALLTYPE* SetCallback)(ISampleGrabber*, ISampleGrabberCB*, long);
} ISampleGrabberVtbl;

struct ISampleGrabber {
    const ISampleGrabberVtbl* lpVtbl;
};

typedef struct ISampleGrabberCBVtbl {
    HRESULT(STDMETHODCALLTYPE* QueryInterface)(ISampleGrabberCB*, REFIID, void**);
    ULONG(STDMETHODCALLTYPE* AddRef)(ISampleGrabberCB*);
    ULONG(STDMETHODCALLTYPE* Release)(ISampleGrabberCB*);
    HRESULT(STDMETHODCALLTYPE* SampleCB)(ISampleGrabberCB*, double, IMediaSample*);
    HRESULT(STDMETHODCALLTYPE* BufferCB)(ISampleGrabberCB*, double, BYTE*, long);
} ISampleGrabberCBVtbl;

struct ISampleGrabberCB {
    const ISampleGrabberCBVtbl* lpVtbl;
};

#define FRAME_WIDTH 640
#define FRAME_HEIGHT 480

HWND g_hwnd = NULL;
HDC g_hdc = NULL;
BITMAPINFO g_bmi;

void InvertBuffer(BYTE* pBuffer, long size) {
    for (long i = 0; i < size; i++) {
        pBuffer[i] = 255 - pBuffer[i];
    }
}

typedef struct {
    ISampleGrabberCB cb;
} MyGrabberCallback;

HRESULT STDMETHODCALLTYPE MyQueryInterface(ISampleGrabberCB* This, REFIID riid, void** ppv) {
    *ppv = This;
    return S_OK;
}
ULONG STDMETHODCALLTYPE MyAddRef(ISampleGrabberCB* This) { return 2; }
ULONG STDMETHODCALLTYPE MyRelease(ISampleGrabberCB* This) { return 1; }

HRESULT STDMETHODCALLTYPE MySampleCB(ISampleGrabberCB* This, double SampleTime, IMediaSample* pSample) {
    BYTE* pBuffer = NULL;
    if (SUCCEEDED(pSample->lpVtbl->GetPointer(pSample, &pBuffer))) {
        long size = pSample->lpVtbl->GetActualDataLength(pSample);
        BYTE* pCopy = (BYTE*)malloc(size);
        if (pCopy) {
            memcpy(pCopy, pBuffer, size);
            InvertBuffer(pCopy, size);
            StretchDIBits(g_hdc, 0, 0, FRAME_WIDTH, FRAME_HEIGHT, 0, 0, FRAME_WIDTH, FRAME_HEIGHT, pCopy, &g_bmi, DIB_RGB_COLORS, SRCCOPY);
            free(pCopy);
        }
    }
    return S_OK;
}

HRESULT STDMETHODCALLTYPE MyBufferCB(ISampleGrabberCB* This, double SampleTime, BYTE* pBuffer, long BufferLen) {
    return S_OK;
}

ISampleGrabberCBVtbl g_cbVtbl = {
    MyQueryInterface,
    MyAddRef,
    MyRelease,
    MySampleCB,
    MyBufferCB
};

MyGrabberCallback g_callback = { { &g_cbVtbl } };

LRESULT CALLBACK WndProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam) {
    if (msg == WM_DESTROY) PostQuitMessage(0);
    return DefWindowProc(hwnd, msg, wParam, lParam);
}

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow) {
    if (FAILED(CoInitialize(NULL))) return -1;

    WNDCLASS wc = { 0 };
    wc.lpfnWndProc = WndProc;
    wc.hInstance = hInstance;
    wc.lpszClassName = L"CamWinClass";
    RegisterClass(&wc);

    RECT rc = { 0, 0, FRAME_WIDTH, FRAME_HEIGHT };
    AdjustWindowRect(&rc, WS_OVERLAPPEDWINDOW, FALSE);

    g_hwnd = CreateWindow(wc.lpszClassName, L"Camera Inverted", WS_OVERLAPPEDWINDOW | WS_VISIBLE,
        CW_USEDEFAULT, CW_USEDEFAULT, rc.right - rc.left, rc.bottom - rc.top,
        NULL, NULL, hInstance, NULL);

    if (!g_hwnd) {
        CoUninitialize();
        return -1;
    }

    g_hdc = GetDC(g_hwnd);

    ZeroMemory(&g_bmi, sizeof(g_bmi));
    g_bmi.bmiHeader.biSize = sizeof(BITMAPINFOHEADER);
    g_bmi.bmiHeader.biWidth = FRAME_WIDTH;
    g_bmi.bmiHeader.biHeight = FRAME_HEIGHT;
    g_bmi.bmiHeader.biPlanes = 1;
    g_bmi.bmiHeader.biBitCount = 24;
    g_bmi.bmiHeader.biCompression = BI_RGB;

    IGraphBuilder* pGraph = NULL;
    ICaptureGraphBuilder2* pBuilder = NULL;
    IBaseFilter* pCamera = NULL;
    IBaseFilter* pGrabberFilter = NULL;
    ISampleGrabber* pGrabber = NULL;
    IMediaControl* pControl = NULL;
    IVideoWindow* pVidWin = NULL;

    if (FAILED(CoCreateInstance(&CLSID_FilterGraph, NULL, CLSCTX_INPROC_SERVER, &IID_IGraphBuilder, (void**)&pGraph))) return -1;
    if (FAILED(CoCreateInstance(&CLSID_CaptureGraphBuilder2, NULL, CLSCTX_INPROC_SERVER, &IID_ICaptureGraphBuilder2, (void**)&pBuilder))) return -1;
    pBuilder->lpVtbl->SetFiltergraph(pBuilder, pGraph);

    ICreateDevEnum* pDevEnum = NULL;
    IEnumMoniker* pEnum = NULL;
    IMoniker* pMoniker = NULL;
    if (SUCCEEDED(CoCreateInstance(&CLSID_SystemDeviceEnum, NULL, CLSCTX_INPROC_SERVER, &IID_ICreateDevEnum, (void**)&pDevEnum))) {
        if (SUCCEEDED(pDevEnum->lpVtbl->CreateClassEnumerator(pDevEnum, &CLSID_VideoInputDeviceCategory, &pEnum, 0))) {
            if (pEnum && pEnum->lpVtbl->Next(pEnum, 1, &pMoniker, NULL) == S_OK) {
                pMoniker->lpVtbl->BindToObject(pMoniker, NULL, NULL, &IID_IBaseFilter, (void**)&pCamera);
            }
        }
    }

    if (!pCamera) return -1;
    pGraph->lpVtbl->AddFilter(pGraph, pCamera, L"Capture");

    if (FAILED(CoCreateInstance(&CLSID_SampleGrabber, NULL, CLSCTX_INPROC_SERVER, &IID_IBaseFilter, (void**)&pGrabberFilter))) return -1;
    pGraph->lpVtbl->AddFilter(pGraph, pGrabberFilter, L"Grabber");
    pGrabberFilter->lpVtbl->QueryInterface(pGrabberFilter, &IID_ISampleGrabber, (void**)&pGrabber);

    AM_MEDIA_TYPE mt;
    ZeroMemory(&mt, sizeof(mt));
    mt.majortype = MEDIATYPE_Video;
    mt.subtype = MEDIASUBTYPE_RGB24;
    mt.formattype = FORMAT_VideoInfo;
    pGrabber->lpVtbl->SetMediaType(pGrabber, &mt);
    pGrabber->lpVtbl->SetCallback(pGrabber, (ISampleGrabberCB*)&g_callback, 0);

    pBuilder->lpVtbl->RenderStream(pBuilder, &PIN_CATEGORY_PREVIEW, &MEDIATYPE_Video, pCamera, pGrabberFilter, NULL);

    pGraph->lpVtbl->QueryInterface(pGraph, &IID_IMediaControl, (void**)&pControl);
    pControl->lpVtbl->Run(pControl);

    pGraph->lpVtbl->QueryInterface(pGraph, &IID_IVideoWindow, (void**)&pVidWin);
    if (pVidWin) {
        LONG style = WS_OVERLAPPEDWINDOW | WS_VISIBLE;
        pVidWin->lpVtbl->put_WindowStyle(pVidWin, style);
        pVidWin->lpVtbl->put_Visible(pVidWin, OATRUE);

        RECT rcMain;
        GetWindowRect(g_hwnd, &rcMain);
        pVidWin->lpVtbl->SetWindowPosition(pVidWin, rcMain.right + 10, rcMain.top, rcMain.right - rcMain.left, rcMain.bottom - rcMain.top);
    }

    MSG msg;
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }

    if (pVidWin) pVidWin->lpVtbl->Release(pVidWin);
    if (pControl) pControl->lpVtbl->Release(pControl);
    if (pGrabber) pGrabber->lpVtbl->Release(pGrabber);
    if (pGrabberFilter) pGrabberFilter->lpVtbl->Release(pGrabberFilter);
    if (pCamera) pCamera->lpVtbl->Release(pCamera);
    if (pBuilder) pBuilder->lpVtbl->Release(pBuilder);
    if (pGraph) pGraph->lpVtbl->Release(pGraph);

    ReleaseDC(g_hwnd, g_hdc);
    CoUninitialize();
    return 0;
}
```
