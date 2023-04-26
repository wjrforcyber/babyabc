[![.github/workflows/build-posix.yml](https://github.com/berkeley-abc/abc/actions/workflows/build-posix.yml/badge.svg)](https://github.com/berkeley-abc/abc/actions/workflows/build-posix.yml)
[![.github/workflows/build-windows.yml](https://github.com/berkeley-abc/abc/actions/workflows/build-windows.yml/badge.svg)](https://github.com/berkeley-abc/abc/actions/workflows/build-windows.yml)
[![.github/workflows/build-posix-cmake.yml](https://github.com/berkeley-abc/abc/actions/workflows/build-posix-cmake.yml/badge.svg)](https://github.com/berkeley-abc/abc/actions/workflows/build-posix-cmake.yml)

# babyABC: ABC kindergarten

This ABC repo is based on Berkeley's Synthesis and Verification tool [ABC](https://github.com/berkeley-abc/abc).

## New Feature
1. Use Cosine similarity to lookup similar file name. #1
    <details>
    <summary>Expand example</summary>
    </br>Old:

    ```
    UC Berkeley, ABC 1.01 (compiled Apr  3 2023 12:26:13)
    abc 01> read_aiger i
    Cannot open input file "i".
    abc 01> read_aiger i1
    Cannot open input file "i1".
    abc 01> read_aiger i10
    Cannot open input file "i10". Did you mean "i10.aig"?
    abc 01> read_aiger i1.a
    Cannot open input file "i1.a".
    abc 01> read_aiger i1.ai
    Cannot open input file "i1.ai".
    abc 01> read_aiger i1.aig
    Cannot open input file "i1.aig".
    abc 01> read_aiger 1.aig
    Cannot open input file "1.aig".
    abc 01> read_aiger i1.g
    Cannot open input file "i1.g".
    ```
    New:
    ```
    abc 01> read_aiger i
    Cannot open input file "i". Did you mean "i10.aig"?
    abc 01> read_aiger i1
    Cannot open input file "i1". Did you mean "i10.aig"?
    abc 01> read_aiger i10
    Cannot open input file "i10". Did you mean "i10.aig"?
    abc 01> read_aiger i1.a
    Cannot open input file "i1.a". Did you mean "i10.aig"?
    abc 01> read_aiger i1.ai
    Cannot open input file "i1.ai". Did you mean "i10.aig"?
    abc 01> read_aiger i1.aig
    Cannot open input file "i1.aig". Did you mean "i10.aig"?
    abc 01> read_aiger 1.aig
    Cannot open input file "1.aig". Did you mean "i10.aig"?
    abc 01> read_aiger i1.aig
    Cannot open input file "i1.aig". Did you mean "i10.aig"?
    ```
    </details>


## Commit templete
<details>
<summary>commit templete</summary>

```
# Title: Summary, imperative, start upper case, don't end with a period
# No more than 50 chars. #### 50 chars is here:  #

# Remember blank line between title and body.

# Body: Explain *what* and *why* (not *how*). Include task ID (Jira issue).
# Wrap at 72 chars. ################################## which is here:  #


# At the end: Include Co-authored-by for all contributors.
# Include at least one empty line before it. Format:
# Co-authored-by: name <user@users.noreply.github.com>
#
# How to Write a Git Commit Message:
# https://chris.beams.io/posts/git-commit/
#
# 1. Separate subject from body with a blank line
# 2. Limit the subject line to 50 characters
# 3. Capitalize the subject line
# 4. Do not end the subject line with a period
# 5. Use the imperative mood in the subject line
# 6. Wrap the body at 72 characters
# 7. Use the body to explain what and why vs. how
```

credit @lisawolderiksen

</details>

<details>
<summary>click to expand ORIGINAL README</summary>

## Compiling:

To compile ABC as a binary, download and unzip the code, then type `make`.
To compile ABC as a static library, type `make libabc.a`.

When ABC is used as a static library, two additional procedures, `Abc_Start()`
and `Abc_Stop()`, are provided for starting and quitting the ABC framework in
the calling application. A simple demo program (file src/demo.c) shows how to
create a stand-alone program performing DAG-aware AIG rewriting, by calling
APIs of ABC compiled as a static library.

To build the demo program

 * Copy demo.c and libabc.a to the working directory
 * Run `gcc -Wall -g -c demo.c -o demo.o`
 * Run `g++ -g -o demo demo.o libabc.a -lm -ldl -lreadline -lpthread`

To run the demo program, give it a file with the logic network in AIGER or BLIF. For example:

    [...] ~/abc> demo i10.aig
    i10          : i/o =  257/  224  lat =    0  and =   2396  lev = 37
    i10          : i/o =  257/  224  lat =    0  and =   1851  lev = 35
    Networks are equivalent.
    Reading =   0.00 sec   Rewriting =   0.18 sec   Verification =   0.41 sec

The same can be produced by running the binary in the command-line mode:

    [...] ~/abc> ./abc
    UC Berkeley, ABC 1.01 (compiled Oct  6 2012 19:05:18)
    abc 01> r i10.aig; b; ps; b; rw -l; rw -lz; b; rw -lz; b; ps; cec
    i10          : i/o =  257/  224  lat =    0  and =   2396  lev = 37
    i10          : i/o =  257/  224  lat =    0  and =   1851  lev = 35
    Networks are equivalent.

or in the batch mode:

    [...] ~/abc> ./abc -c "r i10.aig; b; ps; b; rw -l; rw -lz; b; rw -lz; b; ps; cec"
    ABC command line: "r i10.aig; b; ps; b; rw -l; rw -lz; b; rw -lz; b; ps; cec".
    i10          : i/o =  257/  224  lat =    0  and =   2396  lev = 37
    i10          : i/o =  257/  224  lat =    0  and =   1851  lev = 35
    Networks are equivalent.

## Compiling as C or C++

The current version of ABC can be compiled with C compiler or C++ compiler.

 * To compile as C code (default): make sure that `CC=gcc` and `ABC_NAMESPACE` is not defined.
 * To compile as C++ code without namespaces: make sure that `CC=g++` and `ABC_NAMESPACE` is not defined.
 * To compile as C++ code with namespaces: make sure that `CC=g++` and `ABC_NAMESPACE` is set to
   the name of the requested namespace. For example, add `-DABC_NAMESPACE=xxx` to OPTFLAGS.

## Building a shared library

 * Compile the code as position-independent by adding `ABC_USE_PIC=1`.
 * Build the `libabc.so` target:

     make ABC_USE_PIC=1 libabc.so

## Bug reporting:

Please try to reproduce all the reported bugs and unexpected features using the latest
version of ABC available from https://github.com/berkeley-abc/abc

If the bug still persists, please provide the following information:

 1. ABC version (when it was downloaded from GitHub)
 1. Linux distribution and version (32-bit or 64-bit)
 1. The exact command-line and error message when trying to run the tool
 1. The output of the `ldd` command run on the exeutable (e.g. `ldd abc`).
 1. Versions of relevant tools or packages used.


## Troubleshooting:

 1. If compilation does not start because of the cyclic dependency check,
try touching all files as follows: `find ./ -type f -exec touch "{}" \;`
 1. If compilation fails because readline is missing, install 'readline' library or
compile with `make ABC_USE_NO_READLINE=1`
 1. If compilation fails because pthreads are missing, install 'pthread' library or
compile with `make ABC_USE_NO_PTHREADS=1`
    * See http://sourceware.org/pthreads-win32/ for pthreads on Windows
    * Precompiled DLLs are available from ftp://sourceware.org/pub/pthreads-win32/dll-latest
 1. If compilation fails in file "src/base/main/libSupport.c", try the following:
    * Remove "src/base/main/libSupport.c" from "src/base/main/module.make"
    * Comment out calls to `Libs_Init()` and `Libs_End()` in "src/base/main/mainInit.c"
 1. On some systems, readline requires adding '-lcurses' to Makefile.

The following comment was added by Krish Sundaresan:

"I found that the code does compile correctly on Solaris if gcc is used (instead of
g++ that I was using for some reason). Also readline which is not available by default
on most Sol10 systems, needs to be installed. I downloaded the readline-5.2 package
from sunfreeware.com and installed it locally. Also modified CFLAGS to add the local
include files for readline and LIBS to add the local libreadline.a. Perhaps you can
add these steps in the readme to help folks compiling this on Solaris."

The following tutorial is kindly offered by Ana Petkovska from EPFL:
https://www.dropbox.com/s/qrl9svlf0ylxy8p/ABC_GettingStarted.pdf

## Final remarks:

Unfortunately, there is no comprehensive regression test. Good luck!

This system is maintained by Alan Mishchenko <alanmi@berkeley.edu>. Consider also
using ZZ framework developed by Niklas Een: https://bitbucket.org/niklaseen/abc-zz (or https://github.com/berkeley-abc/abc-zz)


</details>
