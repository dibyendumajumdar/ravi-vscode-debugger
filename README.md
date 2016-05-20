Lua and Ravi 5.3 Debug Adapter for VSCode
=========================================

The aim is to provide a debug adapter that allows Microsoft's Visual Studio Code to step through [Ravi](http://ravilang.org) or [Lua 5.3](http://www.lua.org) code.

![Debugger in Action](images/screenshot0.png)

Implementation Notes
--------------------
The approach is to create a standalone executable that can be invoked by VSCode. VSCode communicates 
with the adapter via stdin/stdout. To prevent conflict with Lua, all Lua output to 
stdout/stderr is captured and sent to the debugger front-end.

Status
------
This is work in progress. The basic debugger is working with following features and limitations.

* Launch a Ravi/Lua 5.3 program and stop on entry (note that Lua 5.1 and 5.2 are not supported)
* Step through code (stepin, stepout, next all behave as stepin)
* Continue works, but pause doesn't
* Set breakpoints at line/source level (upto a max of 20 breakpoints)
* Only local variables and variable arguments are shown in the Variables window right now; number of variables displayed is limited to 120
* Tables are expanded to one level only - expansion limited to 120 elements
* Lua stdout and stderr are redirected to the debugger
* The debugger can step into dynamically generated Lua code
* No recognition of Ravi specific type information yet
* Has been tested briefly on Windows 10 and OSX so far
* Various hard coded limits - e.g. number of breakpoints limited to 20
* Expect bugs!

Note: This is very early days and the debugger not yet ready for real use so try at your own risk!

Installation on Mac OSX
-----------------------
Status: Not working as the vsce packaging does not set the permission on shell script correctly. When you try to run the debugger you will see the error `spawn EACCES`. A workaround is to manually change the permission of the `ravidebug.sh` script located in the [extensions folder](https://code.visualstudio.com/docs/extensions/install-extension#_your-extensions-folder). 

A prequisite on Mac OSX is an installation of Ravi. You need a `NOJIT` build that creates the `ravidebug` executable. The `ravidebug` executable must be on the PATH so that the VSCode Ravi Debugger extension script can find it.

Installation on Windows 10
--------------------------
You can install the Ravi Debug extension from VSCode Marketplace - search for 'Lua and Ravi 5.3 Debugger'. A pre-built binary is included in the installation - note that this does not have JIT enabled. 

Issues
------
* The installation on OSX does not set the permissions on scripts correctly

Getting Started
---------------
Once you have installed the extension, you will need to setup launch.json as shown in the steps below.

![First Launch](images/screenshot1.png)

![Select Ravi Debugger](images/screenshot2.png)

![Configure launch.json](images/screenshot3.png)

See also
--------
[Ravi Programming Language](http://ravilang.org)
