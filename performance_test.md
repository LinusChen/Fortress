# Performance test.
In this file, I record the tests for the time needed for some computation work.
This will provided knowledge about how to improve the performance of my code.

## Frequently used functions.
In this part, I test the computational cost of some functions.

### Univariate function

I start with functions with only one variable.
  y=f(x)

For each of these fuctions, the test is done in the following way.
x is an np.array size nx=1e4.
The evaluation of f(x) would be repeated n=1e4 times,
and the time used will be counted using the timeit package.

| Function          |   Time (sec)  |
| -------------     | ------------- |
| x ** 2.0          |      0.05     |
| x ** 2.3          |      1.25     |
| cos(x)            |      0.09     |
| exp(x)            |      0.09     |
| exp(1j\*x)        |      2.9      |
| cos(x)+1j\*sin(x) |      0.4      |
| phase_to_V(x)     |      0.24     |
| jv(0,x)           |     58        |
| jv(1,x)           |     56        |
| j0(x)             |      2.4      |
| j1(x)             |      2.4      |

Note:
- j0(x) is much faster than jv(0,x). Similar for 1st order Bessel.
- exp(1j\*x) is much slower than cos(x)+1j\*sin(x)
- phase_to_V is a more efficient function to compute exp(1j\*x),
  it creates y as an empty complex array and then assigns cos(x) and sin(x) as real/imag.
- x\*\*2.0 is faster than x\*\*2.3, mabe because special treating of integer power.

### Univariate function pre-tabulated

When the function value is real number.
| n_step_grid       |   Time (sec)  |
| -------------     | ------------- |
|  1e5              |     10.4      |
|  1e4              |      7.4      |
|  1e3              |      5.2      |
|  1e2              |      3.3      |


When the function value is complex number.
| n_step_grid       |   Time (sec)  |
| -------------     | ------------- |
|  1e5              |     12.1      |
|  1e4              |     10.9      |
|  1e3              |      9.1      |
|  1e2              |      6.8      |

Therefore, for simple functions like cos, exp, j0, etc.,
it is generally not worthy to pre-tabulate.

### FT of ChromaticSBD

In this section, we test the time used for FT of an ChromaticSBD object.
For each ChromaticSBD object, we evalute on a grid of
  n_wavelength = 1, n_uv=101.
We repeat the operation for n=1e4 times.

|           object  |   Time (sec)  |
| -------------     | ------------- |
|  star             |      2.1      |
|  ring0            |      3.5      |
|  ring1            |      4.0      |
|  star_ring        |      6.5      |

Here star is a point source;
ring0 is UniformRing;
ring1 is InclinedSBD of UniformRing.
star_ring is the combination of star and ring1.

Apparently, the time for ring0 computing is much longer than
the time spent on the j0 or j1 computing.
Where is the overhead?

