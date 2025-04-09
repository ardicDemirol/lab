# lab

#include <windows.h>
#include <stdio.h>

HANDLE hEvent;

DWORD WINAPI ThreadFunc(LPVOID lpParam) {
    for (int i = 0; i < 20; i += 5) {
        WaitForSingleObject(hEvent, INFINITE);
        printf("%d,%d,%d,", i, i, i);
        SetEvent(hEvent);
    }
    return 0;
}

int main() {
    hEvent = CreateEvent(NULL, FALSE, TRUE, NULL);
    HANDLE hThread = CreateThread(NULL, 0, ThreadFunc, NULL, 0, NULL);
    
    for (int i = 0; i < 20; i += 5) {
        WaitForSingleObject(hEvent, INFINITE);
        printf("%d,%d,%d,", i, i, i);
        SetEvent(hEvent);
    }
    
    WaitForSingleObject(hThread, INFINITE);
    CloseHandle(hEvent);
    return 0;
}






#include <windows.h>
#include <stdio.h>

CRITICAL_SECTION cs;

DWORD WINAPI ThreadFunc(LPVOID lpParam) {
    for (int i = 0; i < 20; i += 5) {
        EnterCriticalSection(&cs);
        printf("%d,%d,%d,", i, i, i);
        LeaveCriticalSection(&cs);
    }
    return 0;
}

int main() {
    InitializeCriticalSection(&cs);
    HANDLE hThread = CreateThread(NULL, 0, ThreadFunc, NULL, 0, NULL);
    
    for (int i = 0; i < 20; i += 5) {
        EnterCriticalSection(&cs);
        printf("%d,%d,%d,", i, i, i);
        LeaveCriticalSection(&cs);
    }
    
    WaitForSingleObject(hThread, INFINITE);
    DeleteCriticalSection(&cs);
    return 0;
}
