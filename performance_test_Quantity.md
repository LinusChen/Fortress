
# Quantity and array
## Test 1

For a set of values (b,w), and value r, compute
  $z = 2pi (b/w) r$,
so that z would be an array shape of (n_b, n_w).
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

When (b,w,r) are in all units of "units.dimensionless_unscaled".
In this case, the z would be dimensionless, and then turn into np.array.
So overhead reduced.
|     n_b, n_w      |   Time (sec)  |
| -------------     | ------------- |
|  1, 1             |      0.46     |
|  6, 50            |      0.65     |
|  100, 100         |      0.77     |

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

## Test 2

Let a and b be to array with size n, and do a computation c = a/b.
Test for time, for different n and different type of a and b.
Type includes: pure np.array, Quantity, Quantity but with dimensionless unit, etc.

|     Type                |   n=1           |  n = 1e2      |    n=1e4      |    n=1e6      |
| -------------           |  -------------  | ------------- | ------------- |-------------  |
|  np.array               |   0.008         |  0.008        |   0.07        |   8.86        |
|  Quan, dim-less         |   0.11          |  0.10         |   0.14        |   9.16        |
|  Quan, unit=cm          |   0.15          |  0.15         |   0.19        |   9.66        |
|  Quan, unit=(m,cm)      |   0.17          |  0.17         |   0.22        |   9.56        |

Lessons learned:
When arrays are large (like n=1e6), most time is spent on actual computing,
and there is no need to worry about the overhead of unit manupulating.
However, for smaller arrays, the overhead become dominant.

