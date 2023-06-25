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

## Quantity and array

For a set of values (b,w), and value r, compute
  $z = 2pi (b/w) r$
So that z would be an array shape of (n_b, n_w).
The operation will be repeated for 1e4 times.

When (b,w,r) are in units of (m, mum, mas), respectively.
In this case, the z need be converted to dimensionless, and then turn into np.array.
Again, there is an overhead of ~1,0 sec, when (n_b, n_w) is negligible.
|     n_b, n_w      |   Time (sec)  |
| -------------     | ------------- |
|  1, 1             |      1.0      |
|  6, 50            |      1.2      |
|  100, 100         |      1.5      |

When (b,w,r) are in units of (cm, cm, rad), respectively.
In this case, the z need be converted to dimensionless, and then turn into np.array.
So overhead reduced if all the inputs is already in cgs.
|     n_b, n_w      |   Time (sec)  |
| -------------     | ------------- |
|  1, 1             |      0.69     |
|  6, 50            |      0.89     |
|  100, 100         |      1.1      |

When (b,w,r) are just np.array instead of Quantity.
In this case, z would directly be a np.array, with no unit conversion needed.
So overhead reduced if all the inputs is already normal np.array.
|     n_b, n_w      |   Time (sec)  |
| -------------     | ------------- |
|  1, 1             |      0.07     |
|  6, 50            |      0.13     |
|  100, 100         |      0.25     |

Following I record the time needed for computing V=j0(z), again 1e4 times.
|     n_b, n_w      |   Time (sec)  |
| -------------     | ------------- |
|  1, 1             |      0.003    |
|  6, 50            |      0.04     |
|  100, 100         |      1.3      |

Summary: if the complicated dealing with units is needed,
the process of calculating z would easily spend more time than the j0 computing.
Therefore, for frequently used Quantity, always try to do the following.
- convert into cgs system beforehand.
- if possible, represent as pure float array.
  Maybe indicate the units in name, e.g., r_rad.

## FT of ChromaticSBD

In this section, we test the time used for FT of an ChromaticSBD object.|  star_ring        |      4.7      |

For each ChromaticSBD object, we evalute on a grid of
  n_wavelength = 1, n_uv=101.
We repeat the operation for n=1e4 times.

|           object  |   Time (sec)  |
| -------------     | ------------- |
|  star             |      2.1      |
|  ring0            |      3.5      |
|  ring1            |      4.0      |
|  star_ring        |      6.5      |

Following is when n_wavelength = 6.
|           object  |   Time (sec)  |
| -------------     | ------------- |
|  star0            |      2.2      |
|  star1            |      2.4      |
|  star2            |      4.0      |
|  ring0            |      4.2      |
|  ring1            |      4.7      |
|  star_ring        |      7.5      |

Following is when (n_b,n_w) = (6,50). And already with some optimization about the ring computing (using r_rad).
|           object  |   Time (sec)  |
| -------------     | ------------- |
|  star0            |      2.1      |
|  star1            |      2.3      |
|  star2            |      3.9      |
|  ring0            |      1.9      |
|  ring1            |      2.3      |
|  star_ring        |      4.7      |
|  New way.         |               |
|  star0            |      0.7      |
|  star1            |      2.3      |
|  star2            |      2.4      |
|  star_ring        |      3.2      |

Apparently, the time for ring0 computing is much longer than
the time spent on the j0 or j1 computing.
Where is the overhead?

Also when n_wavelength increase from 1 to 6,
the increase in time is very limited.
This suggests the time on actually computing the data on each wavelength and baseline
is limited.
More time is spent on overhead.

Later result (for ring0 computing; n_b, n_w = 6, 50):
- just using (r_in_rad, r_out_rad) instead (r_in, r_out)
  would decrease the time from 4.2s to 2.8s.
- converting b,w to cm (before the meshgrid) further decrease time to 2.1s.

About point source computing.
May I save time for point source without offset?

Following is when (n_b,n_w) = (6,50). And with some optimization regarding point source.
Note that now "star1" impossible.
|           object  |   Time (sec)  |    Time   |
| -------------     | ------------- |  -------- |
|                   |    f_corr     |  FT_T2    |
|  star0            |      0.7      |
|  star2            |      2.4      |
|  ring0            |      1.7      |
|  ring1            |      2.0      |
|  star_ring        |      3.0      |

### Description of SBDs tested.
- star0 is a point source with no offset
- star1 is a point source with offset (not possible in new design)
- star2 is star0 with a displacement.
- ring0 is a UniformRing;
- ring1 is InclinedSBD of rong0.
- star_ring is the combination of star0 and ring1.
The "new way" is to implement star0 with offset=None.
Turns out that is much more efficient (eliminating the computing of phase shift, which is actually 0).

