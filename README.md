<img src="https://raw.githubusercontent.com/zorkzou/Molden2AIM/master/m2a-logo.png" />

# Molden2AIM
Molden2AIM is a utility program which can be used to create AIM-WFN, AIM-WFX, and NBO-47 files from a Molden file.

## Recent Changes
Version 5.0.2 (10/09/2020).

1. The MOLDEN file with H-functions has been supported if it is generated by [Dalton](http://daltonprogram.org/).
2. The utility `ReOrdAtm` has been merged into Molden2AIM and runs automatically.
3. For the MOLDEN file saved by [ORCA](https://orcaforum.kofo.mpg.de/), `[PROGRAM] orca` in MOLDEN and `program=0` in `m2a.ini` are not needed in the case of the default title `Molden file created by orca_2mkl for BaseName=...`.

Version 5.0.0 (06/05/2020).

1. If possible, save a new MOLDEN file or NBO-47 file in spherical functions. `carsph=1` in `m2a.ini` is required.
2. The MOLDEN file with H-functions has been supported if it is generated by [Multiwfn](http://multiwfn.codeplex.com/), [ORCA](https://orcaforum.kofo.mpg.de/), or [CFour](http://www.cfour.de/) (Ver. 2.1).
3. If possible, the `$LCAOMO` and `$FOCK` blocks will be printed in the NBO-47 file (`nbopro=1` in `m2a.ini` is required), so the Second Order Perturbation Theory Analysis may be performed by [NBO](http://nbo7.chem.wisc.edu/) for the RHF, UHF, RKS, and UKS types of wavefunctions.
4. Orthogonality will be checked if the `$FOCK` block exists in the NBO-47 file.
5. Bug fix for modern Fortran compilers.

Version 4.4.0 (05/27/2020).

1. A new X2C/PBE0 EDF library (by Chun Gao) can take core correlations into account, which may be requested by `edftyp=1` in `m2a.ini`. Some test calculations of noble gas atoms with 22 functionals showed that PBE0 can reproduce the core densities of CCSD(T,full) with the best agreements.
2. The initialization file `m2a.ini` may be generated automatically if it doesn't exist.
3. The fitting program denfit.f90 has been modified to improve the accuracy.
4. Bug fix: energies in the WFN file were wrong.

Version 4.3.0 (02/09/2019).

1. The Molden file generated by StoBe has been supported.
2. The Molden file generated by Crystal (molecule only) has been supported through `[Program] crystal` in MOLDEN or `PROGRAM=10` in m2a.ini.
3. The number of core electrons may also be specified in the terminal.

Version 4.2.1 (05/11/2018).

1. The EDF library has been updated for the following cores/elements: ncore = 2 (B), 10 (Na), 28 (Cu, Pd, I, Xe, Cs, Sm, Eu, Gd, Tb), 46 (Cd, Xe), 78 (Pa, Es, Fm), and 92 (Cn, Nh). It's found that these old EDFs may produce a local minimum at R = 0 and lead to a (3,+3) critical point wrongly. Thank Dr. Tian Lu for reporting the problem.
2. The fitting program denfit.f90 has been modified for the above problem.

## Features

* It converts the data format from Molden to AIM's WFN. The latter format can be read by [AIMPAC](http://www.chemistry.mcmaster.ca/aimpac/imagemap/imagemap.htm), [AIMPAC2](http://www.beaconresearch.org/AIMPAC2/index.html), [AIM2000](http://www.aim2000.de/), [AIMALL](http://aim.tkgristmill.com/), [AIM-UC](http://alfa.facyt.uc.edu.ve/quimicomp/), [Critic2](http://schooner.chem.dal.ca/wiki/Critic2), [DensToolKit](https://sites.google.com/site/jmsolanoalt/software/denstoolkit), [DGrid](http://www.cpfs.mpg.de/~kohout/dgrid.html), [MORPHY98](http://morphy.mib.man.ac.uk/), [Multiwfn](http://multiwfn.codeplex.com/), [ORBKIT](https://orbkit.github.io/), [PAMoC](http://www.istm.cnr.it/~barz/pamoc/), [ProMolden](http://azufre.quimica.uniovi.es/d-DensEl/), [TopChem](http://www.lct.jussieu.fr/pagesperso/pilme/topchempage.html), [TopMoD](http://www.lct.jussieu.fr/pagesperso/silvi/topmod.html), [Xaim](http://www.quimica.urv.es/XAIM/), and so on. The GAB file of [Gabedit](http://gabedit.sourceforge.net/) is compatible.
* It saves [NBO](http://nbo7.chem.wisc.edu/)'s *.47 data file. One can do NBO analysis using the stand-alone [GENNBO](http://nbo7.chem.wisc.edu/) program. In addition, the following loops can be performed using [NBO](http://nbo7.chem.wisc.edu/) or [NBO2Molden](https://github.com/zorkzou/NBO2Molden). However the results may be different since [NBO](http://nbo7.chem.wisc.edu/) saves natural bond orbitals (NBOs) into the MOLDEN file by default.

<img src="https://raw.githubusercontent.com/zorkzou/Molden2AIM/master/m2a-loop.png" />

* After the *.47 file being generated, it can calculate the generalized Wiberg bond order indices (GWBO) in MO (see I. Mayer, C.P.L. 97, 270, 1983). In the case of closed-shell system, they are the Mayer bond orders (MBO) in MO.
* It saves AIM's [WFX data file](http://aim.tkgristmill.com/wfxformat.html), which can be read by [AIMALL](http://aim.tkgristmill.com/), [Critic2](http://schooner.chem.dal.ca/wiki/Critic2), [DensToolKit](https://sites.google.com/site/jmsolanoalt/software/denstoolkit), [GPView](http://life-tp.com/gpview/), [Multiwfn](http://multiwfn.codeplex.com/), or [ORBKIT](https://orbkit.github.io/). There are two versions of atomic EDF library for Z = 3-120 controlled by `edftyp` in `m2a.ini`: the default X2C/HF version by `edftyp=0` (see W. Zou, Z. Cai, J. Wang, K. Xin, An open library of relativistic core electron density function for the QTAIM analysis with pseudopotentials, J. Comput. Chem. 2018, 39, 1697-1706) and the X2C/PBE0 version by `edftyp=1`.

## Compilation

    > F90 -O3 edflib.f90 edflib-pbe0.f90 molden2aim.f90 -o molden2aim.exe

where `F90` can be `gfortran`, `pgf90`, `ifort`, or other Fortran90 compilers.

## Running Molden2AIM

-   Windows

1. Put `molden2aim.exe` and MOLDEN/Gabedit files into the same folder.
2. If necessary, insert a `[Program] program_name` line into the MOLDEN file, or edit the `program` parameter in `m2a.ini` (you can also setup other parameters there).
3. If ECP or MCP is used, insert a `[Core]` or `[Pseudo]` segment into the MOLDEN/Gabedit file. See below for the format and examples.
4. Double-click `molden2aim.exe`, and then type in the MOLDEN/Gabedit file name.

-   Unix/Linux/MacOS

1. Put `molden2aim.exe` and MOLDEN/Gabedit files into the same folder.
2. If necessary, insert a `[Program] program_name` line into the MOLDEN file, or edit the `program` parameter in `m2a.ini` (you can also setup other parameters there).
3. If ECP or MCP is used, insert a `[Core]` or `[Pseudo]` segment into the MOLDEN/Gabedit file. See below for the format and examples.
5. In the terminal, type in

    > ./molden2aim.exe

   and then type in the MOLDEN/Gabedit file name.

## ECP/MCP

In the case of ECP or MCP, a segment of `[Core]` should be defined in the MOLDEN file. The format is

		[Core]
		Iatom : Ncore     or    Element: Ncore
		...

where Ncore is the number of core electrons replaced by ECP or MCP. Atom/element with Ncore=0 can be ignored. For example, for a cluster with the atoms N_1, N_2, N_3, Pt_4, and Pt_5, it can be

		[Core]
		Pt: 60
		N : 2
		2 : 0

This means that the numbers of core electron are 60 in Pt_4 and Pt_5 and 2 in N_1 and N_3. In N_2 the number of core electron is set to 2 but then reset to 0. It is equivalent to

		[Core]
		1 : 2
		3 : 2
		4 : 60
		5 : 60

Another way is to define a segment of `[Pseudo]` in the MOLDEN file, which is supported by [Molden](http://www.cmbi.ru.nl/molden/). The format is

    [Pseudo]
    Name1   IAtom1   ZA1-Ncore1
    Name2   IAtom2   ZA2-Ncore2
		...

## About the Molden file

MOLDEN (or GAB) files generated by the the following programs are fully or partly supported by Molden2AIM at present.

* [ACES-II](http://www.qtp.ufl.edu/ACES/) (> 2.9)
* [BDF-G](http://182.92.69.169:7226/)
* [CADPAC](https://en.wikipedia.org/wiki/CADPAC)
* [CFour](http://www.cfour.de/)
* [Columbus](http://www.univie.ac.at/columbus/)
* [Crystal](http://www.crystal.unito.it/) (molecule only)
* [DALTON](http://daltonprogram.org/) (> 2013)
* [deMon2k](http://www.demon-software.com/public_html/)
* [Firefly](http://classic.chem.msu.su/gran/gamess/), through the utility [Molden](http://www.cmbi.ru.nl/molden/) or [Gabedit](http://gabedit.sourceforge.net/). See [molden_gabedit.jpg](https://raw.githubusercontent.com/zorkzou/Molden2AIM/master/molden_gabedit.jpg).
* [Gaussian](http://www.gaussian.com/), through the utility [Molden](http://www.cmbi.ru.nl/molden/) or [Gabedit](http://gabedit.sourceforge.net/). See [molden_gabedit.jpg](https://raw.githubusercontent.com/zorkzou/Molden2AIM/master/molden_gabedit.jpg).
* [Gamess](http://www.msg.chem.iastate.edu/gamess/), through the utility [Molden](http://www.cmbi.ru.nl/molden/) or [Gabedit](http://gabedit.sourceforge.net/). See [molden_gabedit.jpg](https://raw.githubusercontent.com/zorkzou/Molden2AIM/master/molden_gabedit.jpg).
* [Gamess-UK](http://www.cfs.dl.ac.uk/), through the utility [Molden](http://www.cmbi.ru.nl/molden/). See [molden_gabedit.jpg](https://raw.githubusercontent.com/zorkzou/Molden2AIM/master/molden_gabedit.jpg).
* [MOLCAS](http://www.molcas.org)
* [MOLPRO](http://www.molpro.net/)
* [MRCC](http://www.mrcc.hu/)
* [Multiwfn](http://multiwfn.codeplex.com/)
* [NBO](http://nbo7.chem.wisc.edu/) (> May.2014)
* [NWChem](http://www.nwchem-sw.org/), (>= Ver. 6.8) by MOLDEN_NORM JANPA or NONE to generate a MOLDEN file. See the attached examples.
* [ORCA](https://orcaforum.kofo.mpg.de/)
* [Priroda](http://wt.knc.ru/wiki/index.php/Priroda_Documentation)
* [PSI4](http://www.psicode.org/)
* [PySCF](https://github.com/sunqm/pyscf)
* [Q-Chem](http://www.q-chem.com/)
* [StoBe](http://www.fhi-berlin.mpg.de/KHsoftware/StoBe/index.html)
* [TeraChem](http://www.petachem.com/)
* [Turbomole](http://www.turbomole.com/)

See [readme.html](https://zorkzou.github.io/Molden2AIM/readme.html) for details.

Examples of applications can be found in W. Zou, D. Nori-Shargh, and J. E. Boggs, On the Covalent Character of Rare Gas Bonding Interactions: A New Kind of Weak Interaction, J. Phys. Chem. A 117, 207-212 (2013); Erratum: J. Phys. Chem. A 120, 2057-2057 (2016).

The EDF library (X2C/HF version) was published in W. Zou, Z. Cai, J. Wang, and K. Xin, An open library of relativistic core electron density function for the QTAIM analysis with pseudopotentials, J. Comput. Chem. 39, 1697-1706 (2018).
