# gtfs2gmns
 light-weight conversion from GTFS data format to GMNS data format 

The open-source Python code (GTFS2GMNS) is released to facilitate researchers and planners in constructing multi-modal transit networks easily from the generic General Transit Feed Specification (GTFS) to the network modeling format in General Modeling Network Specification (GMNS). The converted physical and service networks in GMNS format are more convenient for network modeling tasks such as transit network routing, traffic flow assignment, simulation and service network optimization.

Your comments will be valuable for code review and improvement. Please feel free to add your comments to our Google document of GTFS2GMNS Users' Guide.

<img width="1284" height="490" alt="image" src="https://github.com/user-attachments/assets/9a891d25-6862-4157-9945-68b44093b19d" />

Getting Started
## 1. Download GTFS Data
On the TransitFeed homepage, users can browse and download official GTFS feeds from around the world. Make sure that the following files are present, so that we can proceed.

stop.txt
route.txt
trip.txt
stop_times.txt
agency.txt
GTFS2GMNS can handle the transit data from several agencies. Users need to configure different sub-files in the same directory. Under the test folder, three subfolders Cottonwood_Area_Transit, Flagstaff_MountainLine, and Sedona_RoadRunner with their owm GTFS data is set up.

Convert GTFS Data into GMNS Format
## 2. Read GTFS data
Step 2.1: Read routes.txt

route_id, route_long_name, route_short_name, route_url, route_type
Step 2.2: Read stop.txt

stop_id, stop_lat, stop_lon, direction, location_type, position, stop_code, stop_name, zone_id
Step 2.3: Read trips.txt

trip_id, route_id, service_id, block_id, direction_id, shape_id, trip_type
and create the directed_route_id by combining route_id and direction_id
Step 2.4: Read stop_times.txt

trip_id, stop_id, arrival_time, deaprture_time, stop_sequence

create directed_route_stop_id by combining directed_route_id and stop_id through the trip_id

Note: the function needs to skip this record if trip_id is not defined, and link the virtual stop id with corresponding physical stop id.

fetch the geometry of the direction_route_stop_id

return the arrival_time for every stop

## 3. Building service network
### Step 3.1 Create physical nodes

The physical node is the original stop in the standard GTFS
### Step 3.2 Create directed route stop vertices

Add route stop vertices. The node_id of route stop nodes starts from 100001

Note: the route stop vertex is created near the corresponding physical node, to make some offset.

Add an entrance link from the physical node to the route stop node

Add an exit link from the route stop node to the physical node. As they both connect to the physical nodes, the in-station transfer process can be also implemented

### Step 3.3 Create boarding and decoarding arcs

Add links between each physical node and the corresponding route stop vertices.
Step 3.4 Create transferring arcs

Add transferring links between physical nodes of different modes.
Step 3.5 Create service arcs

Add service links between each route stop pair of each trip
## 4. Output
The output files include node.csv and link.csv.

## 5. Visualization
You can visualize generated networks using NeXTA or QGIS.
