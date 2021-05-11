# EVPathPlanning
Controllable path planning and traffic scheduling for emergency services in the Internet of Vehicles

** Dependencies
Rebuild src SUMO 1.2

** Guidance

Step 1: Extract the road network (lust.rou.xml) into the file roadnetwork.txt in inputs folder
 - <route edges="-32426#0 -32426#0-AddedOffRampEdge -31128 -31604 -30590#5 -30590#6 -30590#7 -30590#8 -32952#0 -32952#1 --31462#3 --31462#2 --31868#2 --31868#1 --31868#0 -31078#1 -31260 -30324 -31110 -30612 -30994#1 -31502 -31712#1 -31794#0 -31794#1 -31794#2 -31794#3 -31794#4 -31794#5 -31794#6 -31794#7 --30528#42 --30528#41 --30528#40 --30528#39"/>
 - Create node: node -32426#0
 - Create edge and cost: arc -32426#0 -32426#0-AddedOffRampEdge
 
 Can use netconvert to generate road edge lists (cost calculation = edge_length/(allowed_speed*number_of_lanes).

Step 2: Run FindingNPaths.jpynb to get the suggested paths

 For example:
      -30852|-30462|--30768#1|--30768#0|-31230#2|-32628|--31272#2|--31272#1|--31272#0|-30528#21|-30528#22|-30528#23|-31846#0|-31846#1 |-31904#0|-31904#1|-31904#2|-30694#3|-32696#0|-32696#1|-31196#0|-31196#1|-30332#0


Step 3: For each suggested path, create a vehicle entry in lust.rou.xml at the time point 21900 (6:05 AM) and 29100 (8:05 AM)...
       
		<vehicle id="ccu202011281" type="police" depart="21900.00" departPos="87.40" departSpeed="20.00" arrivalPos="40.86">
           <route edges="-30852 -30462 --30768#1 --30768#0 -31230#2 -32628 --31272#2 --31272#1 --31272#0 -30528#21 -30528#22 -30528#23 -31846#0 -31846#1 -31904#0 -31904#1 -31904#2 -30694#3 -32696#0 -32696#1 -31196#0 -31196#1 -30332#0"/>
        </vehicle>  
		
Step 3: Set up the TLS program : tlsProgram.xml

Step 4: Run the screnario and record the EV trip information
     
	- sumo -c scenario/ccuits_full.sumo.cfg --addition-file scenario/tlsProgram.xml --fcd-output ccuits.xml
	- The details of the EVs speed can be found in ccuits.xml by the vehicle ID which has set in Step 3 
	- The delay time of the route can be found in ccuits_full.tripinfo.xml (generated in screnario folder)

	
   	 