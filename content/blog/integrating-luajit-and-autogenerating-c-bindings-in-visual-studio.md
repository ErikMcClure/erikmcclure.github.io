+++
blogimport = true
categories = ["blog"]
date = "2017-07-24T23:15:00Z"
title = "Integrating LuaJIT and Autogenerating C Bindings In Visual Studio"
updated = "2017-08-24T01:07:03.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
[Lua](https://www.lua.org/) is a popular scripting language due to its tight integration with C. [LuaJIT](http://luajit.org/) is an extremely fast JIT compiler for Lua that can be integrated into your game, which also provides an [FFI Library](http://luajit.org/ext_ffi.html) that directly interfaces with C functions, eliminating most overhead. However, the FFI library only accepts a subset of the C standard. Specifically, **"C declarations are not passed through a C pre-processor, yet. No pre-processor tokens are allowed, except for #pragma pack."** The website suggests running the header file through a preprocesser stage, but I have yet to find a LuaJIT tutorial that actually explains how to do this. Instead, all the examples simply copy+paste the function prototype into the Lua file itself. Doing this with makefiles and GCC is trivial, because you just have to add a compile step using <a href="https://gcc.gnu.org/onlinedocs/gcc-4.9.1/gcc/Preprocessor-Options.html">the {{<code>}}-E{{</code>}} option</a>, but integrating this with Visual Studio is more difficult. In addition, I'll show you how to properly load scripts and modify the PATH lookup variable so your game can have a proper {{<code>}}scripts{{</code>}} folder instead of dumping everything in {{<code>}}bin{{</code>}}.

##### Compilation
To begin, we need to [download LuaJIT](http://luajit.org/download.html) and get it to actually compile. Doing this manually isn't too difficult, simply open an x64 Native Tools Command Prompt (or x86 Native Tools if you want 32-bit), navigate to {{<code>}}src/msvcbuild.bat{{</code>}} and run {{<code>}}msvcbuild.bat{{</code>}}. The default options will build an x64 or x86 dll with dynamic linking to the CRT. If you want a static lib file, you need to run it with the {{<code>}}static{{</code>}} option. If you want static linking to the CRT so you don't have to deal with that annoying Visual Studio Runtime Library crap, you'll have to modify the .bat file directly. Specifically, you need to find {{<code>}}%LJCOMPILE% /MD{{</code>}} and change it to {{<code>}}%LJCOMPILE% /MT{{</code>}}. This will then compile the static lib or dll with static CRT linking to match your other projects.

This is a bit of a pain, and recently I've been trying to automate my build process and dependencies using [vcpkg](https://github.com/Microsoft/vcpkg) to act as a C++ package manager. A port of LuaJIT is included in the latest update of vcpkg, but if you want one that always statically links to the CRT, you can [get it here](https://github.com/Black-Sphere-Studios/vcpkg/tree/master/ports/luajit).

An important note: the build instructions for LuaJIT state that you should copy the lua scripts contained in {{<code>}}src/jit{{</code>}} to your application folder. What it doesn't mention is that this is *optional* - those scripts contain debugging instructions for the JIT engine, which you probably don't need. It will work just fine without them.

Once you have LuaJIT built, you should add it's library file to your project. This library file is called **lua51.lib** (and the dll is **lua51.dll**), because LuaJIT is designed as a drop-in replacement for the default Lua runtime. Now we need to actually load Lua in our program and integrate it with our code. To do this, use {{<code>}}lua_open(){{</code>}}, which returns a {{<code>}}lua_State*{{</code>}} pointer. You will need that {{<code>}}lua_State*{{</code>}} pointer for everything else you do, so store it somewhere easy to get to. If you are building a game using an Entity Component System, it makes sense to build a {{<code>}}LuaSystem{{</code>}} that stores your {{<code>}}lua_State*{{</code>}} pointer.

##### Initialization
The next step is to load in all the standard Lua libraries using {{<code>}}luaL_openlibs(L){{</code>}}. Normally, you shouldn't do this if you need script sandboxing for player-created scripts. However, **LuaJIT's FFI library is inherently unsafe**. Any script with access to the FFI library can call any kernel API it wants, so you should be extremely careful about using LuaJIT if this is a use-case for your game. We can also register any C functions we want to the old-fashioned way via {{<code>}}lua_register{{</code>}}, but this is only useful for functions that don't have C analogues (due to having multiple return values, etc).

There is one function in particular that you probably want to overload, and that is the {{<code>}}print(){{</code>}} function. By default, Lua will simply print to standard out, but if you aren't redirecting standard out to your in-game console, you probably have your own {{<code>}}std::ostream{{</code>}} (or even a custom stream class) that is sent all log messages. By overloading {{<code>}}print(){{</code>}}, we can have our Lua scripts automatically write to both our log file and our in-game console, which is extremely useful. Here is a complete re-implementation of {{<code>}}print{{</code>}} that outputs to an arbitrary {{<code>}}std::ostream&{{</code>}} object:
{{<pre cpp>}}int PrintOut(lua_State *L, std::ostream& out)
{
  int n = lua_gettop(L);  /* number of arguments */
  if(!n)
    return 0;
  int i;
  lua_getglobal(L, "tostring");
  for(i = 1; i <= n; i++)
  {
    const char *s;
    lua_pushvalue(L, -1);  /* function to be called */
    lua_pushvalue(L, i);   /* value to print */
    lua_call(L, 1, 1);
    s = lua_tostring(L, -1);  /* get result */
    if(s == NULL)
      return luaL_error(L, LUA_QL("tostring") " must return a string to "
        LUA_QL("print"));
    if(i > 1) out << "\t";
    out << s;
    lua_pop(L, 1);  /* pop result */
  }
  out << std::endl;
  return 0;
}
{{</pre>}}To overwrite the existing {{<code>}}print{{</code>}} function, we need to first define a Lua compatible shim function. In this example, I pass {{<code>}}std::cout{{</code>}} as the target stream:

{{<pre cpp>}}int lua_Print(lua_State *L)
{
  return PrintOut(L, std::cout);
}
{{</pre>}}Now we simply register our {{<code>}}lua_Print{{</code>}} function using {{<code>}}lua_register(L, "print", &lua_Print){{</code>}}. If we were doing this in a {{<code>}}LuaSystem{{</code>}} object, our constructor would look like this:

{{<pre cpp>}}LuaSystem::LuaSystem()
{
  L = lua_open();
  luaL_openlibs(L);
  lua_register(L, "print", &lua_Print);
}
{{</pre>}}To clean up our Lua instance, we need to both trigger a final GC iteration to clean up any dangling memory, and then we call {{<code>}}lua_close(L){{</code>}}, so our destructor would look like this:
{{<pre cpp>}}LuaSystem::~LuaSystem()
{
  lua_gc(L, LUA_GCCOLLECT, 0);
  lua_close(L);
  L = 0;
}
{{</pre>}}
##### Loadings Scripts via Require
At this point most tutorials skip to the part where you load a Lua script and write "Hello World", but we aren't done yet. Integrating Lua into your game means loading scripts and/or arbitrary strings as Lua code while properly resolving dependencies. If you don't do this, any one of your scripts that relies on another script will have to do {{<code>}}require("full/path/to/script.lua"){{</code>}}. We also face another problem - if we want to have a {{<code>}}scripts{{</code>}} folder where we simply automatically load every single script into our workspace, simply loading them all can cause duplicated code, because {{<code>}}luaL_loadfile{{</code>}} does *not* have any knowledge of {{<code>}}require{{</code>}}. You can solve this by simply loading a single {{<code>}}bootstrap.lua{{</code>}} script which then loads all your game's scripts via {{<code>}}require{{</code>}}, but we're going to build a much more robust solution.

First, we need to modify Lua's {{<code>}}PATH{{</code>}} variable, or the variable that controls where it looks up scripts relative to our current directory. This function will append a path (which should be of the form {{<code>}}"path/to/scripts/?.lua"{{</code>}}) to the beginning of the {{<code>}}PATH{{</code>}} variable, giving it highest priority, which you can then use to add as many {{<code>}}script{{</code>}} directories as you want in your game, and any lua script from any of those folders will then be able to {{<code>}}require(){{</code>}} a script from any other folder in {{<code>}}PATH{{</code>}} without a problem. Obviously, you should probably only add one or two folders, because you don't want to deal with potential name conflicts in your script files.
{{<pre cpp>}}int AppendPath(lua_State *L, const char* path)
{
  lua_getglobal(L, "package");
  lua_getfield(L, -1, "path"); // get field "path" from table at top of stack (-1)
  std::string npath = path;
  npath.append(";");
  npath.append(lua_tostring(L, -1)); // grab path string from top of stack
  lua_pop(L, 1);
  lua_pushstring(L, npath.c_str());
  lua_setfield(L, -2, "path"); // set the field "path" in table at -2 with value at top of stack
  lua_pop(L, 1); // get rid of package table from top of stack
  return 0;
}
{{</pre>}}Next, we need a way to load all of our scripts using {{<code>}}require(){{</code>}} so that Lua properly resolves the dependencies. To do this, we create a function in C that literally calls the {{<code>}}require(){{</code>}} function for us:
{{<pre cpp>}}int Require(lua_State *L,const char *name)
{
  lua_getglobal(L, "require");
  lua_pushstring(L, name);
  int r = lua_pcall(L, 1, 1, 0);
  if(!r)
    lua_pop(L, 1);
  WriteError(L, r, std::cout);
  return r;
}
{{</pre>}}By using this to load all our scripts, we don't have to worry about loading them in any particular order - {{<code>}}require{{</code>}} will ensure everything gets loaded correctly. An important note here is {{<code>}}WriteError(){{</code>}}, which is a generic error handling function that processes Lua errors and writes them to a log. All errors in lua will return a nonzero error code, and will *usually* push a string containing the error message to the stack, which must then be popped off, or it'll mess things up later.
{{<pre cpp>}}void WriteError(lua_State *L, int r, std::ostream& out)
{
  if(!r)
    return;
  if(!lua_isnil(L, -1)) // Check if a string was pushed
  {
    const char* m = lua_tostring(L, -1);
    out << "Error " << r << ": " << m << std::endl;
    lua_pop(L, 1);
  }
  else
    out << "Error " << r << std::endl;
}
{{</pre>}}
##### Automatic C Binding Generation
Fantastic, now we're all set to load up our scripts, but we still need to somehow define a header file and also load that header file into LuaJIT's FFI library so our scripts have direct access to our program's exposed C functions. One way to do this is to just copy+paste your C function definitions into a Lua file in your scripts folder that is then automatically loaded. This, however, is a pain in the butt and is error-prone. We want to have a *single source of truth* for our function definitions, which means defining our entire LuaJIT C API in a single header file, which is then loaded directly into LuaJIT. Predictably, we will accomplish this by abusing the C preprocessor:

{{<pre cpp>}}#ifndef __LUA_API_H__
#define __LUA_API_H__

#ifndef LUA_EXPORTS
#define LUAFUNC(ret, name, ...) ffi.cdef[[ ret lua_##name(__VA_ARGS__); ]]; name = ffi.C.lua_##name
local ffi = require("ffi")
ffi.cdef[[ // Initial struct definitions
#else
#define LUAFUNC(ret, name, ...) ret __declspec(dllexport) lua_##name(__VA_ARGS__)
extern "C" { // Ensure C linkage is being used
#endif

struct GameInfo
{
  uint64_t DashTail;
  uint64_t MaxDash;
};

typedef const char* CSTRING; // VC++ complains about having const char* in macros, so we typedef it here

#ifndef LUA_EXPORTS
]] // End struct definitions
#endif

  LUAFUNC(CSTRING, GetGameName);
  LUAFUNC(CSTRING, IntToString, int);
  LUAFUNC(void, setdeadzone, float);

#ifdef Everglade_EXPORTS
}
#endif

#endif
{{</pre>}}The key idea here is to use macros such that, when we pass this through the preprocessor *without* any predefined constants, it will magically turn into a valid Lua script. However, when we compile it in our C++ project, our project defines {{<code>}}LUA_EXPORTS{{</code>}}, and the result is a valid C header. Our C {{<code>}}LUAFUNC{{</code>}} is set up so that we're using C linkage for our structs and functions, and that we're *exporting the function* via {{<code>}}__declspec(dllexport){{</code>}}. This obviously only works for Visual Studio so you'll want to set up a macro for the GCC version, but I will warn you that VC++ got really cranky when i tried to use a macro for that in my code, so you may end up having to redefine the entire {{<code>}}LUAFUNC{{</code>}} macro for each compiler.

At this point, we have a bit of a choice to make. It's more convenient to have the C functions available in the global namespace, which is what this script does, because this simplifies calling them from an interactive console. However, **using {{<code>}}ffi.C.FunctionName{{</code>}} is significantly faster**. Technically the fastest way is declaring {{<code>}}local C = ffi.C{{</code>}} at the top of a file and then calling the functions via {{<code>}}C.FunctionName{{</code>}}. Luckily, importing the functions into the global namespace does not preclude us from using the "fast" way of calling them, so our script here imports them into the global namespace for ease of use, but in our scripts we can use the {{<code>}}C.FunctionName{{</code>}} method instead. Thus, when outputting our Lua script, our {{<code>}}LUAFUNC{{</code>}} macro wraps our function definition in a LuaJIT {{<code>}}ffi.cdef{{</code>}} block, and then runs a *second* Lua statement that brings the function into the global namespace. This is why we have an initial {{<code>}}ffi.cdef{{</code>}} code block for the structs up top, so we can include that second lua statement after each function definition.

Now we need to set up our compilation so that Visual Studio generates this file without any predefined constants and outputs the resulting lua script to our {{<code>}}scripts{{</code>}} folder, where our other in-game scripts can automatically load it from. We can accomplish this using a Post-Build Event (under Configuration Properties -> Build Events -> Post-Build Event), which then runs the following code:
{{<pre>}}CL LuaAPI.h /P /EP /u
COPY "LuaAPI.i" "../bin/your/script/folder/LuaAPI.lua" /Y
{{</pre>}}Visual Studio can sometimes be finicky about that newline, but if you put in two statements on two separate lines, it should run both commands sequentially. You may have to edit the project file directly to convince it to actually do this. The key line here is {{<code>}}CL LuaAPI.h /P /EP /u{{</code>}}, which tells the compiler to preprocess the file and output it to a {{<code>}}*.i{{</code>}} file. There is no option to configure the output file, it will always be the exact same file but with a {{<code>}}.i{{</code>}} extension, so we have to copy and rename it ourselves to our scripts folder using the {{<code>}}COPY{{</code>}} command.

##### Loading and Calling Lua Code
We are now set to load all our lua scripts in our script folder via {{<code>}}Require{{</code>}}, but what if we want an interactive Lua console? There are lua functions that read strings, but to make this simpler, I will provide a function that loads a lua script from an arbitrary {{<code>}}std::istream{{</code>}} and outputs to an arbitrary {{<code>}}std::ostream{{</code>}}:
{{<pre cpp>}}const char* _luaStreamReader(lua_State *L, void *data, size_t *size)
{
  static char buf[CHUNKSIZE];
  reinterpret_cast<std::istream*>(data)->read(buf, CHUNKSIZE);
  *size = reinterpret_cast<std::istream*>(data)->gcount();
  return buf;
}

int Load(lua_State *L, std::istream& s, std::ostream& out)
{
  int r = lua_load(L, &_luaStreamReader, &s, 0);

  if(!r)
  {
    r = lua_pcall(L, 0, LUA_MULTRET, 0);
    if(!r)
      PrintOut(L, out);
  }

  WriteError(L, r, out);
  return r;
}
{{</pre>}}
Of course, the other question is how to call Lua functions from our C++ code directly. There are many, *many* different implementations of this available, of varying amounts of safety and completeness, but to get you started, here is a very simple implementation in C++ using templates. Note that this does not handle errors - you can change it to use {{<code>}}lua_pcall{{</code>}} and check the return code, but [handling arbitrary Lua errors is nontrivial](http://www.knightsgame.org.uk/blog/2012/09/03/notes-on-luac-error-handling/).
{{<pre cpp>}}template<class T, int N>
struct LuaStack;

template<class T> // Integers
struct LuaStack<T, 1>
{
  static inline void Push(lua_State *L, T i) { lua_pushinteger(L, static_cast<lua_Integer>(i)); }
  static inline T Pop(lua_State *L) { T r = (T)lua_tointeger(L, -1); lua_pop(L, 1); return r; }
};
template<class T> // Pointers
struct LuaStack<T, 2>
{
  static inline void Push(lua_State *L, T p) { lua_pushlightuserdata(L, (void*)p); }
  static inline T Pop(lua_State *L) { T r = (T)lua_touserdata(L, -1); lua_pop(L, 1); return r; }
};
template<class T> // Floats
struct LuaStack<T, 3>
{
  static inline void Push(lua_State *L, T n) { lua_pushnumber(L, static_cast<lua_Number>(n)); }
  static inline T Pop(lua_State *L) { T r = static_cast<T>(lua_touserdata(L, -1)); lua_pop(L, 1); return r; }
};
template<> // Strings
struct LuaStack<std::string, 0>
{
  static inline void Push(lua_State *L, std::string s) { lua_pushlstring(L, s.c_str(), s.size()); }
  static inline std::string Pop(lua_State *L) { size_t sz; const char* s = lua_tolstring(L, -1, &sz); std::string r(s, sz); lua_pop(L, 1); return r; }
};
template<> // Boolean
struct LuaStack<bool, 1>
{
  static inline void Push(lua_State *L, bool b) { lua_pushboolean(L, b); }
  static inline bool Pop(lua_State *L) { bool r = lua_toboolean(L, -1); lua_pop(L, 1); return r; }
};
template<> // Void return type
struct LuaStack<void, 0> { static inline void Pop(lua_State *L) { } };

template<typename T>
struct LS : std::integral_constant<int, 
  std::is_integral<T>::value + 
  (std::is_pointer<T>::value * 2) + 
  (std::is_floating_point<T>::value * 3)>
{};

template<typename R, int N, typename Arg, typename... Args>
inline R _callLua(const char* function, Arg arg, Args... args)
{
  LuaStack<Arg, LS<Arg>::value>::Push(_l, arg);
  return _callLua<R, N, Args...>(function, args...);
}
template<typename R, int N>
inline R _callLua(const char* function)
{
  lua_call(_l, N, std::is_void<R>::value ? 0 : 1);
  return LuaStack<R, LS<R>::value>::Pop(_l);
}

template<typename R, typename... Args>
inline R CallLua(lua_State *L, const char* function, Args... args)
{
  lua_getglobal(L, function);
  return _callLua<R, sizeof...(Args), Args...>(L, function, args...);
}
{{</pre>}}Now you have everything you need for an extensible Lua scripting implementation for your game engine, and even an interactive Lua console, all using LuaJIT. Good Luck!
