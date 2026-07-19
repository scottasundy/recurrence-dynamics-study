# Continuous dynamics

## Equal-mass hard particles on a ring

Four labeled hard point particles move on a unit ring with rational positions
and velocities. Collisions are perfectly elastic; equal masses exchange
velocities. Event times and state updates are evaluated with exact fractions.

The selected orbit has period \(T=10\) and contains 36 collision events per
period. Exchanging two velocity labels preserves the initial positions,
velocity multiset, momentum, and kinetic energy, but changes the subsequent
labeled trajectory.

`elastic_ring_orbit.csv` contains the sampled exact states.
`elastic_ring_pair_distance.png` shows the position-space separation of the
paired trajectories.

## Irrational torus flow

For

\[
q(t)=(t,\sqrt{2}\,t)\pmod 1,
\]

exact recurrence at \(T>0\) would require both \(T\) and \(\sqrt{2}T\) to be
integers, which is impossible. Continued-fraction convergents to \(\sqrt{2}\)
produce arbitrarily close returns.

`irrational_torus_near_returns.csv` lists the convergents and return errors.
`irrational_torus_return_error.png` shows the decrease in near-return error.
