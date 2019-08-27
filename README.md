# S4IOF (Scheme 4 In One File) - A Minimal R4RS Scheme System
                         
SIOF is a portable interpreter for a subset of the Scheme programming language. 
It can be built from a single portable C source file [siof.c](https://raw.githubusercontent.com/false-schemers/siof/master/siof.c); 
there are no OS- or hardware-specific parts, no compiler-specific tricks, no dependency on platform-specific building tools. There is no distributive to install: just compile the file with your favorite C compiler, link it with the standard C runtime libraries and be done with it.

## Installation

There is no installation to speak of; just grab the file and compile it with your favorite C compiler.

Here's how you can compile SIOF on a unix box:

```
gcc -o siof -Wall -O3 -DNDEBUG siof.c
```

Please note that some compilers may issue hundreds of warnings; we recommend to add `-Wno-parentheses-equality` for
Clang and `-D_CRT_SECURE_NO_WARNINGS` for Windows headers (unless you want to hear that `fopen`
is no longer a reasonable way to open files).

If you specify high optimization settings as in the example above, be ready for long compilation times. But it's usually worth the wait: the executable is smaller and faster (which is important because the speed was traded for code size).

The resulting interpreter has no dependencies and can be run from any location.


## Scheme Compatibility

SIOF is true to basic Scheme principles -- it features precise garbage collector, supports proper tail recursion, `call/cc`, `dynamic-wind`, and multiple return values. It is mostly compatible with R4RS, but it has the following known limitations and deviations from the standard:

  *  hygienic macro facilities (`define-syntax` & friends) are not implemented
  *  there is a simple `define-macro` facility for S-expression rewriting
  *  `read` and `string->symbol` are case-sensitive
  *  `eval` is single-argument; environment functions are not implemented
  *  fixnums are limited to 24 bits, flonums are doubles
  *  no support for bignums/rational/complex numbers
  *  `max` and `min` do not preserve inexactness
  *  some run-time errors such as zero division and fixnum overflows trigger asserts in C code unless `NDEBUG` is defined during compilation
  * `transcript-on` and `transcript-off` are not implemented

SIOF supports some popular extensions defined in other standards and libraries:

  *  `error` (not based on exceptions)
  *  `file-exists?`, `delete-file`, `rename-file`
  *  `exit`, `abort`, `reset`, `command-line`
  *  `get-environment-variable`, `system`, `current-jiffy`, `jiffies-per-second` 


## Origins

SIOF's original code is written in [#F](https://github.com/false-schemers/sharpF), a language for building Scheme-like
systems. Its #F source code can be found there in `examples` directory:

[s4iof.sf](https://raw.githubusercontent.com/false-schemers/sharpF/master/examples/s4iof.sf)

SIOF's on-the-fly compiler is derived from Marc Feeley's Scheme Interpreter (see `gambit/bench/src/scheme.scm` in the [Gambit Scheme repository](https://github.com/gambit/gambit)). Supporting library code comes from #F's [LibS library](https://raw.githubusercontent.com/false-schemers/sharpF/master/lib/libs.sf).

