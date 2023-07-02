
# Quantity and array
## Test 1

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

## Test 2

Let a and b be to array with size n, and do a computation c = a/b.
Test for time, for different n and different type of a and b.
Type includes: pure np.array, Quantity, Quantity but with dimensionless unit, etc.

