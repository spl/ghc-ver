This script allows you to easily switch between different versions of GHC (the Glasgow Haskell
Compiler) that are installed on your system. It's a simple Bash script that manipulates a
symlink pointing to a subdirectory with the current preferred version of the GHC binaries.

Configuring
-----------

1. Install the [Haskell Platform](http://hackage.haskell.org/platform/) (HP) for Mac OS X.

   The HP creates a directory `/Library/Frameworks/GHC.framework` with all the tools needed
   to use Haskell. In particular, the `Versions` subdirectory contains the GHC installation.
   Each future installation of HP adds a new version in a subdirectory of `Versions`.

   In `Versions`, there is a symlink `Current` that points to the subdirectory with the
   "current" GHC. We use that to determine which GHC we want to use at any one point in time.

2. Update the HP symlinks in `/usr/bin` to use `Current`.

   To make the HP tools part of the standard path, the HP installer creates symlinks in
   `/usr/bin` that point to the binaries in the current version. By default, they may link to
   the specific subdirectory in `Versions`. You should relink them, replacing the specific
   version with `Current`.

   You can find the current symlinks with `ls -l /usr/bin/ | grep GHC\.framework`. For
   example, I see `ghc`, `ghc-pkg`, `ghci`, `hp2ps`, `hpc`, `hsc2hs`, `runghc`, and
   `runhaskell`, among others. There may also be version-specific ones like `ghc-7.4.1` which
   you may want to delete, since they will not be applicable if you change your current
   version.

   For each symlink `$F` from above, recreate it with
   
        sudo ln -sf /Library/Frameworks/GHC.framework/Versions/Current/usr/bin/$F /usr/bin/$F

3. Put `ghc-ver` in your `$PATH`.

And you're done!

Using
-----

* Use `ghc-ver list` to see the versions available.

  It simply looks at the subdirectories in `/Library/Frameworks/GHC.framework/Versions` and
  gives you the list of names. Thus, it helps to name the versions appropriately and
  concisely.

* Use `ghc-ver 7.4.1` to select version `7.4.1` if it is available. You will get a notice if
  it is not, and you will also get a confirmation of the current GHC version, which should
  be unchanged in the case of failure.

* Install any future versions of the HP as normal. Unless the installer changes, you should
  just be able to switch to the new version using `ghc-ver`. (You may need to update the
  `/usr/bin` symlinks as described above.)

* Install any non-HP versions of GHC in `/Library/Frameworks/GHC.framework/Versions`.

  In the case where you are compiling GHC from source, say the version is called `HEAD`, you
  can create the directory `/Library/Frameworks/GHC.framework/Versions/HEAD` and configure
  GHC as follows:

        ./configure \
        --prefix=/Library/Frameworks/GHC.framework/Versions/HEAD/usr \
        --with-gmp-libraries=/Library/Frameworks/GMP.framework        \
        --with-gmp-includes=/Library/Frameworks/GMP.framework/Headers

  Then, `sudo make install` will do the right thing.

  You may or may not need the extra `GMP.framework` flags. There have be times in the past
  where GMP did not work properly when compiling GHC from source.