> Warnings:
> 
> - This will probably be outdated at some point, please notify me if this stops working
> - Mono 5.10 is required (as of today, this is the alpha)
> - Clang/C2 consumes large amounts of memory when assembling large amalgamations! E.g. for IKVM libraries, it needs at least 3GB of free memory!
> - It is not possible to bundle other native libraries on Windows (e.g. SQLite.Interop.dll), you need to ship them with the binary.
> - CHECK THE LICENSES OF THE LIBRARIES YOU USE!
>

### Introduction

This package enables fully static binary amalgamations with compressed class libraries in Mono 5.10 on Windows using `mkbundle`. It greatly improves the final binary size, usually it ends up around 40 to 60 % of the non-zlib binary.

### Prerequisites

- Mono 5.10
- An installation of Visual Studio 2017, with **at minimum** these invidual components
  - .NET SDK and Targeting Pack of whatever .NET version you are trying to use, must be newer than 4.5
  - Clang/C2 (experimental)
  - MSBuild
  - NuGet
  - VC++ 2017 v141 Toolset (x86,x64)
  - Visual C++ 2017 Redistributable Update
  - Visual Studio C++ core features
  - Windows 8.1 SDK

Selecting those components individually will automatically enable other required components, don't uncheck them.

### Bundling

1. Compile (MSBuild) your solution either from the command line or your preffered IDE
2. Open the `x64 Native Tools Command Prompt for VS 2017` shell (*exactly this one, none other*)
3. Run `set VCSUBSYSTEM=console zlibstatic.lib` for console applications, `set VCSUBSYSTEM=windows zlibstatic.lib` otherwise
4. Let `C:\zlib-win-redist` be the path where you cloned this repository
5. Run `set LIB=%LIB;C:\zlib-win-redist\Library\lib`
6. Run `set INVLUDE=%INCLUDE%;C:\zlib-win-redist\Library\include`
7. Now bundle using (replace the paths with yours) `mkbundle -v --static --deps -o OUT.exe -L "%PROGRAMFILES%\Mono\lib\mono\4.5" IN.exe`
8. You **don't** need to ship any zlib.dll, it has been linked statically! You still need to ship licenses if they require it.

