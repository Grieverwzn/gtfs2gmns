# gtfs2gmns
 light-weight conversion from GTFS data format to GMNS data format 

The open-source Python code (GTFS2GMNS) is released to facilitate researchers and planners in constructing multi-modal transit networks easily from the generic General Transit Feed Specification (GTFS) to the network modeling format in General Modeling Network Specification (GMNS). The converted physical and service networks in GMNS format are more convenient for network modeling tasks such as transit network routing, traffic flow assignment, simulation and service network optimization.

Your comments will be valuable for code review and improvement. Please feel free to add your comments to our Google document of GTFS2GMNS Users' Guide.

# Convert GTFS data format to line plan or service network described using GMNS data format 
<img width="1269" height="504" alt="image" src="https://github.com/user-attachments/assets/d6dc9521-b499-4ec0-81d2-d1722c14a275" />


# Getting Started
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
### Step 2.1: Read routes.txt

route_id, route_long_name, route_short_name, route_url, route_type

### Step 2.2: Read stop.txt

stop_id, stop_lat, stop_lon, direction, location_type, position, stop_code, stop_name, zone_id

### Step 2.3: Read trips.txt

trip_id, route_id, service_id, block_id, direction_id, shape_id, trip_type
and create the directed_route_id by combining route_id and direction_id
### Step 2.4: Read stop_times.txt

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
### Step 3.4 Create transferring arcs

Add transferring links between physical nodes of different modes.
### Step 3.5 Create service arcs

Add service links between each route stop pair of each trip
## 4. Output
The output files include node.csv and link.csv.

## 5. Visualization
You can visualize generated networks using NeXTA or QGIS.

# Examples 1 (Transit Assignment in the DC-Maryland-Virginia (DMV) Metropolitan area): 
### Transit service network in DC-Maryland-Virginia Metropolitan area (illustrated using QGIS)
<img width="844" height="493" alt="image" src="https://github.com/user-attachments/assets/21bb8e04-29cc-4773-8613-ed936d2abf61" />

### Transit assignment on metro lines (analysis period AM from 6：00 AM to 9:00 AM)
<img width="897" height="550" alt="image" src="https://github.com/user-attachments/assets/2279778c-4f26-492a-a919-c2419c593ed5" />

### Transit assignment on bus lines (analysis period AM from 6：00 AM to 9:00 AM)
<img width="796" height="544" alt="image" src="https://github.com/user-attachments/assets/9b052070-e8a4-40fc-a3b6-2f679876ec44" />

### Transit assignment on AMTRAK rail lines (analysis period AM from 6：00 AM to 9:00 AM)
<img width="760" height="522" alt="image" src="https://github.com/user-attachments/assets/db113ace-4301-47f8-97af-ab1b53fdf138" />
### Boarding in the illustrative region
<img width="526" height="517" alt="image" src="https://github.com/user-attachments/assets/4786600a-db29-4af0-843c-7e8620f84a2b" />


Note: The figures are generated during the project The Northern Virginia Transportation Authority (NVTA) TransAction when the author worked in Arizona State University (ASU)

# Examples 2 (Transit accessibiltiy analysis in the Philadaphia Metropolitan area): 
### Transit service network in the Philadephia Metropolitan area, SEPTA network (illustrated using QGIS)
<img width="977" height="686" alt="image" src="https://github.com/user-attachments/assets/fd02a472-1fe0-44cc-b2b9-ca8de4bd96a9" />

### Zone based transit accessibility calculated through the transit service network
<img width="886" height="587" alt="image" src="https://github.com/user-attachments/assets/9701440b-31bb-4132-ba3e-f3e60ea8accb" />

### The improvement of transit accessibility after subsidy allocations to improve the fairness of the transit accessibility 
<img width="780" height="573" alt="image" src="https://github.com/user-attachments/assets/ef5de1e8-978d-4431-9ba9-68da563fb3e1" />

Note: The figures are generated during the project of Enhanced Mobility Innovation （EMI） with Software-Based Solutions for Smart and Equitable Travel Demand Managementwhen the author worked in Villanova University (ASU)
