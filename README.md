## Licenses

TinyGLTF is licensed under MIT license.

TinyGLTF uses the following third party libraries.

* json.hpp : Copyright (c) 2013-2017 Niels Lohmann. MIT license.
* base64 : Copyright (C) 2004-2008 René Nyffenegger
* stb_image.h : v2.08 - public domain image loader - [Github link](https://github.com/nothings/stb/blob/master/stb_image.h)
* stb_image_write.h : v1.09 - public domain image writer - [Github link]

### Loading glTF 2.0 model

```c++
// Define these only in *one* .cc file.
#define TINYGLTF_IMPLEMENTATION
#define STB_IMAGE_IMPLEMENTATION
#define STB_IMAGE_WRITE_IMPLEMENTATION
// #define TINYGLTF_NOEXCEPTION // optional. disable exception handling.
#include "tiny_gltf.h"

using namespace tinygltf;

Model model;
TinyGLTF loader;
std::string err;
std::string warn;

bool ret = loader.LoadASCIIFromFile(&model, &err, &warn, argv[1]);
//bool ret = loader.LoadBinaryFromFile(&model, &err, &warn, argv[1]); // for binary glTF(.glb)

if (!warn.empty()) {
  printf("Warn: %s\n", warn.c_str());
}

if (!err.empty()) {
  printf("Err: %s\n", err.c_str());
}

if (!ret) {
  printf("Failed to parse glTF\n");
  return -1;
}
```

#### Loader options

* `TinyGLTF::SetPreserveimageChannels(bool onoff)`. `true` to preserve image channels as stored in image file for loaded image. `false` by default for backward compatibility(image channels are widen to `RGBA` 4 channels). Effective only when using builtin image loader(STB image loader).

## Compile options

* `TINYGLTF_NOEXCEPTION` : Disable C++ exception in JSON parsing. You can use `-fno-exceptions` or by defining the symbol `JSON_NOEXCEPTION` and `TINYGLTF_NOEXCEPTION`  to fully remove C++ exception codes when compiling TinyGLTF.
* `TINYGLTF_NO_STB_IMAGE` : Do not load images with stb_image. Instead use `TinyGLTF::SetImageLoader(LoadimageDataFunction LoadImageData, void *user_data)` to set a callback for loading images.
* `TINYGLTF_NO_STB_IMAGE_WRITE` : Do not write images with stb_image_write. Instead use `TinyGLTF::SetImageWriter(WriteimageDataFunction WriteImageData, void *user_data)` to set a callback for writing images.
* `TINYGLTF_NO_EXTERNAL_IMAGE` : Do not try to load external image file. This option would be helpful if you do not want to load image files during glTF parsing.
* `TINYGLTF_ANDROID_LOAD_FROM_ASSETS`: Load all files from packaged app assets instead of the regular file system. **Note:** You must pass a valid asset manager from your android app to `tinygltf::asset_manager` beforehand.
* `TINYGLTF_ENABLE_DRACO`: Enable Draco compression. User must provide include path and link correspnding libraries in your project file.
* `TINYGLTF_NO_INCLUDE_JSON `: Disable including `json.hpp` from within `tiny_gltf.h` because it has been already included before or you want to include it using custom path before including `tiny_gltf.h`.
* `TINYGLTF_NO_INCLUDE_STB_IMAGE `: Disable including `stb_image.h` from within `tiny_gltf.h` because it has been already included before or you want to include it using custom path before including `tiny_gltf.h`.
* `TINYGLTF_NO_INCLUDE_STB_IMAGE_WRITE `: Disable including `stb_image_write.h` from within `tiny_gltf.h` because it has been already included before or you want to include it using custom path before including `tiny_gltf.h`.
* `TINYGLTF_USE_RAPIDJSON` : Use RapidJSON as a JSON parser/serializer. RapidJSON files are not included in TinyGLTF repo. Please set an include path to RapidJSON if you enable this featrure.
* `TINYGLTF_USE_CPP14` : Use C++14 feature(requires C++14 compiler). This may give better performance than C++11.
