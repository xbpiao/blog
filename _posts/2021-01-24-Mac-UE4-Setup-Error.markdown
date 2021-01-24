---
layout: post
title:  "UE4.26在Mac下Setup.sh错误记录"
date:   2021-01-24 11:09:00 +0800
categories: UE4
typora-root-url: ..\..

---

# 一、环境

macOS、UE4.26源码编译(之前的版本没问题，近期的才需要这么干)

```sh
xbpiao@192 UnrealEngine % git branch
  4.25
  4.25-plus
* 4.26
  release
xbpiao@192 UnrealEngine % git log
commit 422429a318fc3732b92b7eded62819838a2d8a4a (HEAD -> 4.26, origin/4.26)
Author: Rolando Caloca <Rolando.Caloca@epicgames.com>
Date:   Fri Jan 22 12:35:11 2021 -0500

    UE4.26 - Fix for imageview handles deleted still bound on framebuffers
    #rb Brandon.Schaefer, Mihnea.Balta
    #jira UE-105058
    
    [CL 15165078 by Rolando Caloca in 4.26 branch]
```

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