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

| n_step_grid       |   Time (sec)  |
| -------------     | ------------- |
|  1e4              |      7.4      |


| n_step_grid       |   Time (sec)  |
| -------------     | ------------- |
|  1e4              |     10.9      |

