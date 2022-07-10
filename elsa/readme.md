# Storm Report: Hurricane Elsa `AL052021`
This folder contains two python files (setrun.py, setplot.py) and one Makefile to simulate storm bevavior of hurricane Elsa in July 2021.

## Table of contents
- [Hurricane Elsa Information](#hurricane-elsa-information)
  * [Path & Landfall](#path--landfall)
  * [Storm surge](#storm-surge)
  * [Rainfall & Flooding](#rainfall--flooding)
- [Topography & Bathymetry Data](#topography--bathymetry-data)
- [Storm Data](#storm-data)
- [GeoClaw Parameters](#geoclaw-parameters)
  * [Landfall & Time Range](#landfall--time-range)
  * [Guages](#guages)
  * [AMRClaw](#amrclaw)
- [Observed Surge Data](#observed-surge-data)
- [Validation Result](#validation-result)
  * [Station 1-Cedar Key, FL](#station-1-cedar-key--fl)
  * [Station 2-Clearwater Beach, FL](#station-2)
  * [Station 3-Old Port Tampa, FL](#station-3)
  * [Station 4-Port Manatee, FL](#station-4)
  * [Station 5-Naples, FL](#station-5)
  * [Station 6-Key West, FL](#station-6)
- [Conclusion](#conclusion)

## Hurricane Elsa Information
### Path & Landfall
Elsa was a category 1 hurricane formed over the central tropical Atlantic. Elsa affected many countries including Barbados, St. Lucia, St. Vincent and the Grenadines, Martinique, the Dominican Republic, Haiti, Cuba, and the United States. It caused around $1 billion in total damage and was responsible for 13 direct fatalities. Elsa affected the Florida Keys and the west coast of Florida along its path before making landfall in the Big Bend region on 6th and 7th July. After the Florida landfall, Elsa turned toward the northeast and accelerated towards the U.S. eastern seaboard. 
### Storm surge
Elsa produced storm surge inundation levels of 1 ft to 3 ft above normally dry ground (AGL) along portions of the west coast of Florida, with the highest water levels occurring along the coasts of Hernando, Citrus, Levy, Dixie, and Taylor Counties at around 2 ft to 3 ft. 
### Rainfall & Flooding
Elsa produced a series of heavy rainfalls along its path involing the eastern US. In west Florida, a large region encountered a 3–7 inches of rain fell. Several monitoring sites across southwest Florida recorded rainfall amount of 8–11 inches. Rainfall amounts of 3–7 inches were observed in the Lower Florida Keys, with the highest accumulation occurring in Key West. Significant flooding occurred in multiple areas of Key West as a heavy rainband set up over the island.

If running this example, download setrun.py, setplot.py, and Makefile to the appropriate directory. Execute `$ make all` or `$ make .plots` to compile the code, run the simulation, and plot the results. If problems will be encountered, please refer to <a href="http://www.clawpack.org/quick_surge.html?highlight=storm%20surge" target="_blank">Storm Surge Guide</a> for possible solutions. 

*Reference: National Hurricane Center Tropical Cyclone Report*
(https://www.nhc.noaa.gov/data/tcr/AL052021_Elsa.pdf)

## Topography & Bathymetry Data
Topography data was obtained from:
https://www.gebco.net/data_and_products/gridded_bathymetry_data/
Regions of topography data used was a rectangle region (West 90°, East 65°, North 45°, South 15°)

Currently, topography data is stored in the author's google drive. It can be accessed with a columbia email and can be fetched in setrun.py by modifying codes in topography section similar to this:
```python
# Fetch topo data
clawutil.data.get_remote_file(
           "https://drive.google.com/uc?export=download&id=1e8M_4m_y6yFJk9nOhwkPv9IJj8FotmQK")
```

## Storm Data
Storm specific data for Hurricane Elsa was retrieved from NOAA’s storm data archive:
http://ftp.nhc.noaa.gov/atcf/archive/2021/bal052021.dat.gz

In setrun.py, data can be directly fetched by modifying codes in storm data section similar to this:
```python
# Convert ATCF data to GeoClaw format
clawutil.data.get_remote_file(“http://ftp.nhc.noaa.gov/atcf/archive/2021/bal052021.dat.gz”)
atcf_path = os.path.join(data_dir, “bal052021.dat”)
```

## GeoClaw Parameters
### Landfall & Time Range
Time of landfall was set in the simulation to be 7 August, 1400 UTC. Time range of simulation was set to be 2 days (48 hours) before landfall and 1 day (24 hours) after landfall.
### Guages
Gauges were selected in the NOAA Inundations dashboard:
https://tidesandcurrents.noaa.gov/map/index.html
### AMRClaw

## Observed Surge Data
To compare simulation surge data by GeoClaw, we introduced the observed surge data using `clawpack.geoclaw.util.fetch_noaa_tide_data` along with each guage's station ID. When plotting the observed surge data, we explicitly deduct the tide amount from sea level at each location to make the data solely representing storm surge.

## Validation Result
### Station 1-Cedar Key, FL
Cedar Key, FL `ID: 8727520` experienced a storm surge of approximately 1 meter. GeoClaw predicted approximately 0.3 meters. 

![Station1_Cedar Key](./images/station1_cedarkey.png)
### Station 2-Clearwater Beach, FL
Clearwater Beach, FL `ID: 8726724` experienced a storm surge of approximately 1 meter. GeoClaw predicted approximately 0.3 meters. 

![Station2_Clear Water Beach](./images/station2_clearwaterbeach.png)
### Station 3-Old Port Tampa, FL
Old Port Tampa, FL `ID: 8726607` experienced a storm surge of approximately 1 meter. GeoClaw predicted approximately 0.3 meters. 

![Station3_Old Port Tampa](./images/station3_oldporttampa.png)
### Station 4-Port Manatee, FL
Port Manatee, FL `ID: 8726384` experienced a storm surge of approximately 1 meter. GeoClaw predicted approximately 0.3 meters. 

![Station4_Port Manatee](./images/station4_portmanatee.png)
### Station 5-Naples, FL
Naples, FL `ID: 8725110` experienced a storm surge of approximately 1 meter. GeoClaw predicted approximately 0.3 meters. 

![Station5_Naples](./images/station5_naples.png)
### Station 6-Key West, FL
Key West, FL `ID: 8724580` experienced a storm surge of approximately 1 meter. GeoClaw predicted approximately 0.3 meters. 

![Station6_Key West](./images/station6_keywest.png)


Significant discrepancies in results may stem from the gauges being located in “dry cells” in the simulation. Harvey’s historic rains and the resulting flooding are other significant contributors to the measured storm surge; these factors are not accounted for in the GeoClaw simulation.

## Conclusion
Storm surges obtained from GeoClaw were generally inconsistent with the observed data. In most cases, the observed storm surge greatly exceeded the amount predicted by the GeoClaw model. The reason for this likely comes from Harvey’s historic rains, which caused significant flooding but are not accounted for in the model. Adjustments to the GeoClaw package to incorporate rainfall may lead to more accurate results.


Author: Jinpai (Max) Zhao
```
jz3445@columbia.edu
```
