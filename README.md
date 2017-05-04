<h1> # Rover Swarm Project - Rover 02 </h1>

<p> This project is a simulation of a set of autonomous rovers that are exploring, mapping, and harvesting science on an alien planet. The rovers have very limited capabilities individually and therefore have to operate as a swarm to fulfill their objectives. </p>

![image of mars rovers](http://i.imgur.com/8n6arMu.jpg)

<h3> ## Group-2 </h3>

<h4> **1.What are the movement commands?  What are the scan commands?** </h4>

<p> The basic movement commands are the `moveNorth()`,`moveEast()`,`moveSouth()` and `moveWest()`. These methods are called from `Rover.java` which prints a statement with help of a [PrintWriter](https://docs.oracle.com/javase/7/docs/api/java/io/PrintWriter.html) object. </p>

This is how the `moveNorth()` method is implemented:

    	protected void moveNorth() {
		    sendTo_RCP.println("MOVE N");
	    }

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

  NONE("N") <br />
	RADIOACTIVE("Y") <br />
	ORGANIC("O")<br />
	MINERAL("M")<br />
	ARTIFACT("A")<br />
	CRYSTAL("C")<br />
  
Also there are several types of Terrains that you can add to create you own map or add more difficulties to the existing one as showing: 

<img src="https://s28.postimg.org/gzmqrh9xl/Screen_Shot_2017-05-03_at_7.02.57_PM.png" width="30%"/>

These are defined in Terrain.java in the enums package:

<img src="https://s28.postimg.org/jwed5i559/Screen_Shot_2017-05-03_at_7.12.06_PM.png" width="60%"/>


   NONE ("X")	<br />
   ROCK ("R")   <br />
   SOIL ("N") <br />
   GRAVEL ("G") <br />
   SAND ("S") <br />
   FLUID ("F")

Each rover has been configured with set of tools, type of science that rover can get, and the type of terrains the rover can go over without getting stuck. 
Rover tools are: 

<img src="https://s28.postimg.org/j5lmzq2rt/Screen_Shot_2017-05-03_at_7.05.51_PM.png" width="30%" />

<h1>Scan Commands </h1>

<p> The ScanMap.java is the main controller of scanning the entire map. This program contains a scanArray, size and the coordinates as the parameters which is initialized to null if the rover is at the start position and as it moves it takes it stores details of the coordinate in a scanArray which is used to get the details of a particular coordinate. Another purpose of this program is that, this is input for creating the map by the rover, as the details of the coordinates are stored in an array this helps to create the map which can be used to track the information but the communication server as well other rovers </p>

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

**4.How are the pathfinding classes used?**

pending

**5.What are some design approaches to be considered for mapping behavior and harvesting behavior and when/how to switch from one to the other? Also, what are some approaches to
not getting the rovers stuck in a corner?**

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

 ```roverDetail.setRoverMode( RoverMode.EXPLORE );```


**6.What equipment is available to a rover and how is it configured?**

pending

**7.Describe the different drive and tool types, how they are used and how they interact with the
environment. Go into some of the design considerations for choosing different equipment
configurations for small (5) medium (6-10) and large (10+) groups of rovers. How should tools
and drive types be mixed?**

The different types of drive and tool types are:

* The wheels which can travel on soil and gravel but not on rock and sand

* The walkers are the slowest of all the three types of the drive tools. The main advantage is that they can walk over all except sand.

* The treads are similar to walkers but are little faster when compared with the walkers. The main purpose of these treads are that they are used to run over sand.

Next comes the extraction tools, there are two types of extraction tools:  

* Drills
* Excavators

These are used in rock and gravel, soil and sand respectively. Another important tool is the scanner tools, the scanning tools are:

* Radiation Sensor(Scans radioactive material)
* Chemical Sensor(Scans organic material)
* Spectral Sensor(Scans crystal science material)
* Radar Sensor(Scans mineral science material)

Another type of tool is the Range Extender, which helps to extend the visibility from 7x7 to 11x11 square.

All the rovers should have the extraction tools. Let ⅕ of the rovers be wheelers, other ⅖ be walkers and treads. The main reason for the wheelers is less because they can move faster and extract in larger amount.

**8.Make some recommendations on how to improve the implementation of the project. Make some recommendations on additional features and functions to add to the simulation such as, liquid terrain features, hex vs. square map tiles, power limitations (solar, battery, etc.), towing, chance of break downs, etc**

The rovers can be given the ability to sense the liquid terrain and also need to ensure that they can drill through them. While moving, it has to be ensured that the rover does not get toppled upside down. Additional features that could be added are, to prevent the parts from eroding by the exposure of cosmic rays, additional sensors can to be added to enhance each of the rovers' features.
