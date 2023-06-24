# In this file we explain the frequently used symbols in the package.

r_vec = (x,y) is often used to donate a position on the sky, typically it is angular position.
Sometime s_vec=(x,y) is used to donote an "offest" or "displacement" on the sky, which could be understood as difference between two r_vec on the sky.

B_vec=(u,v) is baseline vector.
The inner product of (u,v) and (x,y) would be a scalar, which represents the path difference of the wave from two sky positions reaching the two telescopes.
For a wavelength, the phase difference would be phi = 2pi * b_vec*r_vec/wavelength.

$\nu_{vec} = B_{vec}/\lambda$ could then be called spatial frequency. It has unit cycle/rad.
Its scalar version could be written as $\nu$.
Furthermore, $\omega=2\pi\nu$ could also be called spatial frequency, but has unit rad/rad or rad/mas.

For example, for a binary with seperation s = 1 mas = 4.8e-9rad, with a baseline of B = 100 meter = 1e4cm along the separation,
the path difference would 4.8e-5 cm.
At wavelength = 10 mum, this leads to a phase shift of 4.8e-2 cycle, or 0.3 rad, between the two stars.
