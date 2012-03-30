This script allows you to easily switch between different versions of GHC (the
Glasgow Haskell Compiler) that are installed on your system.

1. Install the Haskell Platform (HP)

2. Change the HP-related softlinks in `/usr/bin` to point to locations in
   `/Library/Frameworks/GHC.framework/Versions/Current/usr/bin`, e.g.
   `/usr/bin/ghc` points to `/Library/Frameworks/GHC.framework/Versions/Current/usr/bin/ghc`.

3. Install any future versions of the HP or GHC (without the HP) in
   `/Library/Frameworks/GHC.framework/Versions`.

4. Use `ghc-ver list` to see the versions available.

5. Use `ghc-ver 7.4.1` to select version `7.4.1` if it is available.

