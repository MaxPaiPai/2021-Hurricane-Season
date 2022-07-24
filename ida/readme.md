# Storm Report: Hurricane Ida `AL092021`
This folder contains two python files (setrun.py, setplot.py) and one Makefile to simulate storm bevavior of hurricane Ida in August 2021.

## Table of contents
- [Hurricane Ida Information](#hurricane-ida-information)
  * [Path & Landfall](#path--landfall)
  * [Storm surge](#storm-surge)
  * [Rainfall & Flooding](#rainfall--flooding)
- [General Code Execution Guide](#general-code-execution-guide)
- [Topography & Bathymetry Data](#topography--bathymetry-data)
- [Storm Data](#storm-data)
- [GeoClaw Parameters](#geoclaw-parameters)
  * [Landfall & Time Range](#landfall--time-range)
  * [Guages](#guages)
  * [AMRClaw](#amrclaw)
- [Observed Surge Data](#observed-surge-data)
- [Validation Result](#validation-result)
  * [Station 1-Pilots Station East, LA](#station-1-pilots-station-east-la)
  * [Station 2-Grand Isle, LA](#station-2-grand-isle-la)
  * [Station 3-Eugene Island, LA](#station-3-eugene-island-la)
  * [Station 4-Bay Waveland Yacht Club, LA](#station-4-bay-waveland-yacht-club-la)
  * [Station 5-Port Fourchon, LA](#station-5-port-fourchon-la)
  * [Result Interpretation](#result-interpretation)
- [Conclusion](#conclusion)

## Hurricane Ida Information

### Path & Landfall
Ida was a category 4 hurricane. It caused severe damage when it made landfall in southeastern Louisiana before its landfall in western Cuba as a category 1 hurricane. Ida later became an extratropical low that caused heavy rain and deadly flooding in the northeastern United States.

### Storm surge
Ida produced a very severe storm surge that penetrated inland from the immediate coastline across portions of southeastern Louisiana, including on both the east and west banks of the Mississippi River and bordering portions of Lake Pontchartrain. Storm surge levels were enough high in some areas to overtop local levee systems. 

### Rainfall & Flooding
Generally speaking, Ida produced widespread rainfall along its path. Ida produced widespread heavy rains along portions of the northern Gulf coast states. Rainfall totals of more than 10 inches occurred over portions of southeastern Louisiana, southeastern Mississippi, and southwestern Alabama. Rainfall totals of 5 to 9 inches over eastern Mississippi, southwestern Alabama, and the western Florida Panhandle. The heavy rainfall produced freshwater flooding, especially in southeastern Louisiana and in southeastern Mississippi.


*Reference: National Hurricane Center Tropical Cyclone Report*
(https://www.nhc.noaa.gov/data/tcr/AL092021_Ida.pdf)

## General Code Execution Guide
If running this example, download setrun.py, setplot.py, and Makefile to the appropriate directory. Execute `$ make all` or `$ make .plots` to compile the code, run the simulation, and plot the results. If problems will be encountered, please refer to <a href="http://www.clawpack.org/quick_surge.html" target="_blank">Storm Surge Guide</a> for possible solutions. 

## Topography & Bathymetry Data
Topography data can be obtained from:
https://www.gebco.net/data_and_products/gridded_bathymetry_data/

Topography file used for this example is stored in professor Kyle Mandli's website and can be fetched in setrun.py by modifying codes in topography section similar to this:
```python
# Fetch topo data
clawutil.data.get_remote_file(
           "http://www.columbia.edu/~ktm2132/bathy/gulf_caribbean.tt3.tar.bz2")
```

## Storm Data
Storm specific data for Hurricane Ida was retrieved from NOAA’s storm data archive:
http://ftp.nhc.noaa.gov/atcf/archive/2021/bal092021.dat.gz

In setrun.py, data can be directly fetched by modifying codes in storm data section similar to this:
```python
# Convert ATCF data to GeoClaw format
clawutil.data.get_remote_file(“http://ftp.nhc.noaa.gov/atcf/archive/2021/bal092021.dat.gz”)
atcf_path = os.path.join(data_dir, “bal092021.dat”)
```

## GeoClaw Parameters
### Landfall & Time Range
Time of landfall was set in the simulation to be 7 August, 1400 UTC. Time range of simulation was set to be 2 days (48 hours) before landfall and 1 day (24 hours) after landfall.
### Guages
Gauges were selected in the NOAA Inundations dashboard:
https://tidesandcurrents.noaa.gov/map/index.html
### AMRClaw
AMRClaw is a very powerful algorithm to refine areas for analysis. After merging the algorithm in setrun.py, we will be able to have a high resolution of regions which will effectively solve bad simulation curves by GeoClaw due to wrongly appeared dry cells from low resolution. One can include or exclude AMRClaw algorithm by modifying codes in setrun.py similar to
```python
from clawpack.clawutil import clawdata
rundata = clawdata.ClawRunData(claw_pkg, num_dim)
```
One may also want to modify AMR parameters like `amr_levels_max` and `refinement_ratios` in a more customized way. In this example, `amr_levels_max` was set to be `7` and `refinement_ratios` was set to be `[2, 2, 2, 3, 4, 4, 4]`. More information regarding parameter can be found in the documentation here: <a href="https://www.clawpack.org/setrun_amrclaw.html#setrun-amrclaw" target="_blank">AMRClaw Information</a>.

## Observed Surge Data
To compare simulation surge data by GeoClaw, we introduced the observed surge data using `clawpack.geoclaw.util.fetch_noaa_tide_data` along with each guage's station ID. When plotting the observed surge data, we explicitly deduct the tide amount from sea level at each location to make the data solely representing storm surge.

## Validation Result
### Station 1-Pilots Station East, LA
Cedar Key, FL `ID: 8760922` experienced a storm surge of approximately 0.75 meter. GeoClaw predicted approximately 0.80 meters. 

![Station1](./images/station1.png)

### Station 2-Grand Isle, LA
Clearwater Beach, FL `ID: 8761724` experienced a storm surge of approximately 0.50 meter. GeoClaw predicted approximately 0.25 meters. 

![Station2](./images/station2.png)

### Station 3-Eugene Island, LA
Old Port Tampa, FL `ID: 8764314` experienced a storm surge of approximately 0.65 meter. GeoClaw predicted approximately 0.55 meters. 

![Station3](./images/station3.png)

### Station 4-Bay Waveland Yacht Club, LA
Port Manatee, FL `ID: 8747437` experienced a storm surge of approximately 0.50 meter. GeoClaw predicted approximately 0.40 meters. 

![Station4](./images/station4.png)

### Station 5-Port Fourchon, LA
Naples, FL `ID: 8762075` experienced a storm surge of approximately 0.60 meter. GeoClaw predicted approximately 0.20 meters. 

![Station5](./images/station5.png)

### Result Interpretation
Differences in surface level are reasonable and acceptable with maximum error among all guages less than 0.5 meters. Individual difference are correlated to rainfall and flooding amount which was not included in the GeoClaw simulation due to their complexity and unpredictability. However, notice there's also a discrepancy of timing between major surge at Key West station. The reason is Key West experienced the most intense rainfall and flooding which explains the surge on observed data. Note that there's little or no precipitation at hurricane eye which explains the surface drop for observed data but a surge on simulation data.

## Conclusion
Timing and pattern of storm surges obtained from GeoClaw were generally consistent with the observed data. In most cases, the observed storm surge slightly exceeded the amount of which from GeoClaw simulation. The reason may likely correspond to the rainfall and flooding caused by hurricane Elsa which was not taken into account by GeoClaw simulation. Future studies can investigate the relationship between timing of surges on real data and precipitation, so that a more detailed analysis can be conducted. 


Author: Jinpai (Max) Zhao
```
jz3445@columbia.edu
```
