## General Information
To submit a QEE simulation request use the google form [here](https://docs.google.com/forms/d/e/1FAIpQLSfw9CPxzb2jllxsDA9O1SxuGset6ipe908l4vG4q5poT2GfRA/viewform).  If that link doesn't work, check the QEE twiki for the form [here](https://twiki.cern.ch/twiki/bin/view/LHCbPhysics/QcdEwExotica).  The current QEE simulation liasons can be found [here](https://twiki.cern.ch/twiki/bin/viewauth/LHCbPhysics/LHCbWGLiaisons) in the QCD, Electroweak, and Exotica row.

## The Google Form
The google form is fairly self explanatory, however there are a few fields that can be tricky the first time filling out the form.  The 'Brief Motivation' field should be one or two sentences about what analysis the simulation request is for.  To determine whcih version of Gauss is needed for your simulation I would consult the following chart in the starterkit [here](https://lhcb.github.io/starterkit-lessons/second-analysis-steps/simulation-intro.html#choosing-your-gauss-version).  I have only needed to use Sim09 so far, and I believe all run 3 simulations will use Sim10.  For the 'Decay files' field be sure to look up the simulation process's event type number [here](http://lhcbdoc.web.cern.ch/lhcbdoc/decfiles/) and list its associated decay file.  In the 'Any other information' section it is typically recommended to request the generator statistics table as it contians the cross section PYTHIA found for the simulated process.  Below is an example form filled out.

Email Addresses: me@cern.ch, advisor@cern.ch    
Brief Motivation: The University of Cincinnati group is trying to measure the WW->lvlv cross section, and needs MC for Run 2.  
For which year(s) do you need your samples to be made, and how many events in total:  Years: 2016, 2017, 2018 Events: 12M Total, 4M Each.  
Which data format: Full DST  
Is your request exceeding the limit and requires PPG approval: *Leave Blank*  
For run-2 requests, would you like to use Sim09 or Sim10?  Sim09  
Event type numbers and events to be produced per magnet polarity: 41922002: 2000000  
Decay files: WW_lnul,lnul=1l15GeV.dec  
Any other information: Generator Statistics Table Requested  

## After Submitting the Form
After submitting the form the QEE MC liaisons will contact you and may clarify some information in the form.  Once the request has been confirmed with the liaisons, and with the QEE conveners who the MC liaisons will cc in their email, the simulation request will be submitted.  The request will go through several status changes ending with 'Done'.  Once completed the MC liaisons will need to generate the statistics table which will take a few more days, however the data will be available in the bookkeeping once the status has been set to done.
