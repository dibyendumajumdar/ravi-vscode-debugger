Ravi / Lua 5.3 Debug Adapter for VSCode
=======================================

The aim is to provide a debug adapter that allows Microsoft's Visual Studio Code to step through [Ravi](http://ravilang.org) or [Lua 5.3](http://www.lua.org) code.

Implementation Notes
--------------------
The approach is to create a standalone executable that can be invoked by VSCode. VSCode communicates 
with the adapter via stdin/stdout. This means that Lua cannot use stdin/stdout - Lua output to 
stdout/stderr is captured and sent to the debugger front-end.

Status
------
This is work in progress. The basic debugger is working with following features and limitations.

* Launch a Ravi/Lua program and stop on entry
* Step through code (stepin, stepout, next all behave as stepin)
* Continue works, but pause doesn't. Note that the execution is very slow under the debugger.
* Set breakpoints at line/source level 
* Only local variables are shown in the Variables window right now; number of variables displayed is limited to 120.
* Tables are expanded to one level only - expansion limited to 120 elements
* Lua stdout and stderr are redirected to the debugger
* The debugger can step into dynamically generated Lua code
* No recognition of Ravi specific type information yet
* Has been tested briefly on Windows 10 and OSX so far
* Various hard coded limits - e.g. number of breakpoints limited to 20
* Expect lots of bugs!

Note: This is very early days and the debugger not yet ready for real use so try at your own risk!

Installation
------------
A prequisite on Mac OSX and Linux is an installation of Ravi. You need a NOJIT build that creates the 'ravidebug' executable. The 'ravidebug' executable must be on the PATH so that the VSCode Ravi Debugger extension script can find it.

You can install the Ravi Debug extension from VSCode Marketplace - just search for 'Ravi Debug'. 

Screenshots
-----------
Todo

See also
--------
[Ravi Programming Language](http://ravilang.org)