---
layout: post
title:  "临时修复EmmyLua调试UE4+UnLua无限循环导致的crash"
date:   2021-02-12 11:44:00 +0800
categories: UE4 UnLua EmmyLua
typora-root-url: ..\..
---

# 一、开发环境

* macOS Big Sur （11.1）
* UE4.26.1 Source
* [UnLua + Lua5.3.6](https://github.com/xbpiao/UnLua/tree/Upgrade_to_Lua5.3.6)
* VSCode1.53.0 + EmmyLua0.3.49

参考：[https://github.com/EmmyLua/VSCode-EmmyLua/issues/58](https://github.com/EmmyLua/VSCode-EmmyLua/issues/58) 配置

emmylua_new supports Windows/Mac/Linux platform.
step1: insert that code to main file
```
package.cpath = package.cpath .. ";/Users/andrewstarks/.vscode/extensions/tangzx.emmylua-0.3.28/debugger/emmy/mac/emmy_core.dylib"
local dbg = require("emmy_core")
dbg.tcpListen("localhost", 9966)
```
step2: launch lua application, make sure the inserted code is executed and no error.
step3: launch debugger in the vscode side, it should work

# 二、现象描述

调试简单的Lua没问题，直接```./lua mylua.lua```
```lua

-- package.cpath = package.cpath .. ";/Users/xbpiao/.vscode/extensions/tangzx.emmylua-0.3.49/debugger/emmy/mac/emmy_core.dylib"
-- package.cpath = package.cpath .. ";/Users/xbpiao/work/github/EmmyLuaDebugger/x86/emmy_core/Debug/emmy_core.53.dylib"
package.cpath = package.cpath .. ";/Users/xbpiao/work/github/EmmyLuaDebugger/x86/emmy_core/Debug/emmy_core.dylib"
print("this is test! ---------->begin")
local dbg = require("emmy_core")
print("this is test! ---------->11111111111")
if (dbg ~= nil) then
    print("dbg ~= nil")
else
    print("dbg == nil")
end

dbg.tcpListen("localhost", 9966)
print("dbg.tcpListen(\"localhost\", 9966)")
dbg.waitIDE(30000)
dbg.breakHere()

print("this is test! ---------->end")

print("this is test debug!!!!!!")

```

vscode EmmyLual默认配置：
```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "emmylua_new",
            "request": "launch",
            "name": "EmmyLua New Debug",
            "host": "localhost",
            "port": 9966,
            "ext": [
                ".lua",
                ".lua.txt",
                ".lua.bytes"
            ],
            "ideConnectDebugger": true
        }
    ]
}
```

但调试 UE4.26.1 + UnLua 一断点UE4Editor就crash，官方的[https://github.com/Tencent/UnLua](https://github.com/Tencent/UnLua) 就可以重现。

# 三、问题原因

感谢开源的 [https://github.com/EmmyLua/EmmyLuaDebugger](https://github.com/EmmyLua/EmmyLuaDebugger) 直接Debug调试一波。
发现 **Debugger::GetVariable** 无限循环递归堆栈溢出导致的crash
[https://github.com/xbpiao/EmmyLuaDebugger/blob/c37ede5580c7b5654feb735c73659ba99bf9d792/emmy_core/emmy_debugger.cpp#L242](https://github.com/xbpiao/EmmyLuaDebugger/blob/c37ede5580c7b5654feb735c73659ba99bf9d792/emmy_core/emmy_debugger.cpp#L242)

临时解决就是限制递归次数，大过年的临时限制188次

[https://github.com/xbpiao/EmmyLuaDebugger/tree/fix_DebuggerGetVariable_Infinite_loop](https://github.com/xbpiao/EmmyLuaDebugger/tree/fix_DebuggerGetVariable_Infinite_loop)

```c++
#ifndef EMMY_CORE_GETVARIABLE_LIMIT
#define EMMY_CORE_GETVARIABLE_LIMIT 188
#endif
```

简单看了下是UObject -> ParentClass -> metatable -> metatable 无限转下去喽！

由于是临时方案暂不提交 **PR** ，有空再测试Windows平台找到较完美的方案再说。

![macosx_emmylua_debug](/blog/assets/images/2021-02-12-EmmyLuaFix/macosx_emmylua_debug.jpg)

如果你懒得编译也可以下载： [emmy_core.dylib](../assets/images/2021-02-12-EmmyLuaFix/emmy_core.dylib) 

