# Dynamic Link Library (DLL)

## 1. What is that?
First, what is a DLL?
DLL stands for **Dynamic Link Library**.
It is code that can be used by more than one program and dynamically links into that program.
By using DLLs, a program can be modularized into separate components.

Imagine having a program that implements the RSA encryption algorithm.
Why not pack it into a dll and reuse the same code in other projects.
By linking it dynamically into your program, the code itself is not contained in your resulting binary, as opposed to static linking.

## 2. Advantages of DLLs

Advantage of dlls:

- Fewer resources get used
  When dlls get used by multiple programs, it reduces code duplication that's loaded into disk and RAM.
  They greatly influence the performance of the program they are running on and of the whole operating system.

- Promoting modular architecture
  DLLs help when making a program modular. You can only load the dlls you need.

- Easy deployment and installation
  When a dlls updates or fixes a problem, not every program downstream has to be relinked.

- Multilanguage Support
  Programs that are written in different Programming languages can use the same DLL functions.
  For that to work the callee and the function have to use the same calling convention. 

## 3. Types of DLLs

### 3.1 Load-time dynamic linking
In load-time dynamic linking, the application makes explicit calls to DLL functions like local functions.
For load-time dynamic linking to work, you need a header (**.h**) file and an import library (**.lib**) file.
The linker will now provide the system with the information to load the DLL at run time.

You might prefer load-time dynamic linking when preferring ease of use because exported dll functions look like local functions.

### 3.2 Run-time dynamic linking
In Run-time dynamic linking, the application calls the [LoadLibrary](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibrarya)
or the [LoadLibraryEx](https://learn.microsoft.com/en-us/windows/desktop/api/LibLoaderAPI/nf-libloaderapi-loadlibraryexa) functions to load the DLL into virtual memory space.
Then you can resolve the function you want to use from that DLL with the [GetProcAddress](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-getprocaddress) 
function.

You might prefer run-time linking when startup performance is important to you or when an application branch loads different modules as required.

## 4. DLL Entry Point
Dlls have an entry function called **DllMain()**.
This function is called when processes or threads attach or detach themselves from the dll.
Here you can initialize or destroy data structures as required.
If you have multiple threads you can use Thread-Local Storage to allocate memory that is private to each thread.

```cpp
BOOL APIENTRY DllMain(
    HMODULE hModule,    // Handle to the DLL Module
    DWORD Reason,       // Reason for the calling function

) {
    switch(Reason) {
        case DLL_PROCESS_ATTACHED: // A process is loading the DLL.
        break;
        case DLL_THREAD_ATTACHED: // A process is creating a new thread.
        break;
        case DLL_THREAD_DETACH: // A thread exits normally.
        break;
        case DLL_PROCESS_DETACH: // A process unloads the DLL.
        break;
    }
    return TRUE;
}
```

If the DLL Entry Point returns **FALSE**, the application will not start when using load-time linking.
Run-time dynamic linking just fails to load the library when returning **FALSE**.

The entry point should only perform simple initialization tasks. DON'T call any other DLL loading or termination functions.

## 5. Export DLL functions

### 5.1 Using the __declspec(dllexport)

```cpp
#include "randomDLL.h"

#include <windows.h>

BOOL APIENTRY DllMain(
	HMODULE hModule,
	DWORD Reason,
	LPVOID Reserved
) {
	switch (Reason) {

	}
	return TRUE;
}

extern "C" {
	__declspec(dllexport) void _cdecl hello() {
		MessageBoxW(NULL, L"Hallo von der Massagebox", L"Sehr wichtige ansage", MB_OK);
	}
}
```

With the [\_\_declspec](https://learn.microsoft.com/en-us/cpp/cpp/declspec?view=msvc-170) extension, you can specify storage-class information.
With **dllexport** you can specify that you want to export this function.
