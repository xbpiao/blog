---
layout: post
title:  "UE4.26在Mac下Setup.sh错误记录"
date:   2021-01-24 11:09:00 +0800
categories: UE4
typora-root-url: ..\..

---

# 一、环境

macOS、UE4.26源码编译。

# 二、问题记录

执行：**./Setup.sh**

```sh
xbpiao@192 UnrealEngine % ./Setup.sh
Registering git hooks... (this will override existing ones!)
Running system mono/msbuild, version: Mono JIT compiler version 6.12.0.107 (2020-02/a22ed3f094e Wed Oct 28 07:48:22 EDT 2020)
ln: Engine/Binaries/ThirdParty/Mono/Mac/lib/libmsvcrt.dylib: File exists
```

看代码是在：**Engine/Build/BatchFiles/Mac/GitDependencies.sh**

```sh
if [ ! -f Engine/Binaries/ThirdParty/Mono/Mac/lib/libmsvcrt.dylib ]; then
	ln -s /usr/lib/libc.dylib Engine/Binaries/ThirdParty/Mono/Mac/lib/libmsvcrt.dylib
fi
```

哪里使用了**libmsvcrt.dylib**没细看，直接删除

```sh
xbpiao@192 UnrealEngine % rm Engine/Binaries/ThirdParty/Mono/Mac/lib/libmsvcrt.dylib
xbpiao@192 UnrealEngine % ./Setup.sh
Registering git hooks... (this will override existing ones!)
Running system mono/msbuild, version: Mono JIT compiler version 6.12.0.107 (2020-02/a22ed3f094e Wed Oct 28 07:48:22 EDT 2020)
Checking dependencies...
Updating dependencies: 100% (44/44), 38.6/38.6 MiB | 3.00 MiB/s, done.
```

不影响，编译引擎也可以正常跑！