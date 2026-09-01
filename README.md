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
HPC, sponsored by Rolls-Royce and Pratt & Whitney. **The portable part is the machinery, not the domain.**

The work below groups into three claims. Only six repositories can be pinned, so the full list is
at the bottom.

## 1. Finding defects in other people's solvers

One open-source library, one closed commercial code. In both cases the solver did not crash. It
returned a plausible answer that was wrong, which is the harder failure to catch.

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
I cannot read. It is not specific to one case either: the shift is present in **all five geometries
I have tested**, including the vendor's own tutorial geometry, and the particles land on whatever
the receiving mesh offers, cell centres in one, diagonals in another.

## 2. Numerical methods, and what they cost to run

Small problems with exact answers, so the error can be measured rather than inferred, and the cost
of getting it measured too.

**[Lid-driven cavity](https://github.com/nilot-pal/Lid-driven-cavity)**: incompressible
Navier–Stokes on a staggered grid, fractional-step, validated against Ghia et al. (1982). The
solver diverged at a time step that sat below both the linear CFL and the viscous limit. The cause
is a non-linear CFL condition that restricts higher-order explicit time integrators, and the code
now takes its step from that rather than from either textbook criterion.

**[Iterative solvers for finite-difference systems](https://github.com/nilot-pal/cfd-iterative-solvers)**: the companion study. Gauss-Seidel, SOR and ADI on a manufactured Poisson problem whose exact
solution is known, so the error is measured and not inferred from the residual. Observed order of
accuracy 2.06; ADI reaches tolerance in about a quarter of the iterations SOR needs, at both grid
resolutions. This is the solver behaviour that drives the cavity's cost scaling.

## 3. Large simulation, made feasible and made readable

Getting the runs to finish is half of it. The other half is being able to believe the output.

**[Scaling ANSYS CFX across a cluster](https://github.com/nilot-pal/cfx-cluster-scaling)**: standard advice for this solver is to minimise node count, since every iteration exchanges
boundary data across the network. Measured, it went the other way: **3.9× faster on sixteen nodes
than one**, because the workload is bound by I/O and memory throughput rather than compute. Shown
the numbers, the university's research computing director called it "generally the opposite of
what I would expect". It also established that *licences*, not hardware, were the real ceiling, after which the group's allocation was doubled. It also fixed the size at which the queue is worth using at all: below about 50 cores the workstation on the desk was the faster machine. Independent work, not part of my dissertation.

**[Recovering particle identity from CFX impact exports](https://github.com/nilot-pal/cfx-particle-id-recovery)**: CFX writes one row per particle-wall impact and does not write which particle it belongs
to, so per-particle analysis is impossible by design. I recovered it by matching every impact
against a 14 GB trajectory file on its position and velocity signature: **586,764 of 586,764
rotor-blade impacts attributed, 100%**. The answer was that **4.6% of the particles produce 92% of
the impacts**: a small trapped population grazing the blade at 0.08°, while the physical map, the
one everyone else was seeing, is a narrow band at the leading edge. The first version of that
pipeline took 2 h 15 min and then ran out of memory. Parallelising it bought nothing and rewriting
it in C++ bought nothing, because the cost was algorithmic; a streamed spatial index took it to
four minutes on three times the data.

## Other work

**[Membrane permeability from molecular structure](https://github.com/nilot-pal/Membrane-permeability-using-ML)**: can you predict whether a drug-like molecule crosses a lipid membrane from its SMILES string
alone, without the free energies and pKa values that normally decide it? Four-person course
project; I built the RDKit feature generation (200 molecular descriptors and 1024-bit Morgan fingerprints), and the cross-validated regularisation search and learning-curve diagnostics for
the Lasso model. The team's combined Lasso-MLP reached R² = 0.90 from structure alone.

**Machine learning, end to end and evaluated properly.** Two projects carried from raw data through
feature engineering, model selection and threshold choice to a deployment decision:
[spam detection](https://github.com/nilot-pal/text-classification) (TF-IDF and logistic-regression
baselines against DistilBERT) and
[churn prediction](https://github.com/nilot-pal/churn-prediction). DistilBERT wins on recall and on
F1, and is still not obviously the model to deploy, because **model selection and threshold
selection are one decision rather than two**, and the ranking flips when the cost asymmetry moves.
The full argument, with the tables, is at
[nilotpalchakraborty.com/evaluation](https://nilotpalchakraborty.com/evaluation.html).

## All repositories

GitHub pins six. These are the repositories behind the three claims above, whether pinned or not.

**Finding defects in other people's solvers**

- [sph-high-re-surface-tension](https://github.com/nilot-pal/sph-high-re-surface-tension): a surface-tension model that fails above Re ≈ 10³, the diagnostic sequence, and an HLLC Riemann solver
- [cfx-interface-particle-shift](https://github.com/nilot-pal/cfx-interface-particle-shift): particles displaced crossing a rotor–stator interface, in all five geometries tested

**Numerical methods, and what they cost to run**

- [cfd-iterative-solvers](https://github.com/nilot-pal/cfd-iterative-solvers): Gauss-Seidel, SOR and ADI against a known exact solution; observed order of accuracy 2.06
- [Lid-driven-cavity](https://github.com/nilot-pal/Lid-driven-cavity): finite-volume Navier–Stokes validated against Ghia et al., and why it diverged inside both textbook stability limits

**Large simulation, made feasible and made readable**

- [cfx-cluster-scaling](https://github.com/nilot-pal/cfx-cluster-scaling): 3.9× faster on sixteen nodes than one, and the licence ceiling nobody had looked for
- [cfx-particle-id-recovery](https://github.com/nilot-pal/cfx-particle-id-recovery): 4.6% of the particles were producing 92% of the impacts

**Other**

- [Membrane-permeability-using-ML](https://github.com/nilot-pal/Membrane-permeability-using-ML): permeability from SMILES alone, R² = 0.90

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
