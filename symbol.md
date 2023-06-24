# In this file we explain the frequently used symbols in the package.

r_vec = (x,y) is often used to donate a position on the sky, typically it is angular position.
Sometime s_vec=(x,y) is used to donote an "offest" or "displacement" on the sky, which could be understood as difference bwtween two r_vec on the sky.

b_vec=(u,v) is baseline vector.
The inner product of (u,v) and (x,y) would be a scalar, which represents the path difference of the wave from two sky positions reaching the two telescopes.
For a wavelength, the phase difference would be phi = 2pi * b_vec*r_vec/wavelength.

$nu_{vec} = b_{vec}/wavelength$ could then be called spatial frequency.
