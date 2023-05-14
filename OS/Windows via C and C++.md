在 Windows 操作系统下，可以使用 CreateMutex 和 OpenMutex 函数创建和打开一个互斥体对象，并将其用于多进程同步。下面是一个示例：

进程1：

#include <windows.h>
#include <iostream>

int main()
{
    HANDLE hMutex = CreateMutex(NULL, FALSE, L"MyMutex");
    if (hMutex == NULL) {
        std::cerr << "Failed to create mutex: " << GetLastError() << std::endl;
        return 1;
    }

    std::cout << "Acquiring mutex in process 1..." << std::endl;
    DWORD result = WaitForSingleObject(hMutex, INFINITE);
    if (result != WAIT_OBJECT_0) {
        std::cerr << "Failed to acquire mutex: " << GetLastError() << std::endl;
        CloseHandle(hMutex);
        return 1;
    }

    std::cout << "Mutex acquired in process 1." << std::endl;

    // Do some work...

    std::cout << "Releasing mutex in process 1..." << std::endl;
    if (!ReleaseMutex(hMutex)) {
        std::cerr << "Failed to release mutex: " << GetLastError() << std::endl;
        CloseHandle(hMutex);
        return 1;
    }

    std::cout << "Mutex released in process 1." << std::endl;

    CloseHandle(hMutex);
    return 0;
}

  
进程2：
  
  #include <windows.h>
#include <iostream>

int main()
{
    HANDLE hMutex = OpenMutex(MUTEX_ALL_ACCESS, FALSE, L"MyMutex");
    if (hMutex == NULL) {
        std::cerr << "Failed to open mutex: " << GetLastError() << std::endl;
        return 1;
    }

    std::cout << "Acquiring mutex in process 2..." << std::endl;
    DWORD result = WaitForSingleObject(hMutex, INFINITE);
    if (result != WAIT_OBJECT_0) {
        std::cerr << "Failed to acquire mutex: " << GetLastError() << std::endl;
        CloseHandle(hMutex);
        return 1;
    }

    std::cout << "Mutex acquired in process 2." << std::endl;

    // Do some work...

    std::cout << "Releasing mutex in process 2..." << std::endl;
    if (!ReleaseMutex(hMutex)) {
        std::cerr << "Failed to release mutex: " << GetLastError() << std::endl;
        CloseHandle(hMutex);
        return 1;
    }

    std::cout << "Mutex released in process 2." << std::endl;

    CloseHandle(hMutex);
    return 0;
}

