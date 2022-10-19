# HIGLU and HDECAY
The total cross section for Higgs boson production via the dominant gluon fusion mechanism including NLO [two-loop] QCD corrections can be calculated numerically with the program HIGLU.
The QCD corrections are included for arbitrary Higgs and quark masses and increase the cross section at the LHC by up to a factor of 2.
The source code HIGLU provides the evaluation of the production of the Standard Model [SM] Higgs boson as well as the neutral Higgs bosons of the minimal supersymmetric extension [MSSM].
The program HDECAY determines the decay widths and branching ratios of the Higgs bosons within the SM and the MSSM, including the dominant higher-order corrections. 

1) HIGLU setup:

The source code of the HIGLU program is written in FORTRAN. As a first step, make sure to setup a proper compilation of gcc/gfortran/python: 
   ```
   cmsrel CMSSW_10_6_19
   cd CMSSW_10_6_19/src
   source /cvmfs/sft.cern.ch/lcg/contrib/gcc/7.3.0/x86_64-centos7-gcc7-opt/setup.sh	
   ```
Then, proceed to the download and compilation of the HIGLU program:
   ```
   mkdir HIGLU; cd HIGLU
   wget http://tiger.web.psi.ch/higlu/higlu.tar.gz
   tar -xzvf higlu.tar.gz
   make
   ```

2) PDF setup:

LHAPDF no longer bundles PDF set data in the package tarball. The sets are instead all stored online at http://lhapdfsets.web.cern.ch/lhapdfsets/current.
You should install those that you wish to use manually.

Download and install LHAPDF-6.3.0 following these links in the `higlu` directory.

  * https://lhapdf.hepforge.org/downloads/?f=pdfsets/v6.backup/6.2.1
  * https://lhapdf.hepforge.org/downloads?
  * https://lhapdf.hepforge.org/lhapdf5/install

A summary of the steps is reported here as an example:
   ```
   export PATH=$PWD/build/bin:$PATH	
   export LD_LIBRARY_PATH=$PWD/build/lib:$LD_LIBRARY_PATH
   export PYTHONPATH=$PWD/build/lib/python2.7/site-packages:$PYTHONPATH
   mkdir LHAPDF-6.3.0-install
   cd LHAPDF-6.3.0	
   ./configure --prefix=PATHTO/HIGLU/LHAPDF-6.3.0-install
   wget http://www.hepforge.org/archive/lhapdf/pdfsets/6.2/NNPDF30_nlo_as_0118.tar.gz -O- | tar xz -C PATHTO/LHAPDF-6.3.0-install/share/LHAPDF
   wget http://www.hepforge.org/archive/lhapdf/pdfsets/6.2/NNPDF30_nnlo_as_0118.tar.gz -O- | tar xz -C PATHTO/LHAPDF-6.3.0-install/share/LHAPDF
   make
   make install
   ```

Then edit the makefile in the HIGLU directory and include the correct path to specify the `LIBS` and export the relevant `PATHS` and libraries:
   ```
   export LHAPDF=/PATHTO/LHAPDF-6.3.0-install/share/LHAPDF/
   export LHAPDF_DATA_PATH=/PATHTO/LHAPDF-6.3.0-install/share/LHAPDF/
   export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:PATHTO/HIGLU/LHAPDF-6.3.0-install/lib
   ```
and check that the `LD_LIBRARY_PATH` is properly modified:
   ```
   echo $LD_LIBRARY_PATH
   ```

3) Running HIGLU

To run HIGLU, settings can be changed in the `higlu.in` configuration file, for example the mass and type of the mediator (M_HIGGS, TYPE) or the scales (MU_1 and MU_2). Then: 
   ```
   make
   ./run higlu.in
   ```
   
4) Documentation:

**HIGLU**
  * http://tiger.web.psi.ch/higlu/
  * https://arxiv.org/abs/hep-ph/9610350

**HDECAY**
  * https://arxiv.org/pdf/hep-ph/9704448.pdf
  * https://arxiv.org/pdf/1810.00768.pdf


