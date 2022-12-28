<a href="../">Notebook</a> > <a href="./">C Programming</a> > Optimization

# Optimization



## Optimization Levels

* `-O0`

  No optimization (default)

  Generates unoptimized code but has the fastest compilation time. (Note that many other compilers do substantial optimization even if "no optimization" is specified. With gcc, it is very unusual to use `-O0` for production if execution time is of any concern, since `-O0` means (almost) no optimization. This difference between gcc and other compilers should be kept in mind when doing performance comparisons.)

* `-O1`

  Moderate optimization

  Optimizes reasonably well but does not degrade compilation time significantly.

* `-O2`

  Full optimization

  Generates highly optimized code and has the slowest compilation time.

* `-O3`

  Full optimization as in `-O2`

  Also uses more aggressive automatic inlining of subprograms within a unit and attempts to vectorize loops.

* `-Os`

  Optimize space usage (code and data) of resulting program.
