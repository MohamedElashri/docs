# Gauss Documentation
The purpose of this page is to collect some new documentation for important aspects about LHCb simulation software called `Gauss`. All of this assumes that you have already a current LHCb software installation (i.e on `lxplus`)



## Quick Setup

- Set up Gauss: `lb-dev Gauss/v49r21` 
you can decide which version specifying what is after Gauss so it is like `Gauss/<version>`

- cd to the Gauss dir `cd ./GaussDev_v49r21`

- We will need the `DecFiles` package which is available on CERN Gitlab (this is where the pre-defined decay is there)
`git lb-clone-pkg Gen/DecFiles`

- We now build Gauss by running `make`

This can be done on one command like 

```
lb-dev Gauss/v49r21 \\
cd ./GaussDev_v49r21 \\ 
git lb-clone-pkg Gen/DecFiles \\
make 
```
Hint: This will install `Sim09`

## starterkit Example 

To run the startedkit example which deals with this decay $\left.D^{*+} \rightarrow D^{0}\left(\rightarrow K^{+} K^{-} \mu^{+} \mu^{-}\right) \pi^{+}\right)$. This decay is pre-defined in Gauss and is given the event type # `27175000`. This is determined by the `Decfiles` library. The `DecFile` controls the decay itself (i.e. what EvtGen does) as well as provide any event-type specific configuration (e.g. generator cuts). They exist in the `Gen/DecFile` package. The content of the Decfile is the following:


```bash
# EventType: 27175000                                                                                                                                                                                                                                      
#                                                                                                                                                                                                                                                          
# Descriptor: [D*(2010)+ -> (D0 -> {K+ K- mu+ mu-}) pi+]cc                                                                                                                                                                                                 
#                                                                                                                                                                                                                                                          
# NickName: Dst_D0pi,KKmumu=DecProdCut                                                                                                                                                                                                                     
#                                                                                                                                                                                                                                                          
# Cuts: DaughtersInLHCb                                                                                                                                                                                                                                    
#                                                                                                                                                                                                                                                          
# Documentation: Forces the D* decay in generic b-bbar / c-cbar events + Requires products to be in LHCb acceptance                                                                                                                                        
# EndDocumentation                                                                                                                                                                                                                                         
#                                                                                                                                                                                                                                                          
# PhysicsWG: Charm                                                                                                                                                                                                                                         
# Tested: Yes                                                                                                                                                                                                                                              
# Responsible: Luisa Arrabito                                                                                                                                                                                                                              
# Email: unknown@<nospam>cern.ch                                                                                                                                                                                                                           
# Date: 20091215                                                                                                                                                                                                                                           
#                                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                                           
Alias MyD0 D0                                                                                                                                                                                                                                              
Alias MyantiD0 anti-D0                                                                                                                                                                                                                                     
ChargeConj MyD0 MyantiD0                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                           
Decay D*+sig                                                                                                                                                                                                                                               
  1.000 MyD0  pi+    VSS;                                                                                                                                                                                                                                  
Enddecay                                                                                                                                                                                                                                                   
CDecay D*-sig                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                           
Decay MyD0                                                                                                                                                                                                                                                 
  1.000 K+ K- mu+ mu- PHSP;                                                                                                                                                                                                                                
Enddecay                                                                                                                                                                                                                                                   
CDecay MyantiD0                                                                                                                                                                                                                                            
#                                                                                                                                                                                                                                                          
End  
```

This Decfile code contains the Information which tells Gauss what to except when calling `<event-type>.py` as an option. There is a current parser [online](http://lbeventtype.web.cern.ch/) to help you with event type. 


To run Gauss you need to specify the options which tells Gauss about what you need. you can pass the options using the syntax 

./run gaudirun.py `option` Gauss-Job.py

But the best way is to include all the options in `Gauss-Job.py` which is where you need to call things. For example 


Instead of running

```
./run gaudirun.py 
        '$APPCONFIGOPTS/Gauss/Beam6500GeV-md100-2016-nu1.6.py' \  # Sets beam energy and position
        '$APPCONFIGOPTS/Gauss/EnableSpillover-25ns.py' \  # Enables spillover (only Run2)
        '$APPCONFIGOPTS/Gauss/DataType-2016.py' \
        '$APPCONFIGOPTS/Gauss/RICHRandomHits.py' \  # Random hits in RICH for occupancy
        '$DECFILESROOT/options/{eventnumber}.py' \  # Event type containing the signal
        '$LBPYTHIA8ROOT/options/Pythia8.py' \  # Setting Pythia8 as generator
        '$APPCONFIGOPTS/Gauss/G4PL_FTFP_BERT_EmNoCuts.py' \  # Physics simulated by Geant4
        Gauss-Job.py
```

You can  define your own options in `Gauss-Job.py` As the fOllowing:

```
from Gauss.Configuration import *
importOptions("$APPCONFIGOPTS/Gauss/Beam6500GeV-md100-2016-nu1.6.py")
importOptions("$APPCONFIGOPTS/Gauss/EnableSpillover-25ns.py")
importOptions("$APPCONFIGOPTS/Gauss/DataType-2016.py")
importOptions("$APPCONFIGOPTS/Gauss/RICHRandomHits.py")
importOptions("$DECFILESROOT/options/27175000.py")
importOptions("$LBPYTHIA8ROOT/options/Pythia8.py")
importOptions("$APPCONFIGOPTS/Gauss/G4PL_FTFP_BERT_EmNoCuts.py")


GaussGen = GenInit("GaussGen")
GaussGen.FirstEventNumber = 1
GaussGen.RunNumber = 1082

from Configurables import LHCbApp
LHCbApp().DDDBtag = 'dddb-20170721-3'
LHCbApp().CondDBtag = 'sim-20170721-2-vc-md100'
LHCbApp().EvtMax = 5
```


To run the following Simulation you cd to Gauss dir and run the following: 

```
./run gaudirun.py ../Gauss-Job.py
```

Which will be equivalent to 
``` bash
./run gaudirun.py '$APPCONFIGOPTS/Gauss/Beam6500GeV-md100-2016-nu1.6.py'  '$LBPYTHIA8ROOT/options/Pythia8.py' '$DECFILESROOT/options/27175000.py' ../Gauss-Job.py
```

Which of course will depend on where you have `Gauss-Job.py` file

Hint: you will need first to run `lhcb-proxy-init` to obtain the permissions to run Gauss. 

Hint: This will take long time ~20 minutes because we asked for a full detector simulation by Geant4 but we can only tell Gauss to run the generator phase of simulation only by adding `'$GAUSSOPTS/GenStandAlone.py'` as an option. 

Now lets give some information about the options which we told gauss to use.  The following table summarize that: 


| Option                                                   | Description                                                  |
|----------------------------------------------------------|--------------------------------------------------------------|
| `'$APPCONFIGOPTS/Gauss/Beam6500GeV-md100-2016-nu1.6.py’` | Sets beam energy and position                                |
| `'$APPCONFIGOPTS/Gauss/EnableSpillover-25ns.py'`           | Enables spillover (only Run2, not sure about RUN3 yet)<br>   |
| `'$APPCONFIGOPTS/Gauss/DataType-2016.py'`                  | Tells Gauss which data category to expect (divided by year)  |
| `'$APPCONFIGOPTS/Gauss/RICHRandomHits.py' `                | Random hits in RICH for occupancy                            |
| `'$DECFILESROOT/options/{eventnumber}.py'`                 | Event type containing the signal                             |
|  `'$LBPYTHIA8ROOT/options/Pythia8.py'`                     | This tells Gauss to use pythia as signal generator           |
| `'$APPCONFIGOPTS/Gauss/G4PL_FTFP_BERT_EmNoCuts.py'`        | This tells Gauss to use Geant4 as detetor simulation tool with config |
| `'$GAUSSOPTS/GenStandAlone.py'`                            | Run only signal generation part and ignore full detetor simulation |


## Nightly version 

This is how we can run the nightly version of Gauss

The general syntax is 

```
lb-run --nighlty lhcb-gauss-dev/SLOTID Gauss/HEAD gaudirun.py Gauss-Job.py
```

Where `SLOTID` would be `Today` or the one that we are interested

The default is `Today` and could also be used as follows 

```
lb-run --nightly lhcb-gauss-dev Gauss/HEAD  gaudirun.py Gauss-Job.py
```


## Resources

- [ Simulation in LHCb Twiki ](https://twiki.cern.ch/twiki/bin/view/LHCb/LHCbSimulation)
- [ LHCb Software Training Twiki](https://twiki.cern.ch/twiki/bin/view/LHCb/LHCbSoftwareTutorials)
- [Gauss in LHCb Starterkit](https://lhcb.github.io/starterkit-lessons/second-analysis-steps/simulation.html)
