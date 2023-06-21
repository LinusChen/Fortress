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

| Function      |   Time (sec   |
| ------------- | ------------- |
|               |               |
| cos(x)        |      0.09     |
| exp(x)        |      0.09     |
| exp(1j*x)     |      2.9      |
| jv(0,x)       |      58       |
| jv(1,x)       |      56       |
| j0(x)         |      2.4      |
| j1(x)         |      2.4      |

Note:
- j0(x) is much faster than jv(0,x). Similar for 1st order Bessel.

