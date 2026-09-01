# Hi, I'm Nilotpal 👋

I'm a **PhD candidate in Mechanical Engineering at Virginia Tech**, **[in the Laboratory of Transport
Phenomena for Advanced Technologies](https://tpl.me.vt.edu/)**. My PhD confers in **May 2027** and
I'm available from June, work-authorised on OPT with the STEM extension, so no day-one sponsorship.

**[nilotpalchakraborty.com](https://nilotpalchakraborty.com)**: longer write-ups of the work below.

I build the numerical machinery that closed solvers don't provide. Most of my work is inside other
people's codes. A ~1,200-line stochastic breakage library in a commercial CFD solver, with special
functions, spline inversion, and a per-particle state channel packed into the IEEE-754 mantissa of
the diameter word because the API carries no state. An HLLC Riemann solver in an open-source SPH
library, written after working out why its surface-tension model failed at high Reynolds number.

My application domain is particle ingestion in gas-turbine compressors: 100M+ cell campaigns on
HPC, sponsored by Rolls-Royce and Pratt & Whitney, both of whom took the solver code for internal
use. **The portable part is the machinery, not the domain.**

## Some things I've done

**Found and reported a numerical instability in [SPHinXsys](https://github.com/Xiangyu-Hu/SPHinXsys).**
The shipped multiphase surface-tension model breaks down at high Reynolds number: in the square
droplet test the fluid particles don't just disorder, they leave the domain. I isolated it with a
sequence of experiments and reported it in
[#378](https://github.com/Xiangyu-Hu/SPHinXsys/issues/378), and
[#497](https://github.com/Xiangyu-Hu/SPHinXsys/issues/497). The maintainers later traced it to
zero-surface-energy modes and fixed it; I'm acknowledged in the resulting paper:

> S. Zhang, S.D.N. Lourenço, X. Hu, *Multiphase SPH for surface tension: resolving
> zero-surface-energy modes and achieving high Reynolds number simulations*,
> [Computer Methods in Applied Mechanics and Engineering 444 (2025) 118147](https://doi.org/10.1016/j.cma.2025.118147).

[**sph-high-re-surface-tension**](https://github.com/nilot-pal/sph-high-re-surface-tension) has
the diagnostic sequence, the HLLC Riemann solver I wrote while chasing it, and the parameter
studies underneath: a screening design over reference velocity and viscosity, a
one-factor-at-a-time sweep, a ladder of dissipation-limiter settings, and a 2 × 2 factorial on
Poiseuille flow, which is the only case there with a closed-form answer to check against. Video
of every run.

**[Particle redistribution at rotor–stator interfaces in CFX](https://github.com/nilot-pal/cfx-interface-particle-shift)**: erosion maps on a compressor rotor came out striped, with 46.8% of blade nodes taking no impacts
at all, although particles were injected at random upstream. Eight hypotheses, seven killed by
experiment. The eighth held: particles are **displaced 4 mm circumferentially crossing the stage
interface**, landing on the centre-lines of the receiving mesh cells. Isolated by decoupling the
particles from the flow entirely (rotor at 10⁻⁶ rpm, fluid forces off), so nothing but the
interface could be doing it. The same class of bug as the SPHinXsys one, in a solver whose source
I cannot read.

**[Scaling ANSYS CFX across a cluster](https://github.com/nilot-pal/cfx-cluster-scaling)**: standard advice for this solver is to minimise node count, since every iteration exchanges
boundary data across the network. Measured, it went the other way: **3.9× faster on sixteen nodes
than one**, because the workload is bound by I/O and memory throughput rather than compute. Shown
the numbers, the university's research computing director called it "generally the opposite of
what I would expect". It also established that *licences*, not hardware, were the real ceiling, after which the group's allocation was doubled. Independent work, not part of my dissertation.

**[Lid-driven cavity](https://github.com/nilot-pal/Lid-driven-cavity)**: incompressible
Navier–Stokes on a staggered grid, fractional-step, validated against Ghia et al. (1982). The
solver diverged at a time step that sat below both the linear CFL and the viscous limit. The cause
is a non-linear CFL condition that restricts higher-order explicit time integrators, and the code
now takes its step from that rather than from either textbook criterion.

**[Iterative solvers for finite-difference systems](https://github.com/nilot-pal/cfd-iterative-solvers)**: the companion study. Gauss-Seidel, SOR and ADI on a manufactured Poisson problem whose exact
solution is known, so the error is measured and not inferred from the residual. Observed order of
accuracy 2.06; ADI reaches tolerance in about a quarter of the iterations SOR needs, at both grid
resolutions. This is the solver behaviour that drives the cavity's cost scaling.

**[Membrane permeability from molecular structure](https://github.com/nilot-pal/Membrane-permeability-using-ML)**: can you predict whether a drug-like molecule crosses a lipid membrane from its SMILES string
alone, without the free energies and pKa values that normally decide it? Four-person course
project; I built the RDKit feature generation (200 molecular descriptors and 1024-bit Morgan fingerprints), and the cross-validated regularisation search and learning-curve diagnostics for
the Lasso model. The team's combined Lasso-MLP reached R² = 0.90 from structure alone.

**Machine learning projects**, [spam detection](https://github.com/nilot-pal/text-classification)
(TF-IDF baselines vs. DistilBERT), and [churn prediction](https://github.com/nilot-pal/churn-prediction),
both built around evaluation rather than accuracy: precision–recall, threshold tuning under
asymmetric costs, and what the added model complexity actually buys.

## Working on

Learned surrogates for expensive particle-laden physics on the open NASA Rotor 35 geometry, with
the evaluation done properly: held-out conditions, conservation checks on the predicted
distributions, and an honest map of where the surrogate stops being trustworthy.

## Technical

**Languages** Python · Fortran 90 · C++ · Java · MATLAB · Bash
**HPC** Linux · Slurm · MPI · large-memory nodes · scaling and performance profiling
**Numerical** Special functions from scratch · cubic splines and spline inversion · bit-level
IEEE-754 encoding · approximate Riemann solvers (HLLC) · meshfree/SPH · Lagrangian particle
tracking · finite volume (RANS/SST) · KD-tree spatial search
**ML** PyTorch · scikit-learn · XGBoost · NumPy/Pandas, surrogate and reduced-order modelling,
cross-validation, cost-sensitive evaluation
**Simulation** ANSYS CFX · Fluent · TurboGrid · ICEM CFD · solver-embedded user subroutines

## Awards

- **Summer Cunningham Fellowship**, Virginia Tech Graduate School (2026)
- **Pratt Fellowship**, Virginia Tech College of Engineering (2022)
- **Mitacs Globalink Research Fellowship** (2019)
- **Shastri Research Fellowship**, Shastri Indo-Canadian Institute (2019)

📫 nilotpalc@vt.edu · [nilotpalchakraborty.com](https://nilotpalchakraborty.com)
