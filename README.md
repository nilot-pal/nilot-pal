# Hi, I'm Nilotpal 👋

**I build the numerical machinery that closed solvers don't provide.**

Five years inside commercial and open-source CFD codes: a ~1,200-line stochastic breakage library
in ANSYS CFX, an HLLC Riemann solver in SPHinXsys, and 100M+ cell campaigns on HPC. PhD in
Mechanical Engineering at Virginia Tech, conferring **May 2027**, available from June, work-authorised
on OPT with the STEM extension so no day-one sponsorship.

Longer write-ups of everything below: **[nilotpalchakraborty.com](https://nilotpalchakraborty.com)**

---

### Four things I can show you

**Defects I found in solvers I did not write.** One open-source, one commercial. Neither crashed;
both returned plausible answers that were wrong. The maintainers of the first fixed it and
acknowledged me in [*Computer Methods in Applied Mechanics and Engineering* **444** (2025)](https://doi.org/10.1016/j.cma.2025.118147).
The second is present in all five geometries I have tested.

**Machinery built inside a closed API.** Damage history had to survive a particle migrating between
MPI ranks, and the solver's user routine returns six values of which only the diameter travels on.
So the state went into the unused mantissa bits of that diameter, costing 0.39% of the carrier,
which is three orders of magnitude below what the mesh resolves.

**Numerical methods, measured rather than asserted.** Red-black SOR on an NVIDIA L40S against a
matched CPU baseline: bandwidth-bound at 89.6% of achievable DRAM throughput, and once the memory
traffic was cut, runtime tracked traffic to within one percent. Observed order of accuracy 2.06
against a manufactured solution, not inferred from a residual.

**Large simulation, made feasible and made readable.** A scaling study that got **3.9× on sixteen
nodes** where the textbook predicts a penalty, and found licences rather than hardware were the
ceiling. Then attribution of 586,764 impacts back to the particles that caused them, showing
**4.6% of particles were producing 92% of the map**.

The six pinned repositories below are those four claims. Everything else is listed at the bottom.

---

## All repositories

**Finding defects in other people's solvers**

- [sph-high-re-surface-tension](https://github.com/nilot-pal/sph-high-re-surface-tension): a surface-tension model that fails above Re ≈ 10³, the diagnostic sequence, and an HLLC Riemann solver
- [cfx-interface-particle-shift](https://github.com/nilot-pal/cfx-interface-particle-shift): particles displaced crossing a rotor–stator interface, in all five geometries tested

**Building machinery inside a solver with no room for it**

- [mantissa-state-channel](https://github.com/nilot-pal/mantissa-state-channel): per-particle state carried in the unused mantissa bits of a float, with the error bound measured against 385,170 mesh nodes

**Numerical methods, and what they cost to run**

- [cuda-poisson-sor](https://github.com/nilot-pal/cuda-poisson-sor): red-black SOR on an L40S, bandwidth-bound at 89.6% of achievable throughput, with runtime tracking memory traffic to within 1%
- [cfd-iterative-solvers](https://github.com/nilot-pal/cfd-iterative-solvers): Gauss-Seidel, SOR and ADI against a known exact solution; observed order of accuracy 2.06
- [Lid-driven-cavity](https://github.com/nilot-pal/Lid-driven-cavity): finite-volume Navier–Stokes validated against Ghia et al., and why it diverged inside both textbook stability limits

**Large simulation, made feasible and made readable**

- [cfx-cluster-scaling](https://github.com/nilot-pal/cfx-cluster-scaling): 3.9× faster on sixteen nodes than one, and the licence ceiling nobody had looked for
- [cfx-particle-id-recovery](https://github.com/nilot-pal/cfx-particle-id-recovery): 4.6% of the particles were producing 92% of the impacts

**Other**

- [Membrane-permeability-using-ML](https://github.com/nilot-pal/Membrane-permeability-using-ML): permeability from SMILES alone, R² = 0.90
- [text-classification](https://github.com/nilot-pal/text-classification): TF-IDF and logistic regression against DistilBERT, and why the F1 winner is not the obvious deployment choice
- [churn-prediction](https://github.com/nilot-pal/churn-prediction): end to end, with threshold selection treated as part of model selection

## Technical

**Languages** Python · Fortran 90 · C++ · CUDA · MATLAB · Bash
**HPC** Linux · Slurm · MPI · GPU (CUDA, Nsight Compute) · large-memory nodes · scaling and performance profiling
**Numerical** Iterative solvers for linear systems (Gauss-Seidel, SOR, ADI) · finite volume and finite difference · verification against manufactured and analytic solutions · stability analysis · approximate Riemann solvers (HLLC) · special functions from scratch · cubic splines and spline inversion · bit-level IEEE-754 encoding · meshfree/SPH · Lagrangian particle tracking · KD-tree spatial search
**ML** PyTorch · scikit-learn · XGBoost · NumPy/Pandas · surrogate and reduced-order modelling · cross-validation · cost-sensitive evaluation
**Simulation** ANSYS CFX · Fluent · TurboGrid · ICEM CFD · solver-embedded user subroutines

## Awards

- **Summer Cunningham Fellowship**, Virginia Tech Graduate School (2026)
- **Pratt Fellowship**, Virginia Tech College of Engineering (2022)
- **Mitacs Globalink Research Fellowship** (2019)
- **Shastri Research Fellowship**, Shastri Indo-Canadian Institute (2019)

📫 nilotpalc@vt.edu · [nilotpalchakraborty.com](https://nilotpalchakraborty.com)
