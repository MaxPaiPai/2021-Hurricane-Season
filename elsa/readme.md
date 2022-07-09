# Storm Report: Hurricane Elsa `AL052021`
This folder contains two python files (setrun.py, setplot.py) and one makefile to simulate storm bevavior of hurricane Elsa in July 2021.

## Hurricane Elsa Information:
Hurricane Harvey was a category 4 hurricane that was the eighth named and first major hurricane of the 2017 Atlantic Hurricane season. Harvey began as a weak tropical storm which affected the Lesser Antilles before dissipating over the central Caribbean Sea. It reformed over the Bay of Campeche, quickly intensifying to a category 4 hurricane before making landfall over San Jose island, near Rockport, Texas, at 0300 UTC, August 26 2017. It is estimated that at this landfall, sustained winds reached 115 kt and minimum central pressure was 937 mb. Harvey made a second landfall 3 hours later, this time over the Texas mainland. Hurricane Harvey then stalled over the Texas coast for four days, dropping over 60 inches of rain on southeastern Texas, causing historic flooding. Harvey made a final landfall over southwestern Louisiana at 0800 UTC 30 August. Sustained winds during this landfall were 40 kt.

If running this example, download setrun.py, setplot.py, and Makefile to the appropriate directory. Execute `$ make all` or `$ make .plots` to compile the code, run the simulation, and plot the results. If problems will be encountered, please refer to <a href="http://www.clawpack.org/quick_surge.html?highlight=storm%20surge" target="_blank">Storm Surge Guide</a> for possible solutions. 

*Source: National Hurricane Center Tropical Cyclone Report*
(https://www.nhc.noaa.gov/data/tcr/AL092017_Harvey.pdf)

## Topography/Bathymetry Data:
Topography data is automatically downloaded from the Columbia databases at:
http://www.columbia.edu/~ktm2132/bathy/gulf_caribbean.tt3.tar.bz2

## Storm Data:
Data to run the simulation was retrieved from NOAA’s storm data archive:
http://ftp.nhc.noaa.gov/atcf/archive/2017/bal092017.dat.gz

In setrun.py, data can be directly retrieved from the source by writing code similar to this:
```python
# Convert ATCF data to GeoClaw format
clawutil.data.get_remote_file(“http://ftp.nhc.noaa.gov/atcf/archive/2017/bal092017.dat.gz”)
atcf_path = os.path.join(data_dir, “bal092017.dat”)
```

For this example, Hurricane Harvey storm data should be placed in the same directory that the simulation is run in.

## GeoClaw Parameters:
### Landfall
Time of landfall was set in the simulation to be 26 August, 0400 UTC. Simulation ran from 4 days before landfall to 5 days after.
### Guages:
Gauges were selected in the NOAA Inundations dashboard:
https://tidesandcurrents.noaa.gov/map/index.html
### Observed Surge Data:
The observed gauge data for sea level at each location was de-tided using the `fetch_noaa_tide_data()` method and plotted against the predicted storm surge by GeoClaw.
### AMRClaw:


## Validation Result:
`Station 1`Freeport Harbor, TX `ID: 8772471` experienced a storm surge of approximately 1 meter. GeoClaw predicted approximately 0.3 meters. 

Significant discrepancies in results may stem from the gauges being located in “dry cells” in the simulation. Harvey’s historic rains and the resulting flooding are other significant contributors to the measured storm surge; these factors are not accounted for in the GeoClaw simulation.

## Conclusion:
Storm surges obtained from GeoClaw were generally inconsistent with the observed data. In most cases, the observed storm surge greatly exceeded the amount predicted by the GeoClaw model. The reason for this likely comes from Harvey’s historic rains, which caused significant flooding but are not accounted for in the model. Adjustments to the GeoClaw package to incorporate rainfall may lead to more accurate results.

```
Project was studied by Jinpai (Max) Zhao. Contact: jz3445@columbia.edu
```