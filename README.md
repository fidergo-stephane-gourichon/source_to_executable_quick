# A generic script for one-step compile and install of a CMake-based source code tree.

## Need: a generic tool to quickly turn *any* CMake-based source code tree into an executable without manual step.

You're a programmer and often get source code (github, tar.gz,
whatever) to try.

Here we focus on software that uses `CMake` for its build process
(generally C or C++ programs).

## Situation before/without this tool

Whenever you get any `CMake`-based source tree from the net, `CMake`
does the heavy lifting, yet some steps remain:

1. **manually** create up a build directory, (if out-of-source-build
   is wished, which is always good practice) which implies...

2. ...**manually** choose a name for build and install directories
   depending on some factors, like the operating system used to
   compile (any experienced Linux user knows what happens when using
   binaries made for another distribution or release).

3. **manually** do a **configure** step,

4. **manually** set up an install directory (if installing without
   root privilege, which in some case is not even possible and even
   when it is, needs password prompting)

## Why this situation is normal (and not a CMake limitation).

It makes sense that `CMake` keeps those steps manual because it is a
generic tool, and there's no generic answer to those steps.

But in your context, you probably have *your* usual answers to what
`CMake` needs.  So, why not write those answers once and for all?

## Situation after/with this tool

1. Call the script, it does all the steps above so you don't have to.
   You just get your software runnable.

2. Profit!

### Extra benefits

* No downside, script is just a wrapper to the initial `CMake` call.
  You can do what you wish of the resulting tree.

* Fully scriptable, you can also provide additional configure-time
  argument to `CMake` if needed, in the form of additional arguments
  to the script.

#### OS name and version marker

Build and install tree are named based on OS name and version.

This means that there's never any doubt about which operating system a
particular build/install directory is targetting.

1. This averts the scenario of running old build/install trees in newer
versions of distributions, which too often results in:

    * missing library at run time (because distribution upgraded to a
	  newer binary-incompatible library, as binary linking is less
	  flexible than compilation-time configuration),

    * and/or crashes due to subtle library breakage.

2. This allows to easily find and purge versions for old OSes.

<pre>
find /mystorage -iname "*.OSID_myoldOS.*tree" -print0 | xargs -0 rm -rf
</pre>

Also, OS-marks are compatible with multi-architecture scripts.  You
can for example compute OS_ID in another script and refer e.g. to:

<pre>
/path/to/some-tool.OSID_${OS_ID}.installtree/bin/awesometool
</pre>

and expect it to do the right thing.


If you change OS, just re-run the script on your new OS and
[bam it works!](http://www.smbc-comics.com/comic/2011-02-17)

## Virtual FAQ

### This projects seems highly tuned to the author's context, it can't help me, right?

So is most free software.  Even more when people share their `.emacs` configuration, etc.

But you can fork this repo and adapt the script to your local context!

## Okay, show me the code

The script is self-contained and self-explanatory, see
[cmake_project_bootstrap.sh](cmake_project_bootstrap.sh).
