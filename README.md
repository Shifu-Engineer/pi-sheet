<a name="#toc"></a>TOC
 - [DllImport](#dllimport)
 - [MarshalAs](#marshalas)
 - [StructureLayout](#structure-layout)
 - [Marshaling Structure & Class](#marshaling-snc)
 - [Marshaling Function](#marshaling-function)
 - [Marshaling Delegate as Callback](#marshaling-delegate)
 - [Misc](#misc)

<br><br>

## <a name="#dllimport"></a>DllImport

`[DllImport("name.dll", ...)]`

*Indicates that the attributed method is exposed by an unmanaged dynamic-link library (DLL) as a static entry point.*

| Field | Usage |
|-------|-------|
| EntryPoint | *Indicates the name or ordinal (i.e. #1) of the DLL entry point to be called*. **default is C# method name.**<br>`[DllImport("deno.dll", EntryPoint = "testPrint")]` |
| CallingConvention | *Indicates the calling convention of an entry point.* **default is __stdcall.**<br>`[DllImport("demo.dll", CallingConvention=CallingConvention.Cdecl)]` |
| CharSet | *To specify the marshaling behavior of string parameters & name mangling.* **default is CharSet.Ansi.**<br>`[DllImport("demo.dll", CharSet=CharSet.Unicode)]` |
| ExactSpelling | *to search an unmanaged DLL for entry-point names other than the one specified.* **default is false.**<br>`[DllImport("demo.dll", CharSet=CharSet.Unicode, ExactSpelling=true)]` |
| BestFitMapping | *mapping of managed Unicode characters to unmanaged ANSI characters.* **default is true.**<br>`[DllImport("demo.dll", CharSet=CharSet.Unicode, BestFitMapping=true)]` | 
| ThrowOnUnmappableChar | *throwing of an exception on an unmappable Unicode character that is converted to an ANSI "?" character..* **default is false.**<br>`[DllImport("demo.dll", BestFitMapping=true, ThrowOnUnmappableChar=false)]` |
| PreserveSig | *indicates whether unmanaged methods that have HRESULT or retval return values are directly translated or whether HRESULT or retval return values are automatically converted to exceptions..* **default is true.**<br>`[DllImport("demo.dll", ExactSpelling = true, PreserveSig = false)]` |
| SetLastError | *enables the caller to use the Marshal.GetLastWin32Error API function to determine whether an error occurred while executing the method.* **default is false.**<br>`[DllImport("demo.dll", SetLastError=true)]` |

Details: [DllImportAttribute Class](https://msdn.microsoft.com/en-us/library/system.runtime.interopservices.dllimportattribute)

<br><br>

## <a name="#marshalas"></a>MarshalAs

`[MarshalAs(UnmanagedType.x)]`

*Indicates how to marshal the data between managed and unmanaged code. You can apply this attribute to **parameters, fields, or return values**. This attribute is optional, as each data type has a default marshaling behavior. This attribute is only necessary when a given type can be marshaled to multiple types **i.e. String***

| C/C++ | C# |
|-------|----|
|`void demo(char* data)` | pass-by-value<br>`static extern void demo([MarshalAs(UnmanagedType.LPStr)] string data)` |
| `void demo(char* data, int size)` | pass-by-ref : **IntPtr**<br>`static extern void demo(IntPtr buffer, int size)` |
| `void demo(char* data, int size)` | pass-by-ref : **StringBuilder**<br>`static extern void demo([MarshalAs(UnmanagedType.LPStr)] StringBuilder buffer, int size)` |
| `char x` | `[MarshalAs(UnmanagedType.U1)]`<br>`public char x` |
| `char* x` | `[MarshalAs(UnmanagedType.LPStr)]`<br>public string x` |
| `char x[256]` | `[MarshalAs(UnmanagedType.ByValTStr, SizeConst=256)]`<br>`public string x` |
| `WCHAR* x` | `[MarshalAs(UnmanagedType.LPWStr)]`<br>`public string x` |
| `WCHAR x[256]` | `[MarshalAs(UnmanagedType.ByValTStr, SizeConst=256)]`<br>`public string x` |
| `TCHAR* x` | `[MarshalAs(UnmanagedType.LPTStr)]`<br>`public string x` |
| `TCHAR x[256]` | `[MarshalAs(UnmanagedType.ByValTStr, SizeConst=256)]`<br>`public string x` |
| `bool` | `MarshalAs(UnmanagedType.U1)` / `MarshalAs(UnmanagedType.I1)` |
| `BOOL` | `MarshalAs(UnmanagedType.Bool)` |
| `int numbers[10]` | `[MarshalAs(UnmanagedType.ByValArray, SizeConst=10)]`<br>`public int[] Numbers` |

Details: [MarshalAsAttribute Class](https://msdn.microsoft.com/en-us/library/system.runtime.interopservices.marshalasattribute) & [UnmanagedType Enumeration](https://msdn.microsoft.com/en-us/library/system.runtime.interopservices.unmanagedtype)





