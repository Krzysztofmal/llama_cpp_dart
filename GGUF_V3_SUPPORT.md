# GGUF Format Support

This fork of `llama_cpp_dart` uses the **latest version** of llama.cpp from the master branch.

## Overview

By tracking the latest llama.cpp master branch, this fork provides comprehensive support for all GGUF format versions through automatic backward compatibility.

## What is GGUF?

GGUF (GPT-Generated Unified Format) is the model format used by llama.cpp:
- **GGUF v2**: Original stable format (2023)
- **GGUF v3**: Intermediate version with improved metadata (early 2024)
- **GGUF v4**: Current format with enhanced features (2024+)

## Changes in This Fork

### 1. Submodule Update
The `src/llama.cpp` submodule tracks the **latest master branch** of llama.cpp, ensuring:
- All GGUF format versions are supported
- Latest performance optimizations
- Most recent bug fixes
- New features as they're released

### 2. Compatibility Matrix

| GGUF Version | Status |
|--------------|--------|
| v2 | ✅ Fully supported (backward compatible) |
| v3 | ✅ Fully supported (backward compatible) |
| v4 | ✅ Fully supported (current standard) |

The library **automatically detects** the GGUF version and loads models accordingly.

## Building Native Libraries

After cloning this fork, you'll need to rebuild the native libraries for your platform.

### macOS (ARM64 - Apple Silicon)
```bash
cd src/llama.cpp
cmake -B build -DBUILD_SHARED_LIBS=ON -DLLAMA_METAL=ON -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release -j 8
cp build/libllama.dylib ../../bin/MAC_ARM64/
cd ../..
```

### macOS (Intel x64)
```bash
cd src/llama.cpp
cmake -B build -DBUILD_SHARED_LIBS=ON -DLLAMA_METAL=ON -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release -j 8
cp build/libllama.dylib ../../bin/MAC_X64/
cd ../..
```

### Linux (x64)
```bash
cd src/llama.cpp
cmake -B build -DBUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release -j 8
cp build/libllama.so ../../bin/LINUX_X64/
cd ../..
```

### Windows (x64)
```powershell
cd src\llama.cpp
cmake -B build -DBUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release
copy build\Release\llama.dll ..\..\bin\WINDOWS_X64\
cd ..\..
```

### Android
```bash
cd android
./gradlew assembleRelease
```

### iOS
```bash
cd ios
xcodebuild -project llama_cpp_dart.xcodeproj -scheme llama_cpp_dart -configuration Release
```

## Usage Example

```dart
import 'package:llama_cpp_dart/llama_cpp_dart.dart';
import 'dart:io';

void main() async {
  // Set library path for your platform
  Llama.libraryPath = "bin/MAC_ARM64/libllama.dylib";
  
  // Load any GGUF model (v2, v3, or v4 - auto-detected)
  final llama = Llama(
    "path/to/model.gguf",
    modelParams: ModelParams()..nGpuLayers = 99,
    contextParams: ContextParams()..nCtx = 2048,
    samplerParams: SamplerParams()..temp = 0.7,
  );
  
  // Use as normal
  llama.setPrompt("Hello, world!");
  await for (final token in llama.generateText()) {
    stdout.write(token);
  }
  
  llama.dispose();
}
```

## Testing with Any GGUF Model

This fork works with models from any source:
- ✅ Hugging Face model hub
- ✅ Models quantized with latest llama.cpp tools
- ✅ Legacy models in older GGUF formats
- ✅ Custom fine-tuned models

The library automatically handles format detection and compatibility.

## Keeping Up to Date

To update to the latest llama.cpp version:

```bash
cd src/llama.cpp
git checkout master
git pull origin master
cd ../..

# Rebuild the library
cd src/llama.cpp
rm -rf build
cmake -B build -DBUILD_SHARED_LIBS=ON -DLLAMA_METAL=ON -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release -j 8
cp build/libllama.dylib ../../bin/MAC_ARM64/  # Adjust for your platform
cd ../..
```

Or use the provided script:
```bash
./update_submodules.sh
```

## Troubleshooting

### Segmentation Fault
If you encounter segmentation faults:
1. Ensure you've recompiled the native library after updating the submodule
2. Verify the library path is correct
3. Check that you're using the correct library for your platform

### Library Not Found
Ensure you've built the native libraries for your platform and set the correct path:
```dart
Llama.libraryPath = "bin/YOUR_PLATFORM/libllama.{dylib|so|dll}";
```

### Compilation Errors
Make sure you've properly initialized and updated submodules:
```bash
git submodule update --init --recursive
```

### Model Loading Errors
If a model fails to load:
- Check the model file isn't corrupted
- Ensure you have enough RAM/VRAM
- Try reducing `nGpuLayers` or `nCtx`

## Benefits of Latest Version

Using the latest llama.cpp master provides:
- 🚀 **Better Performance**: Latest optimizations for Metal, CUDA, and CPU
- 🐛 **Bug Fixes**: All known issues resolved
- 📦 **New Features**: Latest capabilities as they're added
- 🔒 **Security**: Latest security patches
- 📱 **Broader Device Support**: Optimizations for more hardware

## Original Repository

This is a fork of [netdur/llama_cpp_dart](https://github.com/netdur/llama_cpp_dart).

The main difference is that this fork explicitly tracks the latest llama.cpp master for maximum compatibility and features.

## Contributing

If you find issues, please open an issue with:
- Your platform (OS and architecture)
- The GGUF model you're testing
- llama.cpp commit hash (from `git log -1` in `src/llama.cpp`)
- Full error logs

## License

MIT License (same as original repository)

Copyright (c) 2024 netdur (original)
Copyright (c) 2026 Krzysztofmal (fork maintainer)
