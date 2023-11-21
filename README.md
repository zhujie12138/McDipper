# McDIPPER (Monte-Carlo Dipole Event-geneRator )

The ```McDIPPER``` is a saturation-based heavy-ion initial condition MC generator. It used the input from diverse saturation models and fits to produce a 3D energy and charge deposition. 

## Requisites 
gcc>9.3 will be better for compilation

### GSL
gsl2.7.1 works.

The ```McDIPPER``` uses the GSL library for integration routines, as well as for the evaluation of special functions. The GSL library can be found [here](https://www.gnu.org/software/gsl/)

### LHAPDF 
latest version: such as 6.5.4 (install the package first, then download the relative PDF sets)

The ```McDIPPER``` uses the LHAPDF library, which allows a simple way to compare results for different parton distribution function (PDF) sets. To compile the code succesfully, one needs to have installed the LHAPDF library. In their [page](https://lhapdf.hepforge.org/install.html), one can find that the LHAPDF lib can be installed a
```
wget https://lhapdf.hepforge.org/downloads/?f=LHAPDF-6.X.Y.tar.gz -O LHAPDF-6.X.Y.tar.gz
tar xf LHAPDF-6.X.Y.tar.gz
cd LHAPDF-6.X.Y
./configure --prefix=/path/for/installation
make
make install
```

PDF SET download: wget http://lhapdfsets.web.cern.ch/lhapdfsets/current/EPPS16nlo_CT14nlo_Pb208.tar.gz -O- | tar xz -C /home/lhapdf/share/LHAPDF/
PDF SET like:
EPPS16nlo_CT14nlo_Pb208
CT18NNLO
CT14nnlo
CT18NLO


### CUBA

The ```McDIPPER``` uses also the CUBA library for multi-dimensional integration. Please follow the instructions for installing, which can be found [here](https://feynarts.de/cuba/). The header needed for compilation is included in this package under the ``` src/external ``` folder. 

Installation: one pdf in it will show.
./configure
make

## How to compile

On Mac:
The compilation of a build can be performed following the following commands  
```
make builder BUILD=path/to/build/fldr
cd path/to/build/fldr
make
```

On linux: just make
maybe you need to add some libpath of your device at the 6th line of the Makefile



and to run
```
./mcDip -i /path/to/config.yaml
```
This will change, as I change the configuration option.

## The structure of the code

The main class is Event.cpp, which produces the initial stage quantities by using the nucleus and charges classes. The ``` event ``` class calls the ``` charges ``` class , which imports the already pre-computed quantities 

- eg: Energy density from single gluon production 
- eq: Energy density from single quark production 
- nu: Charge density of a u-quark
- nd: Charge density of a d-quark
- ns: Charge density of a s-quark

In the case it does not find them, the model classes (gbw, ipsat) are called to compute the tabulation of charges. These conserved charges are computed then in terms of η, T1,T2. These are stored in the ``` tabs ``` folder, where they can be accessed and reused. For the default parameters, each set is around 30-40 MB. Once these are computed, they are interpolated and can be accessed fast to produce the event. 

## Input 

The ```McDIPPER``` needs the tabulated IP-Sat dipole functions, which are given for two parameters sets from [Rezaian et al](https://arxiv.org/abs/1212.2974).

For the input needed in the simulation please see the example configuration files in the ``` configs ``` for examples of configuration files. 

## Output 

There are three flags for 


## Analysis suite

Additionally, I have included a set of python classes that can help the user analyze data. These are stored in the ``` analysis ``` folder, and consist of

```
analysis/analysis
```

## Dealing with issues in MacOS 

For usage in the mac environment, a couple of issues may arise when installing the LHAPDF. The python version installed in most the newest iOS is incompatible with the scripts from LHAPDF. This can be circumvented for the needs of this code by installing LHAPDF using ``` --disable-python ``` when installing LHAPDF.


## Acknowledgments

- This work is supported by the Deutsche Forschungsgemeinschaft (DFG, German Research Foundation) through the CRC-TR 211 'Strong-interaction matter under extreme conditions'– project number 315477589 – TRR 211.

