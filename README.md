# Заметки по C++

### Полезный софт
* **HxD** - файл в массив байт

### DLL для внедрения в чужой процесс
```cpp
#include <Windows.h>

BOOL APIENTRY DllMain(HMODULE hModule, DWORD  ul_reason_for_call, LPVOID lpReserved)
{
	switch (ul_reason_for_call)
	{
	case DLL_PROCESS_ATTACH:
		DisableThreadLibraryCalls(hModule);
		CreateThread(NULL, NULL, (LPTHREAD_START_ROUTINE)main, NULL, NULL, NULL);
	case DLL_THREAD_ATTACH:
	case DLL_THREAD_DETACH:
	case DLL_PROCESS_DETACH:
		break;
	}
	return TRUE;
}
```
    
***

### Массив байт в vector\<BYTE\>
```cpp
typedef unsigned char BYTE;
BYTE bytes[2] = {0x0, 0x1};

const std::vector<BYTE> vectorBytes(bytes, bytes + sizeof(bytes));
```

***

### Считать с файла vector\<BYTE\> 
```cpp 	
ifstream inputDll("C:\Library.dll", ios::binary);
if (inputDll.is_open())
{
    vector<BYTE> rawData((istreambuf_iterator<char>(inputDll)), istreambuf_iterator<char>());
    inputDll.close();
}
```
[Подробнее на **StackOverflow**](https://stackoverflow.com/questions/15138353/how-to-read-a-binary-file-into-a-vector-of-unsigned-chars)
  
***

### Записать в файл vector\<BYTE\>
```cpp
ofstream outDll("C:\Library.dll", ios::out | ios::binary);
outDll.write(reinterpret_cast<char*>(rawData.data()), rawData.size());
outDll.close();
```
[Подробнее на **StackOverflow**](https://stackoverflow.com/questions/22662728/c-writing-to-file-vector-of-byte)

***

### Xor шифрование массива байт
```cpp
std::wstring pass = L"password";
std::vector<BYTE> rawResult;

for (int i = 0; i < vectorBytes.size(); i++)
  rawResult.push_back(vectorBytes[i] ^ pass[i % pass.size()]);
```
                                         
