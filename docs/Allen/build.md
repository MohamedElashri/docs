# Allen

## Build Allen on CERN Lxplus 

- Setup the environment

```bash
source /cvmfs/lhcb.cern.ch/lib/LbEnv
```

- Clone Allen from CERN gitlab

```bash
git clone https://gitlab.cern.ch/lhcb/Allen.git Allen
```

- Build Allen

```bash
cd Allen
mkdir build
cd build
cmake -DSTANDALONE=ON -DCMAKE_TOOLCHAIN_FILE=/cvmfs/lhcb.cern.ch/lib/lhcb/lcg-toolchains/LCG_101/x86_64-centos7-clang12-opt.cmake ..
```

- Build the project

``` bash
make -j 4
```

-  Run a test to see if it works 

```bash
toolchain/wrapper ./Allen --sequence=hlt1_pp_default
```

## Build Allen on Sleepy

- Setup the environment

```bash
source /cvmfs/lhcb.cern.ch/lib/var/lib/LbEnv/stable/linux-64/bin/activate
```

- Clone Allen from CERN gitlab

```bash
git clone https://gitlab.cern.ch/lhcb/Allen.git Allen
```

- Build Allen

if you want to build for CPU then run

```bash
cd Allen
mkdir build
cd build
cmake -DSTANDALONE=ON -DCMAKE_TOOLCHAIN_FILE=/cvmfs/lhcb.cern.ch/lib/lhcb/lcg-toolchains/LCG_101/x86_64-centos7-clang12-opt.cmake ..
```

but if you want to build for GPU 

```bash
cd Allen
mkdir build
cd build
cmake -DSTANDALONE=ON -DCMAKE_TOOLCHAIN_FILE=/cvmfs/lhcb.cern.ch/lib/lhcb/lcg-toolchains/LCG_101/x86_64-centos7-clang12+cuda11_4-opt.cmake ..
```


- Build the project

``` bash
make -j 4
```

-  Run a test to see if it works 

```bash
toolchain/wrapper ./Allen --sequence=hlt1_pp_default
```
