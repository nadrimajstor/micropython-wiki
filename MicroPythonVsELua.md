## eLua features

1. Latest release of Lua is 5.2. eLua is based on Lua 5.1.4, and this fact is not widely publicized. (ref: https://github.com/elua/elua/blob/master/src/lua/lua.h) uPy is based on the latest Python release, 3.4 (Note: over time, uPy will certainly linger with updates, too).

2. eLua, with LTR patch, boasts only 5.42K RAM usage after book, down from ~25K with unpatched Lua. (ref: http://www.eluaproject.net/doc/v0.9/en_arch_ltr.html). uPy offers ~540 *bytes* usage after boot (10 times less than patched eLua), and truly can boot and accept commands with 2K heap.

3. Just a quote (http://www.eluaproject.net/doc/v0.9/en_elua_egc.html):
> The EGC (Emergency Garbage Collector) patch [...], what it does is that it lets you run a garbage collection cycle in Lua in a low memory situation, from inside Lua's memory allocation function (something that the current version of Lua can't do out of the box).

That's unheard of. In uPy, garbage collection exactly runs when memory allocator can't find enough memory, as a native feature. Note that by "current version of Lua" the previous version of Lua is meant (see above). 5.2 appear to have native support for what most people do consider garbage collection in the first place.

## Lua language (mis)features.

1. Doesn't error out on usage on invalid and non-existent variables, returns legitimate value for such.
1. Numeric type is single, forced float. Inefficient w/o floating-point hardware (eLua works around by offering integer-only version, which then cannot support float). Problem with bitwise logical operations. Etc, etc.
1. Container type is single, associative array (called "table"). (Latest version of Lua work that around by providing dynamic array-vs-map switching underneath.) Multiple issues due to this, like array indexing starting from arbitrary number rather than zero, "length" of table not always corresponding to number of elements in it, etc.
1. Funky OO syntax, like obj:close().
1. No exceptions, instead pseudo-functional workaround, pcall() and friends. (Also goto in Lua 5.2)
