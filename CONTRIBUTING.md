Code Conventions
----------

When contributing, you must follow our code conventions. Otherwise, the CI builds will automatically fail and your PR will not be merged until the non-conforming code is fixed. Due to this, we strongly advise you to run `src/CheckBasicStyle.lua` before commiting, it will perform various code style checks and warn you if your code does not conform to our conventions. `CheckBasicStyle.lua` can be configured to run automatically before every commit via a pre-commit hook, **this is highly recommended**. The way to do it is listed at the bottom of this file.

Here are the conventions:

 * We use the subset of C++11 supported by MSVC 2013 (ask if you think that something would be useful)
 * All new public functions in all classes need documenting comments on what they do and what behavior they follow, use doxy-comments formatted as `/** Description */`. Do not use asterisks on additional lines in multi-line comments.
 * Use spaces after the comment markers: `// Comment` instead of `//Comment`. A comment must be prefixed with two spaces if it's on the same line with code:
  - `SomeFunction()<Space><Space>//<Space>Note the two spaces prefixed to me and the space after the slashes.`
 * All variable names and function names use CamelCase style, with the exception of single letter variables.  
  - `ThisIsAProperFunction()` `This_is_bad()` `this_is_bad` `GoodVariableName` `badVariableName`.
 * All member variables start with `m_`, all function parameters start with `a_`, all class names start with `c`.
  - `class cMonster { int m_Health; int DecreaseHealth(int a_Amount); }`
 * Put spaces after commas. `Vector3d(1, 2, 3)` instead of `Vector3d(1,2,3)`
 * Put spaces before and after every operator.
  - `a = b + c;`
  - `if (a == b)`
 * Keep individual functions spaced out by 5 empty lines, this enhances readability and makes navigation in the source file easier.
 * Add those extra parentheses to conditions, especially in C++:
  - `if ((a == 1) && ((b == 2) || (c == 3)))` instead of ambiguous `if (a == 1 && b == 2 || c == 3)`
  - This helps prevent mistakes such as `if (a & 1 == 0)`
 * 
 * Use the provided wrappers for OS stuff:
  - Threading is done by inheriting from `cIsThread`, thread synchronization through `cCriticalSection`, `cSemaphore` and `cEvent`, file access and filesystem operations through the `cFile` class, high-precision timers through `cTimer`, high-precision sleep through `cSleep`
 * No magic numbers, use named constants:
  - `E_ITEM_XXX`, `E_BLOCK_XXX` and `E_META_XXX` for items and blocks
  - `cEntity::etXXX` for entity types, `cMonster::mtXXX` for mob types
  - `dimNether`, `dimOverworld` and `dimEnd` for world dimension
  - `gmSurvival`, `gmCreative`, `gmAdventure` for game modes
  - `wSunny`, `wRain`, `wThunderstorm` for weather
  - `cChunkDef::Width`, `cChunkDef::Height` for chunk dimensions (C++)
  - etc.
 * Instead of checking for a specific value, use an `IsXXX` function, if available:
  - `cPlayer:IsGameModeCreative()` instead of` (cPlayer:GetGameMode() == gmCreative)` (the player can also inherit the gamemode from the world, which the value-d condition doesn't catch)
 * Please use **tabs for indentation and spaces for alignment**. This means that if it's at line start, it's a tab; if it's in the middle of a line, it's a space
 * Alpha-sort stuff that makes sense alpha-sorting - long lists of similar items etc.
 * White space is free, so use it freely
  - "freely" as in "plentifully", not "arbitrarily"
 * All `case` statements inside a `switch` need an extra indent.
 * Each and every control statement deserves its braces. This helps maintainability later on when the file is edited, lines added or removed - the control logic doesn't break so easily.
  - The only exception: a `switch` statement with all `case` statements being a single short statement is allowed to use the short brace-less form.
  - These two rules really mean that indent is governed by braces
 * Add an empty last line in all source files (GCC and GIT can complain otherwise)

Pre-commit hook
---------
When contributing, the code conventions above *must* be followed. Otherwise, the CI builds will automatically fail and your PR will not be merged until the non-conforming code is fixed. It is highly recommended to set up a pre-commit hook which will check your code style before every commit. Here is how to do that:

 * Clone the repository as usual. 
 * Go to your `<clone location>/.git/hooks` folder, create a text file named "pre-commit" there with the following contents:
```
#!/usr/sh
src/CheckBasicStyle.lua 1>&2 -g
```
 * If on Linux/Unix, you need to give the newly created file an execute permission: `chmod +x .git/hooks/pre-commit`
 * Lua must be installed.
 * You're done. Now, `src/CheckBasicStyle.lua` will check the changed files before every commit. If a problem is found, it will point you to that problem and will cancel the commit.
 
Note that the check script is not smart enough to catch everything, so not having any warnings does not necessarily imply that you followed the conventions fully. The other humans working on this will perform more checks before merging.

Copyright
---------

Your work must be licensed at least under the Apache license.

You can add yourself to the CONTRIBUTORS file if you wish.

**PLUGINS ONLY**: If your plugin is not licensed under the Apache license, then it must be compatible and marked as such. This is only valid for the plugins included within the MCServer source; plugins developed on separate repositories can use whatever license they want.
