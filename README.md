# qvSZPs
Repository providing the basis set files for the q-vSZPs basis set and the non-adaptive q<sup>avg</sup>-vSZPs variant.
The basis set files are located in `q-vSZPs_basis/` with the q-vSZPs being provided in TURBOMOLE format together witht the required ECP file. <br>
The q<sup>avg</sup>-vSZPs basis set is provided in TURBOMOLE and ORCA format.

## Automatic input file generation for the q-vSZPs

### Precomputed atomic charges
To compute the atomic charges needed for the basis setup, the multicharge software can be used (https://github.com/thfroitzheim/multicharge)
The EEQ_BC charges as used in the orignal publication [TBD] can be reproduced using the `eeqbc2026-q-vSZPs-paper` branch under (https://github.com/benbaed/multicharge-q-vSZPs-paper)


For a given file `xyz` format, the EEQ<sub>BC</sub> charges can be computed using
```
multicharge -m eeqbc -json struc.xyz
```
(the `coord` format is also accepted, see `mctc-lib` (https://github.com/grimme-lab/mctc-lib) for conversion from other formats)

For full automatation, the JSON formatter provided under (https://stedolan.github.io/jq) can be used to write the `ext.charges` file
```
jq -c '.charges[]' multicharge.json > ext.charges
```
The `ext.charges` file now contains the atomic charges for each atom in the order of the structure file.

### Basis set setup and Input generation
For an automatic setup of the basis set and generation of an ORCA input file, the `q-vSZP` software (https://github.com/grimme-lab/qvSZP) can be utilized.
Example program command-line call:

```
qvSZP --struc struc.xyz --bfile q-vszps --efile ecpq --cm extonlyq --v --outname orca --conv TightSCF --defgrid 3 --d4par 1.0 1.1710 0.3478 5.5828 1.50
```
Here, the `--cm extonlyq` option instructs the code to read the external charge file named `ext.charges` (see previous step) <br>

An alternative input generation using the CEH charge model (does not required an external charge file):
```
qvSZP --struc struc.xyz --bfile q-vszps --efile ecpq --cm ceh --v --outname orca --conv TightSCF --defgrid 3
```

`--bfile` and `--efile` provide the locations of the basis set and ecp files respectively (in TURBOMOLE format) <br>
For an overview of the other options use `--help`

## Citation

Benedikt Bädorf, Marcel Müller, Thomas Froitzheim and Stefan Grimme, **2026** <br>
chemrxiv: [10.26434/chemrxiv.15003662/v1](https://chemrxiv.org/doi/full/10.26434/chemrxiv.15003662/v1)

For the q-vSZP basis set: <br>
Marcel Müller, Andreas Hansen and Stefan Grimme, *J. Chem. Phys.*, **2023**, 159, 164108. <br>
DOI: [10.1063/5.0172373](https://doi.org/10.1063/5.0172373)

For the bond capacity electronegativity equilibration charge model (EEQ<sub>BC</sub>):  <br>
Thomas Froitzheim, Marcel Müller, Andreas Hansen, and Stefan Grimme, *J. Chem. Phys.*, **2025**, 162, 214109. <br>
DOI: [10.1039/10.1063/5.0268978](https://doi.org/10.1063/5.0268978) <br>
chemrxiv: [10.26434/chemrxiv-2025-1nxwg](https://doi.org/10.26434/chemrxiv-2025-1nxwg)

For the charge extended Hückel model (CEH): <br>
Marcel Müller, Thomas Froitzheim, Andreas Hansen, and Stefan Grimme, *J. Phys. Chem. A* **2024**, 128, 49, 10723–10736 <br>
DOI: [10.1063/5.0268978](https://doi.org/10.1021/acs.jpca.4c06989)
