# GGUF v3 Support

This fork of `llama_cpp_dart` provides support for GGUF v3 format models.

## Overview

The main change is updating the llama.cpp submodule to a version that supports GGUF v3 format (commit `504dc37be` from January 21, 2024).

## What is GGUF v3?

GGUF (GPT-Generated Unified Format) v3 is an intermediate version of the model format used by llama.cpp:
- Introduced improved metadata handling
- Enhanced tensor type support
- Backward compatible with GGUF v2
- Superseded by GGUF v4 in late January 2024

## Changes in This Fork

### 1. Submodule Update
The `src/llama.cpp` submodule has been updated to commit `504dc37be` which provides GGUF v3 support.

### 2. Compatibility Matrix

| GGUF Version | Status |
|--------------|--------|
| v2 | ✅ Supported (backward compatible) |
| v3 | ✅ Fully supported |
| v4 | ⚠️ Not supported in this version |

## Building Native Libraries

After cloning this fork, you'll need to rebuild the native libraries for your platform.

### macOS (ARM64)
```bash
cd src/llama.cpp
cmake -B build -DBUILD_SHARED_LIBS=ON -DLLAMA_METAL=ON
cmake --build build --config Release
cp build/libllama.dylib ../../bin/MAC_ARM64/
```

### macOS (x64)
```bash
cd src/llama.cpp
cmake -B build -DBUILD_SHARED_LIBS=ON -DLLAMA_METAL=ON
cmake --build build --config Release
cp build/libllama.dylib ../../bin/MAC_X64/
```

### Linux (x64)
```bash
cd src/llama.cpp
cmake -B build -DBUILD_SHARED_LIBS=ON
cmake --build build --config Release
cp build/libllama.so ../../bin/LINUX_X64/
```

### Windows (x64)
```bash
cd src/llama.cpp
cmake -B build -DBUILD_SHARED_LIBS=ON
cmake --build build --config Release
copy build\Release\llama.dll ..\..\bin\WINDOWS_X64\
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

void main() async {
  // Set library path for your platform
  Llama.libraryPath = "bin/MAC_ARM64/libllama.dylib";
  
  // Load GGUF v3 model
  final llama = Llama(
    "path/to/model-v3.gguf",
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

## Testing GGUF v3 Models

To verify GGUF v3 support:

1. Download a GGUF v3 model (models created between late 2023 and January 2024)
2. Run the example above
3. The model should load without errors

### Finding GGUF v3 Models

GGUF v3 models were primarily distributed during a brief period in early 2024. You can:
- Check Hugging Face for models uploaded around that time
- Convert older models using llama.cpp tools from the same period
- Use the `update_submodules.sh` script to ensure proper submodule initialization

## Troubleshooting

### "Unsupported GGUF version" Error
This means your model is in GGUF v4 format. This fork specifically supports v3. Use the original repository for v4 support.

### Compilation Errors
Make sure you've properly initialized and updated submodules:
```bash
git submodule update --init --recursive
```

### Library Not Found
Ensure you've built the native libraries for your platform and set the correct path in `Llama.libraryPath`.

## Updating Submodules

Use the provided script to update submodules:
```bash
./update_submodules.sh
```

Note: This script will update to the latest commit on the configured branch. For GGUF v3 support, the submodule is pinned to commit `504dc37be`.

## Original Repository

This is a fork of [netdur/llama_cpp_dart](https://github.com/netdur/llama_cpp_dart).

For the latest features and GGUF v4+ support, see the original repository.

## Contributing

If you find issues with GGUF v3 support in this fork, please open an issue with:
- Your platform (OS and architecture)
- The GGUF v3 model you're testing
- Full error logs

## License

MIT License (same as original repository)

Copyright (c) 2024 netdur (original)
Copyright (c) 2026 Krzysztofmal (fork maintainer)
