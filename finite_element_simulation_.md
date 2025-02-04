# Introduction
The EVPSC models (both $\Delta$ and $\sigma$-EVPSC) have been interfaced with a commercial finite element solver (Abaqus/standard).

## 1. Building & running
### 1-1. Material characteristics
One needs have [```FNTX```](texture_file.md) and [```FNSX```](single_crystal_file.md).
Also, an Abaqus input file is required.

### 1-2. Abaqus input file
Abaqus input file should indicate that user material is used.
In the Abaqus input file, the *depvar*, *uvar* variables should be given.
In the below snippet of an Abaqus input file, a material information keyword (***MATERIAL**) suitable for FE simulation using EVPSC is given:
```text
*MATERIAL, name=MyUserMaterial
*DEPVAR
1932
*USER MATERIAL, constants=1
0.,
*USER OUTPUT VARIABLES
30
```
The name of material is 'MyUserMaterial', and the number of dependent variables (***DEPVAR**) is set 1932 in the above.
It should be indicated that this material is described via user material subroutine (***USER MATERIAL**).
The user variable (**NUVARM**) is set to 30.

Example Abaqus files are located in [```umat/src/evpsc_incr/inps```](../umat/src/evpsc_incr/inps).

### 1-3. Using [run.py](../umat/src/evpsc_incr/run.py)
```
python -u run.py --help
```
Run Abaqus/standard or explicit simulation using dEVPSC-FE umat (or vumat)
This script allows to neatly run dEVPSC-FE with prescribed conditions saved in the given files that include necessary conditions.
 - Explicit version is under progress...
          Youngung Jeong; yjeong@changwon.ac.kr

| Optional Arguments       | Description                                                             |
|:-------------------------|:------------------------------------------------------------------------|
| `--h` `--help`           | Show help message and exit                                              |
| `--fn FN`                | Input file                                                              |
| `--cpus CPUS`            | Total number of CPUs                                                    |
| `--ngrs NGRS [NGRS ...]` | The total number of grains pereach phase                                |
| `--irgvb`                | Use RGVB hardening model                                                |
| `--idd`                  | Use DD hardening model                                                  |
| `--j2`                   | Run with J2 theory but with the flow curve from dEVPSC                  |
| `--j2strfn J2STRFN`      | STR_STR.OUT file if exists (optional)                                   |
| `--isa`                  | Build and run SA (in addition to FE)                                    |
| `--pathwork PATHWORK`    | Path name, in which FE is to run                                        |
| `--inp INP`              | Abaqus input file                                                       |
| `--elastmod`             | Calculate elastic modulus (with 10,000 grains per each phase) and exit  |
| `--usermat USERMAT`      | Your user material subroutine                                           |
| `--idtderiv IDTDERIV`    | 0: old way; 1: new way                                                  |
| `--iregime IREGIME`      | 2: delta-EVPSC, 3: sigma-EVPSC                                          |

### 1-4. Input to via dEVPSC-FE run via UMAT
INPUT file must contain the input data necessary to run UMAT.

```
** input to run dEVPSC-FE run via UMAT
* iodf (0: discrete texture; 1: odf (mtex format); 2: random texture) 
2
* texture/ODF file;; eddq_push/EDDQ100
../../../matData/vpscData/EDDQ/eddq.odf
* sx file
eddq_push/eddq_yj.sx
* idiff (0: no diffraction; 1: diffraction)
0
* diff file (dummy if idiff = 0 is given in the above line)
matData/lc/bcc.dif
* abaqus input file
inps/UMAT_DEV_VEL_0001.inp
* noel and npt subjected to 'lookup' (could be dummy unless <isa> flag is on)
1
1 1
* ipx
0
* pxfn (dummy if ipx is 0)
dummy
```
- iodf : Which type of solver
- texture/ODF file : Name of odf file 
- sx file : Name of single crystal file
- idiff : Use of diffraction
- diff file : Name of diffraction file
- abaqus input file : Name of abaqus inp file


## 2. Post-analysis
One can generate "*.odf" file and use Abaqus CAE.
Other method based on several Python scripts is also available.

## 3. Test build
For test-building, use ```umat/src/evpsc_incr/build_umat.py```.
An example usage is:
```sh
$ python -u build_umat.py --isolver 0 --ngrs 100 --iodf 0 --irollback --idtderiv 0 --iregime 2
```
Note that, when building **UMAT**, many subroutines located in ```./src/vpsc_src/``` folder as well as ```./umat/src/evpsc_incr/src/``` are modified such that the implicit none statement at the beginning of each subroutine, such as below:
```fortran
implicit none
```
is changed to:
```fortran
include 'ABA_PARAM.INC`
```
With ```--irollback``` option, changes in those source files are _rolled back_ so that ```implicit none``` is recovered.

## 4. Running with [```umat/src/evpsc_incr/run.py```](../umat/src/evpsc_incr/run.py)
One can use ```run.py``` python script located in ```umat/src/evpsc_incr/``` directory.
This script is designed to help easy build & run by utilizing running conditions listed in a separate text file, e.g., [```umat/src/tests/umat.in```](../umat/src/evpsc_incr/tests/umat.in).o

## 5. References

| Bibliographic info.                                                                                  |  Brief summary                                   |
|:-----------------------------------------------------------------------------------------------------|:-------------------------------------------------|
| [Y. Jeong and C. N. Tomé](https://doi.org/10.1016/j.ijplas.2020.102812), Int. J. Plast. (2020)       | $\Delta$ EVPSC model development                 |
| [Y. Jeong, B. Jeon, C. N. Tomé](https://doi.org/10.1016/j.ijplas.2021.103110), Int. J. Plast. (2021) | FE implementation of $\Delta$ EVPSC              |
| [B. Jeon, Y. Jeong, C. N. Tomé](https://dx.doi.org/10.2139/ssrn.4969819), submitted                  | Enhancement in $\Delta$ and $\sigma$ EVPSC models|
