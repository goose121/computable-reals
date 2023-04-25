# Computable Reals

## Introduction

Computable real numbers `x` are interpreted as (potentially) infinite
fractions in base 2 that are specified through a rule for computation
of an integer `a` with `|(2^k)*x - a| <= 1` for any `k>=0`.

The internal data structure should not be accessed.

The interface for the outside world is as follows:

The type `CREAL` is a supertype of the type `RATIONAL`. `(CREAL-P x)`, for
an object `x`, returns `T` if `x` is of type `CREAL`, otherwise `NIL`.

`(RATIONAL-APPROX-R x k)`, for a `CREAL` `x` and an integer `k>=0`, returns a rational `a` with `|x - a| < 1/2^k`.

`(MAKE-REAL fun)` returns the real number given by `fun`. Here `fun` is a
function taking an argument `k`, that computes `a` as above.

`CREAL`s are displayed by `PRINT` (etc.) as a decimal fraction. The
error hereby is at most one unit in the last digit that was
output. The number of decimal digits after the decimal point is
defined through the dynamic variable `*PRINT-PREC*`.

For *internal* comparison operations etc. a precision threshold is used. It is
defined through the dynamic variable `*CREAL-TOLERANCE*`. Its value should
be a nonnegative integer `n`, meaning that numbers are considered equal
if they differ by at most `2^(-n)`.

**N.B.** There are *no* external comparison functions, since
comparison is in general not decidable in finite time. Instead, we
recommend using ordinary Common Lisp comparison functions after
producing a `k`-bit approximation using the `RATIONAL-APPROX-R` to
convert a `CREAL` to a rational number.

## Exported Functions and Constants

The following functions, constants and variables are exported. (The
package is named `"COMPUTABLE-REALS"` or `"CR"` for short.)

```
CREAL                  type        type of the computable real numbers
CREAL-P object         function    tests for type CREAL
*PRINT-PREC*           variable    specifies precision of output
*CREAL-TOLERANCE*      variable    precision threshold for comparison
APPROX-R x:creal k:int>=0
                       function    returns approximation of x to k digits
MAKE-REAL function     function    creates object of type CREAL
RATIONAL-APPROX-R x:creal k:int>0
                       function    returns a rational approximation that
                                    differs by less than 2^(-k).
RATIONALIZE-R x:creal k:int>0
                       function    returns the simplest rational approximation
                                    that differs by less than 2^(-k)
RAW-APPROX-R x:creal   function    returns 3 values a,n,s with:
                                   if a = 0: |x| <= 2^(-n), s = 0
                                       and n >= *CREAL-TOLERANCE*
                                   else: a0 integer > 4, n0 integer >=0,
                                       s = +1 or -1, and sign(x) = s,
                                       (a-1)*2^(-n) <= |x| <= (a+1)*2^(-n)
PRINT-R x:creal k:int>=0 &optional (flag t)
                       function    outputs x with k decimal digits.
                                   If flag is true, first a newline.
+R {creal}*            function    computes the sum of the arguments
-R creal {creal}*      function    computes negative or difference
*R {creal}*            function    computes the product of the arguments
/R creal {creal}*      function    computes reciprocal or quotient
SQRT-R creal           function    computes the square root
+LOG2-R+               constant    log(2)
+PI-R+                 constant    pi
+2PI-R+                constant    2*pi
+PI/2-R+               constant    pi/2
+PI/4-R+               constant    pi/4
LOG-R x:creal &optional b:creal
                       function    computes the logarithm of n in base b;
                                   default is the natural logarithm
EXP-R creal            function    computes the exponential function
EXPT-R x:creal y:creal function    computes x^y
SIN-R creal            function    computes the sine
COS-R creal            function    computes the cosine
TAN-R creal            function    computes the tangent
ATAN-R x:creal &optional y:creal
                       function    computes the arctangent of x or
                                   the phase angle of (x,y)
ASH-R x:creal n:int    function    computes x * 2^n
ROUND-R x:creal &optional y:creal
                       function    computes two values q (integer) and r
                                   (creal) with x = q*y + r and |r|<=|y|/2
                                   according to the precision specified by
                                   *CREAL-TOLERANCE*
FLOOR-R x:creal &optional y:creal
                       function    like ROUND-R, corresponding to floor
CEILING-R x:creal &optional y:creal
                       function    like ROUND-R, corresponding to ceiling
TRUNCATE-R x:creal &optional y:creal
                       function    like ROUND-R, corresponding to truncate
```




