# Storm Report: Hurricane Elsa `AL052021`
This folder contains two python files (setrun.py, setplot.py) and one Makefile to simulate storm bevavior of hurricane Elsa in July 2021.

## Hurricane Elsa Information:
Elsa was an early season category 1 hurricane (on the Saffir-Simpson Hurricane Wind Scale) that formed over the central tropical Atlantic. Elsa affected many countries including Barbados, St. Lucia, St. Vincent and the Grenadines, Martinique, the Dominican Republic, Haiti, Cuba, and the United States. The hurricane caused around $1 billion in damage along its track and was responsible for 13 direct fatalities. 
Elsa affected the Florida Keys and paralleled the west coast of Florida before making landfall in the Big Bend region on 6 and 7 July. In the Florida Keys, the Smith Shoal Light marine station reported maximum winds of 53 kt and a wind gust to 58 kt at 1408 UTC 6 July. On land, Key West International airport (KEYW) recorded sustained winds of 45 kt and gust of 61 kt at 1550 UTC 6 July. Near the point of landfall in north Florida, Horseshoe Beach reported sustained winds of 54 kt and a wind gust to 62 kt at 1440 UTC 7 July, which was the highest wind recorded during Elsa in Florida. Cedar Key observed sustained winds of 40 kt and wind gusts to 50 kt at 1042 UTC 7 July.
After the Florida landfall, Elsa turned toward the northeast and accelerated ahead of a frontal boundary, producing tropical-storm-force winds over a large portion of the U.S. eastern seaboard. In Georgia, a station in Tybee Island recorded sustained winds of 55 kt and a wind gust to 65 kt at 0303 UTC 8 July, but this was likely associated with a strong squall to the east of the center and not representative of Elsa’s true intensity at the time. Nearby in South Carolina, a Weatherflow station in Calibogue Sound recorded sustained winds of 47 kt and a gust to 70 kt at 0328 UTC 8 July in a similar squall. The elevated Frying Pan Tower located southeast of Cape Fear, North Carolina recorded sustained winds of 43 kt and a gust to 50 kt at 1400 UTC 8 July. In North Carolina, the highest winds were recorded in the Outer Banks by a Weatherflow station at Kites Resort that reported sustained winds of 46 kt and a wind gust to 55 kt at 0155 UTC 9 July.
Most of the wind impacts associated with Elsa in the Mid-Atlantic and northeastern U.S. occurred over the eastern portion of the circulation as the system interacted with an approaching frontal boundary. An elevated site in Tangier Island, Virginia, reported sustained winds of 50 kt and a wind gust to 59 kt at 0153 UTC on 9 July, which was the highest report in the state during Elsa. Similarly, in Maryland, a Weatherflow site at Assateague Island recorded sustained winds of 46 kt and a wind gust of 59 kt at 0353 UTC on 9 July. Farther up the coast, three coastal stations in New Jersey reported wind gusts over 60 kt during the morning hours of 9 July. The highest wind recorded in New York occurred on the northern coast of Long Island, where a Weatherflow site at Eatons Neck reported sustained winds of 48 kt and gust to 52 kt at 1219 UTC on 9 July. Mount Washington recorded a peak sustained wind of 60 kt and a gust of 74 kt at 0554 UTC on 9 July. In coastal Massachusetts and Maine, wind gusts reached over 50 kt on 9 July.


If running this example, download setrun.py, setplot.py, and Makefile to the appropriate directory. Execute `$ make all` or `$ make .plots` to compile the code, run the simulation, and plot the results. If problems will be encountered, please refer to <a href="http://www.clawpack.org/quick_surge.html?highlight=storm%20surge" target="_blank">Storm Surge Guide</a> for possible solutions. 

*Reference: National Hurricane Center Tropical Cyclone Report*
(https://www.nhc.noaa.gov/data/tcr/AL052021_Elsa.pdf)

## Topography & Bathymetry Data:
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
### Station 1: 
Freeport Harbor, TX `ID: 8772471` experienced a storm surge of approximately 1 meter. GeoClaw predicted approximately 0.3 meters. 

Significant discrepancies in results may stem from the gauges being located in “dry cells” in the simulation. Harvey’s historic rains and the resulting flooding are other significant contributors to the measured storm surge; these factors are not accounted for in the GeoClaw simulation.

## Conclusion:
Storm surges obtained from GeoClaw were generally inconsistent with the observed data. In most cases, the observed storm surge greatly exceeded the amount predicted by the GeoClaw model. The reason for this likely comes from Harvey’s historic rains, which caused significant flooding but are not accounted for in the model. Adjustments to the GeoClaw package to incorporate rainfall may lead to more accurate results.


Author: Jinpai (Max) Zhao
```
Contact: jz3445@columbia.edu
```