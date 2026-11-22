# DISSECT

A Windows reverse engineering suite built with WPF and .NET 8. Open any file: DISSECT works out what it is, which language and compiler produced it, whether it is packed, and then gives you the deepest view that format allows.

## What it recovers

| Input | Output |
| --- | --- |
| C#, VB.NET, F# (.NET exe/dll) | Full decompiled C# for every type (ILSpy engine), export as one .cs file |
| Java .class | Class structure, fields, constants, methods and a bytecode listing |
| .jar / .apk / .zip | Entry list and manifest; open any entry to analyse it in place |
| Python packed with PyInstaller | The embedded scripts and modules, extracted as .pyc ready for a Python decompiler |
| C, C++, Rust, Go, Delphi, Nim, VB6 (native PE / ELF) | Function discovery, x86/x64 disassembly, and a summary per function (APIs called, strings used, loops, likely purpose) |
| WASM, DEX, Lua bytecode, .pyc, Mach-O | Format structure: imports, exports, sections, classes |
| Source files | Language detection and highlighting |

Native machine code cannot be turned back into its original source. For native files DISSECT shows the real instructions, with calls, imports and strings resolved by name.

## Detection

- Runtimes: .NET, single-file bundles, Native AOT, PyInstaller, Nuitka, Electron, pkg, Launch4j, Unity IL2CPP
- Languages: AutoIt, Go, Rust, Delphi, Nim, VB6
- Compilers: MSVC (from the Rich header), GCC/MinGW, Clang
- Packers and protectors: UPX, Themida, VMProtect, ASPack, MPRESS, Enigma, PECompact, .NET obfuscators, and high-entropy sections

Every verdict lists the evidence behind it.

## Exposed data

Scans every string, ASCII and UTF-16, for:

- private keys, AWS, Google, GitHub, Slack, Stripe, OpenAI and Anthropic keys
- Discord and Telegram tokens and webhooks, JWTs
- connection strings and hard-coded passwords
- URLs, IPs, emails, registry keys and file paths
- PDB build paths, which expose the developer's username
- suspicious API groups: injection, keylogging, anti-debug, persistence, networking, crypto
- crypto constants and faked build timestamps

## Inspection and patching

- PE: headers, security flags, sections with entropy, imports, exports, resources, version info, manifest, TLS callbacks, signer, overlay
- ELF: sections, symbols, needed libraries
- Embedded file carving: PE, ZIP, PNG, JPEG, PDF, 7z, RAR, SQLite and more, each extractable and analysable
- Hex editor: edit mode, text and hex search, value inspector
- Patcher: NOP, invert jump, force jump, return 0 or 1, raw hex edits, revert one or all. Patches are saved to a copy, with the PE checksum recalculated.

## Build

Requires Visual Studio 2022 with the ".NET desktop development" workload, or the .NET 8 SDK.

Open `Dissect.sln` and press F5. The NuGet packages restore automatically.

Single executable:

```
dotnet publish Dissect.csproj -c Release -r win-x64 --self-contained true -p:PublishSingleFile=true -p:IncludeNativeLibrariesForSelfExtract=true -o publish
```

The result is `publish\Dissect.exe`.

## Usage

Drag a file onto the window, use OPEN, or pass a path: `Dissect.exe target.exe`.

- Ctrl+O opens a file, Ctrl+S saves a patched copy, Ctrl+F focuses the filter
- Double click a row to jump to it, double click an instruction to follow it
- ANALYSE on an archive entry or embedded file opens it as a new target, and BACK returns

## Libraries

- [Iced](https://github.com/icedland/iced): x86/x64 decoder
- [ICSharpCode.Decompiler](https://github.com/icsharpcode/ILSpy): .NET decompiler
- [AvalonEdit](https://github.com/icsharpcode/AvalonEdit): code view

## Legal

Only analyse software you own or have permission to examine.

Copyright (c) 2026 Fastest. MIT License.
