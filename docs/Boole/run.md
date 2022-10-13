## Boole Documentation
The purpose of this page is to collect some new documentation for important aspects about LHCb digitization software called `Boole`. 
All of this assumes that you have already a current LHCb software installation (i.e on `lxplus`)

### Quick Setup

- Set up Gauss: `lb-dev Boole/v41r2` 
you can decide which version specifying what is after Boole so it is like `Boole/<version>`

- cd to the Boole dir `cd ./BooleDev_v41r2`

- run `git lb-use Boole`

- run `make configure $$ make install`


This can be done on one command like 

``` bash
lb-dev Boole/v49r21 \\
cd ./BooleDev_v41r2 \\ 
git lb-use Boole \\
make configure  \\
make install
```

### Example 

In this example, I will take the `.sim` file from LHCb starter kit example and run it through `Boole` to create the digitization. 
The startedkit example which deals with this decay $\left.D^{*+} \rightarrow D^{0}\left(\rightarrow K^{+} K^{-} \mu^{+} \mu^{-}\right) \pi^{+}\right)$.

Following the same tutorial I wrote about Gauss [here](https://github.com/LHCb-Cincinnati/software/blob/main/LHCb/Gauss/README.md). The idea is similar, you pass
the options to `/.run` script or you can have job file like `Boole.Job.py` which contains everything. I will do the later becuase it is easier to debug and more organized. 

This is a very minimal Boole job file 

``` python
from Gaudi.Configuration import *
from Configurables import Boole, LHCbApp


from Gaudi.Configuration import *
importOptions("$APPCONFIGOPTS/Boole/Default.py")
importOptions("$APPCONFIGOPTS/Boole/DataType-2015.py")
importOptions("$APPCONFIGOPTS/Boole/Boole-SetOdinRndTrigger.py")
importOptions("$APPCONFIGOPTS/Persistency/Compression-ZLIB-1.py")
importOptions("$APPCONFIGOPTS/Boole/SMOG.py")



LHCbApp().DDDBtag   = "dddb-20170721-3"
LHCbApp().CondDBtag = "sim-20170721-2-vc-md100"

Boole().DatasetName = "Example"
HistogramPersistencySvc().OutputFile = "example.root"

EventSelector().Input = ["DATAFILE='PFN:/afs/cern.ch/user/m/melashri/public/sim_tutorial/Gauss-27175000-5ev-20220526.sim', TYP='POOL_ROOTTREE' OPT='READ'"]
OutputStream("DigiWriter").Output = "DATAFILE='PFN:Gauss_example.xdigi' TYP='POOL_ROOTTREE' OPT='REC'"
OutputStream("DigiWriter").OptItemList += [ '/Event/Gen/HepMCEvents#1' ]
```

To run the following Simulation you cd to Boole dir and run the following:

``` bash
./run gaudirun.py ../Boole-Job.py
```

Hint: you might need to run `lhcb-proxy-init` to obtain the permissions to run Boole.


## Resources

- [ Simulation in LHCb Twiki ](https://twiki.cern.ch/twiki/bin/view/LHCb/LHCbSimulation)
