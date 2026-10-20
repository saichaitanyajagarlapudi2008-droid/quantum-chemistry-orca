# RESP charges for DEHP and PVC oligomers

Partial charges for di(2-ethylhexyl) phthalate (DEHP) and poly(vinyl chloride) (PVC) oligomers, derived to check the charge sets used in an all-atom MD replication of Zhou & Milner, *Macromolecules* 2018, 51, 3865–3873 ([MD project](https://github.com/saichaitanyajagarlapudi2008-droid/md-projects-gromacs/tree/main/pvc-dehp-density-fluctuations)).

## Method

1. **Geometry optimisation:** B3LYP/def2-SVP in ORCA (`*_opt.inp` → `*_opt.xyz`; `*_opt_trj.xyz` holds every optimisation step).
2. **Electrostatic potential:** HF/6-31G\* single point on the optimised geometry (`*_sp.inp`). HF/6-31G\* is the standard level for RESP. Its gas-phase over-polarisation roughly mimics condensed-phase polarisation, which suits fixed-charge force fields.
3. **Charge fitting:** RESP fit to the ESP in Multiwfn (`*_sp_resp.chg`). For DEHP, CM5 charges were also computed for comparison (`dehp_sp_CM5.chg`).

`.chg` files list element, x, y, z (Å) and charge (e) for each atom. All molecules are neutral singlets.

## Layout

```
dehp/            DEHP: inputs, optimised geometry, RESP and CM5 charges
pvc/trimer/      PVC 3-mer
pvc/pentamer/    PVC 5-mer
pvc/20mer/       PVC 20-mer (122 atoms)
```

## Results

### PVC: chain-length convergence

Mean RESP charge (e) by atom type, for atoms in the central part of each chain (away from the ends):

| Atom type | Trimer | Pentamer | 20-mer |
| --- | --- | --- | --- |
| Cl | −0.189 | −0.204 | −0.198 |
| C (CHCl) | +0.130 | +0.110 | +0.091 |
| C (CH₂) | −0.060 | −0.170 | −0.179 |
| H on CH₂ | +0.063 | +0.085 | +0.099 |
| H on CHCl | +0.076 | +0.070 | +0.102 |

- The trimer is too short: its central CH₂ carbon charge differs strongly from the longer chains.
- From the pentamer to the 20-mer, Cl and CH₂ charges agree within about 0.01 e, so the central-unit charges are close to converged. The CHCl carbon still shifts by about 0.02 e.
- Individual buried carbons scatter widely around these means (standard deviation up to ~0.18 e for CH₂ in the 20-mer). This is a known RESP limitation for atoms far from the molecular surface, and the reason averaging by atom type is used.

### DEHP: RESP vs CM5

Mean charge (e) by atom type:

| Atom type | RESP | CM5 |
| --- | --- | --- |
| C=O carbon | +0.835 | +0.341 |
| C=O oxygen | −0.584 | −0.369 |
| Ester oxygen (O–C) | −0.466 | −0.252 |
| O–CH₂ carbon | +0.090 | −0.013 |
| Aromatic C–H carbon | −0.117 | −0.076 |
| CH₃ carbon | −0.209 | −0.213 |

RESP gives a much more polarised ester group than CM5. That is expected: RESP is fitted to the electrostatic potential, while CM5 is a population-based model. In the MD project, these RESP charges were compared atom by atom with LigParGen (CM1A-LBCC) and OPLS-AA table charges. For example, OPLS-AA assigns the O–CH₂ carbon +0.19 e, compared with +0.09 e from RESP.
