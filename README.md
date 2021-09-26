# Standart C++ Headers for gnustl_shared.so

This headers are used by Inner Core and Inner Core native mods to link gnustl_shared.so and minecraft library, based on it. This headers can be used along with standard headers without conflict, as they are using separate namespace.

This repository is the fork of [zheka2304's gnustl-shared-headers repository](https://github.com/zheka2304/gnustl-shared-headers). Headers here are edited to work in native modding on mobile.

## Installation
To start using this headers on mobile, download the archive from the [release page](https://github.com/DMHYT/mobile-gnustl-headers/releases/tag/000). Extract `stl` folder into the root of your native directory, and then go to `manifest` file and put `"stl"` in the `"include"` param.

<!-- **IMPORTANT NOTE:** you should link `gnustl_shared` to your native library for this to function normally! -->

## Usage
Namespace `std::__ndk1` declared in headers is identical to `std` namespace, so if you need to use for example vector from this namespace, you include `"stl/vector.h"` and use `std::__ndk1::vector<int>`. 

But consider, that types, declared in `std::__ndk1` differ in internal structure from ones in `std` namespace and cannot be replaced by their counterpart.

**IMPORTANT NOTE:** when including some of stl headers, you must use only quotes, and not angle brackets. (for example, `#include "stl/string"` instead of `#include <stl/string>`)

## Common practices

Never use `using namespace std::__ndk1` to reduce amount of code, this is a bad practice even in case of one standard namespace, but in this case, when you have 2 standard namespaces, it will become a terrible mess.

Consider using global macro instead, like so:
```cpp
// std::__ndk1::string -> stl::string
#define stl std::__ndk1
```

String conversion can be easily done via `data()` method:
```cpp
#include "stl/string"

std::string s = "a";
stl::string stl_s = s.data();
std::string std_s = stl_s.data();
```

