# Hi, I'm Nilotpal 👋

I'm a **PhD candidate in Mechanical Engineering at Virginia Tech**, in the Laboratory of Transport
Phenomena for Advanced Technologies. I defend in **December 2026**.

**[nilotpalchakraborty.com](https://nilotpalchakraborty.com)** — longer write-ups of the work below.

I build the numerical machinery that closed solvers don't provide. Most of my work is inside other
people's codes: a ~1,200-line stochastic breakage library in a commercial CFD solver — special
functions, spline inversion, and a per-particle state channel packed into the IEEE-754 mantissa of
the diameter word because the API carries no state — and an HLLC Riemann solver in an open-source
SPH library, written after working out why its surface-tension model failed at high Reynolds
number.

My application domain is particle ingestion in gas-turbine compressors: 100M+ cell campaigns on
HPC, sponsored by Rolls-Royce and Pratt & Whitney, both of whom took the solver code for internal
use. **The portable part is the machinery, not the domain.**

## Some things I've done

**Found and reported a numerical instability in [SPHinXsys](https://github.com/Xiangyu-Hu/SPHinXsys).**
The shipped multiphase surface-tension model breaks down at high Reynolds number — in the square
droplet test the fluid particles don't just disorder, they leave the domain. I isolated it with a
sequence of experiments and reported it in
[#378](https://github.com/Xiangyu-Hu/SPHinXsys/issues/378) and
[#497](https://github.com/Xiangyu-Hu/SPHinXsys/issues/497). The maintainers later traced it to
zero-surface-energy modes and fixed it; I'm acknowledged in the resulting paper:

> S. Zhang, S.D.N. Lourenço, X. Hu, *Multiphase SPH for surface tension: resolving
> zero-surface-energy modes and achieving high Reynolds number simulations*,
> [Computer Methods in Applied Mechanics and Engineering 444 (2025) 118147](https://doi.org/10.1016/j.cma.2025.118147).

**[Lid-driven cavity](https://github.com/nilot-pal/Lid-driven-cavity)** — incompressible
Navier–Stokes on a staggered grid, validated against Ghia et al. (1982). Term project for Advanced
CFD, with a full technical report.

**[Membrane permeability from molecular structure](https://github.com/nilot-pal/Membrane-permeability-using-ML)** —
predicting permeability of drug-like molecules from SMILES-derived descriptors and fingerprints.
Lasso, MLP and a combined model; R² = 0.90 without the expensive physics-based features, against
0.99 with them.

**Machine learning practice** — [spam detection](https://github.com/nilot-pal/text-classification)
(TF-IDF baselines vs. DistilBERT) and [churn prediction](https://github.com/nilot-pal/churn-prediction),
both built around evaluation rather than accuracy: precision–recall, threshold tuning under
asymmetric costs, and what the added model complexity actually buys.

## Working on

Learned surrogates for expensive particle-laden physics on the open NASA Rotor 35 geometry, with
the evaluation done properly — held-out conditions, conservation checks on the predicted
distributions, and an honest map of where the surrogate stops being trustworthy.

## Technical

**Languages** Python · Fortran 90 · C++ · Java · MATLAB · Bash
**HPC** Linux · Slurm · MPI · large-memory nodes · scaling and performance profiling
**Numerical** Special functions from scratch · cubic splines and spline inversion · bit-level
IEEE-754 encoding · approximate Riemann solvers (HLLC) · meshfree/SPH · Lagrangian particle
tracking · finite volume (RANS/SST) · KD-tree spatial search
**ML** PyTorch · scikit-learn · XGBoost · NumPy/Pandas — surrogate and reduced-order modelling,
cross-validation, cost-sensitive evaluation
**Simulation** ANSYS CFX · Fluent · TurboGrid · ICEM CFD · solver-embedded user subroutines

## Awards

- **Summer Cunningham Fellowship**, Virginia Tech Graduate School (2026)
- **Pratt Fellowship**, Virginia Tech College of Engineering (2022)
- **Mitacs Globalink Research Fellowship** (2019)
- **Shastri Research Fellowship**, Shastri Indo-Canadian Institute (2019)

📫 nilotpalc@vt.edu · [nilotpalchakraborty.com](https://nilotpalchakraborty.com)
