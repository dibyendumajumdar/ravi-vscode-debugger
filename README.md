Lua and Ravi 5.3 Debug Adapter for VSCode
=========================================

This project aims to provide a debug adapter that allows Microsoft's Visual Studio Code to step through [Ravi](http://ravilang.org) or [Lua 5.3](http://www.lua.org) code.

![Debugger in Action](images/screenshot0.png)

Status
------
This is work in progress. The basic debugger is working with following features and limitations.

* Launch a Ravi/Lua 5.3 script and stop on entry 
* Step through code
* Set breakpoints at line/source level (upto a max of 20 breakpoints)
* Local variables and variable arguments are shown in the Variables window; number of variables displayed is limited to 250
* Tables are expanded to first few levels only - expansion limited to 250 elements
* Lua stdout and stderr are redirected to the debugger
* The debugger can step into dynamically generated Lua code
* Support limited to x86-64 Windows 10, Ubuntu and OSX platforms
* Unlike other Lua debuggers this implementation uses a custom Ravi VM, with extensions to support debugging
* Various limitations - see list below

Note: This is work in progress so please expect bugs!

Installation
------------
You can install the Ravi Debug extension from VSCode Marketplace - search for 'Lua and Ravi 5.3 Debugger'. 
Pre-built X86 64-bit binaries for Windows 10, OS X Mojave, and Ubuntu 14.04 are included in the installation - 
note that these do not have JIT enabled.

Issues and Limitations
----------------------
* The 'pause' function does not work, i.e. you cannot pause a running script
* In all cases the number of variables or table entries displayed is truncated to 250 items
* The stack trace is truncated to 50 items
* Only upto 20 breakpoints can be set
* You cannot set a breakpoint against a dynamically generated Lua function 
* You may not be able to debug Lua 5.0, 5.1 or 5.2 scripts - only 5.3 is tested and officially supported
* Table variables can only be expanded to the first few levels
* You cannot attach to a script already running somewhere
* When the debugger runs the Lua Hook is disabled - so the running script is not allowed to modify the hook
* Evaluating expressions is not supported
* You cannot modify the variables displayed - the values are readonly
* If you amend/edit the script being debugged it will not be recognised in the debug session so you will need to start a new session to recognise changes
* Using `LUA_PATH` and `LUA_CPATH` has been tested briefly. When using `LUA_CPATH` you need to ensure that the native shared libraries are available on the system path; otherwise library dependencies can fail to be loaded.

Recent changes
--------------
* Jul 2021: Updated to fix breaking change in VS Code Debug extension for breakpoints in 1.53 (breaking change in 1.42). Also revised the mechanism for supplying arguments to the script.
* Jul 2020: Updated Ravi to 1.0 Beta4a
* Dec 2019: Updated Ravi to latest version, added support for arguments to launch script
* Feb 2017: Since VSCode 1.8 there is a bug in VSCode that messes up output to stdout - added a workaround for this bug. Details of bug are at [issue #17526](https://github.com/Microsoft/vscode/issues/17526)
* Feb 2017: The debugger is now synced with Lua 5.3.4
* Oct 2016: Added support for expanding tables to more than 1 level but still limited to first 4 or 5 levels
* Oct 2016: Display of variable types - if __name metamethod is defined for user defined types then this is used
* Oct 2016: Display of variable contents - if __tostring metamethod is defined for user defined types then this is used
* Oct 2016: Variable type names are passed to VSCode so that these get displayed by the UI

Getting Started
---------------
Once you have installed the extension, you will need to setup launch.json as shown in the steps below.

![First Launch](images/screenshot1.png)

![Select Ravi Debugger](images/screenshot2.png)

![Configure launch.json](images/screenshot3.png)

See also
--------
[Ravi Programming Language](http://ravilang.org)

Implementation Notes
--------------------
The debugger is a standalone executable that can be invoked by VSCode. VSCode communicates 
with the debugger via stdin/stdout as per the VSCode debug adapter API which uses 
JSON messages exchanged between VSCode and the debugger. 
