<h1> Rover Swarm Project - Rover 02 </h1>

<p> This project is a simulation of a set of autonomous rovers that are exploring, mapping, and harvesting science on an alien planet. The rovers have very limited capabilities individually and therefore have to operate as a swarm to fulfill their objectives. </p>

![image of mars rovers](http://i.imgur.com/8n6arMu.jpg)



<h1 align=center > What's in this Documentation: </h1>

<h3>1.What are the movement commands? What are the scan commands?</h3>
  <p>- Brief information about the rover movement and some of the useful commands that you might need to understand the rover. </p>

<h3>2.What are the harvesting commands?</h3>
  <p>- Moving around is good but the purpose of these rovers is to harvest as much science as they can. In this section, you will find
    more about how rovers harvesting these sciences. </p>

<h3>3.What are the communication commands?</h3>
<p> - Each rover can't stand alone. To get the best out of each rover in less time, you need all the rovers to communicate together.
  In this section, you will find more information about how they communicate together through some methods calls. </p>

<h3>4.How are the pathfinding classes used?</h3>
 <p> - In this section, you will go deeper to understand how to rover is deciding the shortest path to a specific science using some algorithm. </p>

<h3>5.What are some design approaches to be considered for mapping behavior and harvesting behavior and when/how to switch from one to the other?
Also, what are some approaches to not getting the rovers stuck in a corner?</h3>
<p>- In this section, we explain more about rovers step-by-step decisions based on exploring or harvesting mode. </p>

<h3>6.What equipment is available to a rover and how is it configured?</h3>
 <p> - Each rover has its own equipment. We explain how to configure each rover with its own tools.</p>

<h3>7.Describe the different drive and tool types, how they are used and how they interact with the environment. Go into some of the design considerations for choosing different equipment configurations for small (5) medium (6-10) and large (10+) groups of rovers. How should tools and drive types be mixed?</h3>
<p>- This section talks more about the different type of drive and tools. How rovers have been equiped and how to make the best out of these equipment.</p>

<h3>8.Make some recommendations on how to improve the implementation of the project. Make some recommendations on additional features and functions to add to the simulation such as, liquid terrain features, hex vs. square map tiles, power limitations (solar, battery, etc.), towing, chance of break downs, etc</h3>
 <p> - Our ideas of how to impove this project. </p>

<h1 align=center> Group-2 </h1>

**1.What are the movement commands? What are the scan commands?**

The basic movement commands are the `moveNorth()`,`moveEast()`,`moveSouth()` and `moveWest()`. These methods are called from `Rover.java` which prints a statement with help of a [PrintWriter](https://docs.oracle.com/javase/7/docs/api/java/io/PrintWriter.html) object.

For instance, this is how the `moveNorth()` method is implemented:

```
protected void moveNorth() {
    sendTo_RCP.println("MOVE N");
   }
```

The `sendTo_RCP` is an instance of the `PrintWriter` class that sends the corresponding movement message to the `RoverCommandProcessor`. Based on the direction, the RCP determines its next coordinates. For example, a direction towards `South` would mean an increase in the `y` coordinate, thus we do `yCurrentPos = yCurrentPos + 1;` for going south.

All the Rovers are inside the (swarmBots) package inside the project files. All the implementation for each rover will be inside this package. Each rover will have different way to move based on the purpose and the tools of each Rover.

  ![image of swarmBots directory](http://i.imgur.com/LCKuaw4.png)

  When the rover’s tools and abilities have been assigned, new movement for the rover or further improvement on it can be done. Rover-02, for instance, has these pre-assigned abilities and tools:

  **Rover02: (The Walker)**

  1.Walkers move much slower than Wheels and a little slower than Treads.

  2.Walkers can travel over Soil, Gravel, and Rocks.

  3.Walkers will immediately get stuck upon entering Sand terrain.

  4.SPECTRAL_SENSOR -> Crystal Science

  Spectral sensor four is exclusive to Rover-02's, other rovers can’t catch Crystal Science on the map. Crystal objects can be detected by finding the letter ( C ) on the raw map in the project folder in eclipse.


The movement of the rover is indicated by four letters as East, West, North, South as the following:

            String[] cardinals = new String[4];
  			cardinals[0] = "N";
  			cardinals[1] = "E";
  			cardinals[2] = "S";
  			cardinals[3] = "W";

 <p> To move the rover to any of these directions, a simple call to one of the functions corresponding to that direction would suffice. This class has all the four function which each function will send the indicated letter matched the movement to the server to move your rover. </p>

<p>  Each rover is designed with a unique movement algorithm. This algorithm make sure that the rover knows where to go before moving. For example, the function below is making sure that the rover is checking whether the next move is allowed or not: </p>

          if (scanMapTiles[centerIndex +1][centerIndex].getHasRover()
              || scanMapTiles[centerIndex +1][centerIndex].getTerrain() == Terrain.SAND
              || scanMapTiles[centerIndex +1][centerIndex].getTerrain() == Terrain.NONE) {
            System.out.println(">>>>>>>EAST BLOCKED<<<<<<<<");
            blocked = true;
            stepCount = 5;  //side stepping
          } else {
            moveEast();		

<p>  ROVER-02 is not allowed to step over sand and over another rover. These conditions are also pre-assigned to each rover. However, since the rover is checking each move, the rover has to communicate with the communication server to send whether this move is allowed or not, if allowed, then move. If this move is not allowed, then based on algorithm that this rover has, get a different direction move and check.

  All these commands and communication is inside the while loop which makes the rover keep going unless the rover is stuck somewhere.

<h1>Scan Commands </h1>

The `ScanMap.java` is the main controller responsible for scanning the entire map. This program contains a scanArray, size and the coordinates as the parameters which is initialized to null if the rover is at the start position. </p>

```
public ScanMap(){
  this.scanArray = null;
  this.edgeSize = 0;
  this.centerPoint = null;		
}

public ScanMap(MapTile[][] scanArray, int size, Coord centerPoint){
  this.scanArray = scanArray;
  this.edgeSize = size;
  this.centerPoint = centerPoint;		
}
```
As it moves it stores details of the coordinate in a `scanArray` which is used to get the details of a particular coordinate. Another purpose of this program is that, this is input for creating the map by the rover, as the details of the coordinates are stored in an array.

![scanMap](http://i.imgur.com/un23bl6.png)

This helps to create the map which can be used to share the information with the communication server as well other rovers.

Each Rover has been assigned a tool. Each tool has the ability to harvest a specific type of the science. Later in this documentation, we will describe more about these tools.

One of the important things you might need to know as well is the objects and their shapes:

 Crystal ( C ) : <img src="https://s28.postimg.org/bx5ewp0nd/Screen_Shot_2017-05-03_at_2.07.34_PM.png" width="5%" /> 		Radioactive ( R ): <img src="https://s28.postimg.org/guizht2mh/Screen_Shot_2017-05-03_at_2.07.26_PM.png" width="5%"/>		


Organic ( O ): <img src="https://s28.postimg.org/dri9efnnt/Screen_Shot_2017-05-03_at_2.07.54_PM.png" width="5%"/>
Mineral( M ): <img src="https://s28.postimg.org/wvbkus0i1/Screen_Shot_2017-05-03_at_2.07.42_PM.png" width="5%"/>    

To apply any of these sciences to the map, you just need to add the letter corresponding to the science name as showing:

```
    NONE("N")
    RADIOACTIVE("Y")
    ORGANIC("O")
    MINERAL("M")
    ARTIFACT("A")
    CRYSTAL("C")
```

Also there are several types of Terrains that you can add to create you own map or add more difficulties to the existing one as showing:

<img src="https://s28.postimg.org/gzmqrh9xl/Screen_Shot_2017-05-03_at_7.02.57_PM.png" width="30%"/>

These are defined in Terrain.java in the enums package:

<img src="https://s28.postimg.org/jwed5i559/Screen_Shot_2017-05-03_at_7.12.06_PM.png" width="60%"/>

```
   NONE ("X")
   ROCK ("R")
   SOIL ("N")
   GRAVEL ("G")
   SAND ("S")
   FLUID ("F")
```

Each rover has been configured with set of tools, type of science that rover can get, and the type of terrains the rover can go over without getting stuck.
Rover tools are:

<img src="https://s28.postimg.org/j5lmzq2rt/Screen_Shot_2017-05-03_at_7.05.51_PM.png" width="30%" />

**2.What are the harvesting commands?**

The harvesting mechanism in the rovers are implemented through the `GATHER` commands and its associated mechanism. Firstly, when the `GATHER` command is issued, if the rover is positioned on a tile that contains a sample of science, and if the rover
has the proper extraction tool for that particular tile terrain, then the science is removed from the map and placed
in the rover’s cargo storage.

While traversing, the rovers keep checking if the `scienceDetail` instance of class `ScienceDetail` is null. If not, it means that the server has responded with a specific kind of science. The rover then prints out the details for that particular science first:

```
if (scienceDetail != null) {
		System.out.println("FOUND SCIENCE TO GATHER: " + scienceDetail);
	}
```

To have a deeper look at the `scienceDetail` instance, we need to go to the base class for the rovers, `ROVER.java`:

```
protected ScienceDetail analyzeAndGetSuitableScience() {
		ScienceDetail minDistanceScienceDetail = null;
		try {
			Communication communication = new Communication("http://localhost:3000/api", rovername, "open_secret");

			ScienceDetail[] scienceDetails = communication.getAllScienceDetails();
			RoverDetail[] roverDetails = communication.getAllRoverDetails();

			if (roverDetails == null || roverDetails.length == 0) {
				if (scienceDetails != null && scienceDetails.length > 0) {
					return analyzeAndGetSuitableScienceForCurrentRover(scienceDetails);
				}
			}
      ...
      ...
    }
```

The `analyzeAndGetSuitableScience()` method does the bulk of the work for the gathering procedures. The `getAllScienceDetails` method returns an array of the following:

* `x` and `y` coordinates

* if it has a rover

* the name of the science itself

* the terrain it was found on, and

* setter values for the rover that found and gathered it

Back in `ROVER_02.java`, if we get something in the `scienceDetail`, we do either of two things. First, if our current position coincides with the position of the science, we gather it and print out a statement saying the science has been gathered:

```
if ( scienceDetail.getX() == getCurrentLocation().xpos
							&& scienceDetail.getY() == getCurrentLocation().ypos ) {
						gatherScience( getCurrentLocation() );
						System.out.println( "$$$$$> Gathered science "
								+ scienceDetail.getScience() + " at location "
								+ getCurrentLocation() );
					}
```

Otherwise, the rover will head towards that science by calling the pathfinding algorithm, in this case, the A-Star. Before doing so, it sets its appropriate `driveType` and `ToolType`. Afterwards, it sets its motion based on the response it gets from the A-Star, which will be either `N` for North, `E` for East, `W` for West or `S` for South.

<h3> **3.What are the communication commands?** </h3>

The `communication.java` contains the required methods for communicating with the server as well as other rovers. This program contains code for getting the details of the rover as in what are the features it contains, the coordinate location of the rover it is approaching, the science details.
```
if (roverDetail == null) {
  throw new NullPointerException("roverDetail is null");
}
```
In this case, a `NullPointerException` is thrown because the `roverDetail` object (instance of the class `RoverDetail`) is null. Otherwise the `roverDetailMsg` JSONObject can be populated with relevant data in the following way:

```
roverDetailMsg.put("roverName", roverDetail.getRoverName());
roverDetailMsg.put("x", roverDetail.getX());
roverDetailMsg.put("y", roverDetail.getY());
```

In order to communicate with the server, several request headers are added. This depends on the API call that's being made. For example, in order to send a `GET` request to the URL `/science/all`, the following request headers are added:
```
      con.setRequestMethod("GET");
		con.setRequestProperty("Rover-Name", rovername);
		con.setRequestProperty("Content-Type", "application/json");
		con.setRequestProperty("Accept", "application/json");
```
where `con` is an instance of `HttpURLConnection`.

This communication will perform these activities for communicating with other rovers as well as with the server.

When the rovers are moving throughout the map, they will be conversing with the communication server and vice versa. In order to make that happen, we are using an API. This API provides us with a variety of options for doing RESTful services. As a result, we will be able to extract valuable information as each of the rovers are making their way through the map and define necessary actions based on the information we get. For example: when the communication server is running, the call ` /api/science/all` gives us the following result:

```
[
  {
      hasrover: false,
      science: "CRYSTAL",
      x: 27,
      y: 8,
      terrain: "SOIL",
      f: 2,
      g: 2
  },
  {
      hasrover: false,
      science: "CRYSTAL",
      x: 24,
      y: 10,
      terrain: "SOIL",
      f: 2,
      g: 2
  }
]
```
In this case, we are receiving the information that Rover_02 has found `CRYSTAL` in the terrain `SOIL` at positions `x` and `y` stated above. The Communications Server API contains several other API calls that returns detailed information for `Global`, `Sciences`, `Gather`, `Coordinate` and `Misc`.

**4.How are the pathfinding classes used?**

The basic feature of the path finding algorithm is the implementation of the A-Star algorithm. The purpose of the A-Star algorithm is to find the shortest path at each step. The rovers, while traversing through their corresponding maps, make a call to the `findPath` method in the class `A-Star` like the following:

```
char dirChar = aStar.findPath( getCurrentLocation(),
              new Coord( scienceDetail.getX(),
              scienceDetail.getY() ),driveType );
```
The above statement is implemented in the rover program, the variable `dirChar` is going to have the values in which direction the rover has to move, say `N`,`S`,`W`,`E`.

This statement is repeatedly called and it returns which direction the rover has to move next. That is, if the rover is standing at a current location then it finds the next step which it has to take based on the A-Star algorithm.

The findPath method implemented in the A-Star class returns the series of steps that the rover has to follow in order to reach to the crystal (or any other science material based on its configuration).
If there is a block then the variable will have an 'U' which means that particular coordinate cannot be visited by the rover. In the following declaration of the findPath method, the first two argument it takes are instances of the `Coord` class. When implementing we are sending the `getCurrentLocation` `ew Coord( scienceDetail.getX(),
scienceDetail.getY()` to be those two arguments. The last argument is the driveType, and instance of `RoverDriveType`:

```
public char findPath(Coord start, Coord dest, RoverDriveType drive) {
  // If destination coordinate is blocked/unreachable, return U
    if (blocked(dest, drive)) {
      return 'U';
    }
    ...
    ...
}
```
The following snippet from the method `findPath` helps to get the current location of the rover's coordinate and then stored into a variable called current:

```
 while(!openSet.isEmpty()) {
            Coord current = null;
            for(int i = 0; i < openSet.size(); i++) {
                if(current == null || fScore[openSet.get(i).xpos][openSet.get(i)
                .ypos] < fScore[current.xpos][current.ypos]) {
                    current = openSet.get(i);
                }
```
In order to check whether the rover has moved from the previous coordinate, so that it can ensure that the algorithm provides the required path towards which it has to move further.

```
if(current.equals(dest)) {
                Coord prev = cameFrom[current.xpos][current.ypos];
                while(!start.equals(prev)) {
                    current = prev;
                    prev = cameFrom[prev.xpos][prev.ypos];
                }
```

Based on the current location it redirects to the direction the rover has to move further. Thus the final result will be a character that instructs the rover of its next direction.
```
if(current.ypos < start.ypos) {
                    return 'N';
                } else if(current.xpos > start.xpos) {
                    return 'E';
                } else if(current.ypos > start.ypos) {
                    return 'S';
                } else {
                    return 'W';
                }
```

Here is a demonstration of the scenario when the Rover-02 is moving south because it received an `S` from the A-Star, meaning the nearest crystal is towards its south.

![A_Star img](https://i.imgur.com/gyjKWpF.png)


 **5.What are some design approaches to be considered for mapping behavior and harvesting behavior and when/how to switch from one to the other?**
 **Also, what are some approaches to not getting the rovers stuck in a corner?**

Currently, with each step, the rover is trying to find if it’s the closest one to a science, using A-Star:

    char dirChar = aStar.findPath( getCurrentLocation(),
          new Coord( scienceDetail.getX(),
          scienceDetail.getY() ),
          driveType );

If it is, it travels to that science and prints out the following pair of messages that have the detail for the science as well as the location.

    if(scienceDetail.getX() == getCurrentLocation().xpos
          && scienceDetail.getY() == getCurrentLocation().ypos ) {
        gatherScience( getCurrentLocation() );
        System.out.println( "$$$$$> Gathered science "
            + scienceDetail.getScience() + " at location "
            + getCurrentLocation() );
      }

On the other hand, while a rover is locating a piece of science, if it finds that another rover is closer to the science, the current rover switches to the default explore mode and lets the other rover harvest that science.

 ```
 roverDetail.setRoverMode( RoverMode.EXPLORE );
 ```

The movement for the rovers have been implemented in a way where it checks if the rover is going to face an end in the terrain in its next move:

`
if(scanMapTiles[centerIndex -1][centerIndex].getTerrain() ==Terrain.NONE){...}
`
If this is true, the rover considers that to be a block and implements the sidestepping logic it has for facing a `block`. It sets the `stepCount` to a value of 5, does sidestepping accordingly, and then check if the next move is valid once again.


**6.What equipment is available to a rover and how is it configured?**

The list of equipments available vary for each of the rovers. Upon every request to access the list of equipments for each of the rovers, we will get one drive type (instance of the `RoverDriveType` class) and two tool types (instance of the `RoverToolType` class). We do this if the command sent to the server consists of the string `“EQUIPMENT”`:

```
else if(input.startsWith("EQUIPMENT")) {
        	Gson gson = new GsonBuilder()
        			.setPrettyPrinting()
        			.enableComplexMapKeySerialization()
        			.create();
        	ArrayList<String> eqList = new ArrayList<String>();

        	eqList.add(rover.getRoverDrive().toString());
        	eqList.add(rover.getTool_1().toString());
        	eqList.add(rover.getTool_2().toString());
```

The result returned by the server will consist of a series of text strings. The number of lines of text returned is
variable.

The first line of text returned will be the string “EQUIPMENT”. The following lines will be an `ArrayList<String>` object that has been converted to a string json format.

The last line of text returned will be the string `“EQUIPMENT_END”`. When reconstructed the ArrayList will contain a listing of the Rover Drive system Type and the two RoverToolType attachments. The Drive and ToolTypes will be listed by their string converted names.

The following example shows how the `RoverToolType` and `RoverDriveType` classes handle the cases for each tool type/drive type:

```
RoverToolType output;
switch(input){
  ...
  case "SPECTRAL_SENSOR":
    		output = RoverToolType.SPECTRAL_SENSOR;
    		break;
        ...
      }
```

```
RoverToolType output;
switch(input){
  ...
  case "WALKER":
      		output = RoverDriveType.WALKER;
      		break;
        }
```

When the rovers are traversing the map, either following their default movement logic or the pathfinding (A-Star) algorithm, they will be communicating with the central server, which in our case is called the communication server. Alongside this communication server works the class `Rover.java`, which serves the purpose of being the base class for all the rovers. In this base class, we are implementing a method to retrieve a list of the rover's equipment from the server.

```
protected ArrayList<String> getEquipment() throws IOException {
  ...
  if (jsonEqListIn.startsWith("EQUIPMENT")) {
			while (!(jsonEqListIn = receiveFrom_RCP.readLine()).equals("EQUIPMENT_END")) {
				if (jsonEqListIn == null) {
					break;
				}
				jsonEqList.append(jsonEqListIn);
				jsonEqList.append("\n");
			}
		}
```

As stated above, this portion of the method checks if the string that was returned starts with `"EQUIPMENT"`. Upon satisfying this condition it goes until the last line of the text returned and keeps appending to the StringBuilder instance `jsonEqList`. Otherwise, it would simply mean that the server response did not start with "EQUIPMENT" and would return a null in that case. Finally it will return an ArrayList that contains a listing of the Rover Drive system Type and the two RoverToolType attachments.

Rovers 01, 02 and 03 have different drive types and tool types and based on their corresponding type, their actions vary. For example, due to Rover_02's having a `"SPECTRAL_SENSOR"`, it will be able to detect a crystal science, but might not be able to detect other types of science. The types are defined in the `RoverConfiguration` class:


```
    ROVER_01 ("WHEELS", "EXCAVATOR", "CHEMICAL_SENSOR"),
	ROVER_02 ("WALKER", "SPECTRAL_SENSOR", "DRILL"),
	ROVER_03 ("TREADS", "EXCAVATOR", "RADAR_SENSOR"),
```

In this project, we are running a simulation of NASA's mars rovers. In reality, for NASA's Mars Science Laboratory mission, Curiosity, the following are the detectors and their related instruments:

![radiation_detector_nasa_curiosity](http://i.imgur.com/s9nxYK6.png)

More information for Curiosity's sensors and detectors can be [found here](https://mars.nasa.gov/msl/mission/instruments/radiationdetectors/)



**7.Describe the different drive and tool types, how they are used and how they interact with the environment. Go into some of the design considerations for choosing different equipment configurations for small (5) medium (6-10) and large (10+) groups of rovers. How should tools and drive types be mixed?**

The different types of drive and tool types are:

* The wheels which can travel on soil and gravel but not on rock and sand

* The walkers are the slowest of all the three types of the drive tools. The main advantage is that they can walk over all except sand.

* The treads are similar to walkers but are little faster when compared with the walkers. The main purpose of these treads are that they are used to run over sand.

Next comes the extraction tools, there are two types of extraction tools:  

* Drills can extract samples of science from Rock or Gravel terrain.
* Excavators can extract samples of science from Soil or Sand terrain.

These are used in rock and gravel, soil and sand respectively. Another important tool is the scanner tools, the scanning tools are:

* Radiation Sensor(Scans radioactive material)
* Chemical Sensor(Scans organic material)
* Spectral Sensor(Scans crystal science material)
* Radar Sensor(Scans mineral science material)

Another type of tool is the Range Extender, which helps to extend the visibility from 7x7 to 11x11 square.

All the rovers should have the extraction tools. Let 1/3 of the rovers be wheelers, other 4/5 be walkers and treads. The main reason for the wheelers is less because they can move faster and extract in larger amount.


**8.Make some recommendations on how to improve the implementation of the project. Make some recommendations on additional features and functions to add to the simulation such as, liquid terrain features, hex vs. square map tiles, power limitations (solar, battery, etc.), towing, chance of break downs, etc**

  - One of the difficulties that might be added to the rover's movements are the ability to avoid obstacle that aren't part of their configuration.  
  - In the future, rovers could be configured to know whether they have already Explored a certain area so they wouldn't go back there again.
  - One of the ideas that could be implemented later is that rovers can establish a separate communication protocol for a map and another for movements.

The rovers can be given the ability to sense the liquid terrain and also need to ensure that they can drill through them. While moving, it has to be ensured that the rover does not get toppled upside down. Additional features that could be added are, to prevent the parts from eroding by the exposure of cosmic rays, additional sensors can to be added to enhance each of the rovers' features.
