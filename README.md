robogen-simulator
=================

The ROBOGEN simulator

The simulator can be compiled on Linux, Windows and MAC OSX. Unfortunately, this semester the code is still 
experimental and therefore we do not have any precompiled distributable to ship you. 
Hopefully, thanks to your help, we will produce a stable and robust codebase for the next semester students.

Please, use github as much as possible to report us bugs, issues and ideas for improvement.
Whenever you signal a bug, make sure your description contains:
1) Machine on which the error happened, OS version, installed libraries versions (See dependendencies later)
2) A List of instructions needed to reproduce your bug.
Also, given the premature state of the project, we will keep into consideration your effort in helping us improving the code
in the course evaluation!

Thanks to you and sorry for any (many I guess, at the current stage) incovenient :-) 

## Compile the simulator

### Dependencies
Before starting to compile the ROBOGEN simulator you will need to download and 
make available to your compiler includes and library paths to:
* zLib (http://www.zlib.net/)
* libpng 1.5 (http://libpng.sourceforge.net/index.html)
* Boost (http://www.boost.org/)
* Google Protobuf (http://code.google.com/p/protobuf/)
* OpenSceneGraph (http://www.openscenegraph.org/projects/osg/wiki/Downloads) 
* ODE (http://sourceforge.net/projects/opende/files/) 

ODE must be compiled in double precision mode (dDOUBLE preprocessor flag)

On Linux we advise to install all the needed dependencies from available repositories, except for ODE 
which has to be built in double precision mode.
For example, in Ubuntu Linux 12.04 you can type

    sudo apt-get install zlib1g zlib1g-dev libpng12-0 libpng12-dev libboost1.48-dev libprotobuf-dev \
    protobuf-compiler libopenscenegraph-dev libopenscenegraph80

to install the required dependencies.

Furthermore, you will need to download and install Cmake (http://www.cmake.org/)

*Carefull : Visual Studio express 2012 is not supported yet by any version of cmake.
Check : http://www.microsoft.com/visualstudio/eng/downloads for having Visual Studio 2010

### Simulator build

1) Once the required dependencies are satisfied (all code compiled successfully), clone this repository
to get all the needed source code of the simulator

    git clone https://github.com/amaesani/robogen-simulator.git
    
2) Modify the paths in src/cmake/CustomPath.cmake pointing them to the correct libraries/include directories.
Note that you need to modify only those paths pointing to non-standard locations, i.e. if you installed a library under Linux
under a standard path (e.g. /usr/lib) you do not need to update the corresponding line in CustomPath.cmake


3) Create an empty directory somewhere in your filesystem. We will call this directory ROBOGEN_DIR. Instead, we will
refer to the directory that contains the source code (robogen-simulator/src) as the ROBOGEN_SOURCE_DIR

Then depending on your OS the next steps might change.
On Linux, MAC OS X and Windows/MinGW, from terminal, enter ROBOGEN_DIR and run 

    cd ROBOGEN_DIR
    cmake -DCMAKE_BUILD_TYPE=Release ROBOGEN_SOURCE_DIR
    make -j4
    
On Windows/Visual Studio... [this must be contributed by someone :-)]

### Generating projects for your IDE (Eclipse, Visual Studio, etc.)

Using CMAKE you can as well generate projects for different IDEs. 
For example, to generate a project that can be imported in eclipse

    cd ROBOGEN_DIR
    cmake -DCMAKE_BUILD_TYPE=Debug -G"Eclipse CDT4 - Unix Makefiles" ROBOGEN_SOURCE_DIR

Please check the CMAKE manual for the correct command to generate projects for other IDEs.

## Executables

The most important executables are:
* robogen-file-viewer, a small utility to visualize the robot structures.
* robogen-simulator, the main simulator software, to be used as a server software, listen for connections from the ROBOGEN EA,
  evaluates robots and returns their fitness to the EA

    robogen-file-viewer ROBOT_FILE CONF_FILE
    
where ROBOT_FILE is the robot structure and configuration file is a file containing the ROBOGEN simulator configuration

    robogen-simulator PORT
    
where PORT is the port on which the server will listen for connections.
    
### Configuration file

#### Main configuration file

    scenario=racing                      # Can be racing OR chasing
    timeStep=0.01                        # Timestep [s]
    nTimeSteps=4000                      # Number of timesteps 
    terrainType=flat                     # Flat or rough 
    terrainLength=1                      # [m]
    terrainWidth=1                       # [m]
    terrainHeight=0.5                    # [Optional] Max terrain altitude [m]
    terrainHeightField=test.gif          # [Optional] a grayscale image
    obstaclesConfigFile=obstacles.txt    # Obstacles configuration file
    startPositionConfigFile=startPos.txt # Start position configuration file
    
#### Obstacles file

    #List of obstacles positions and size, separated by a tab, an obstacle per line
    X Y X_SIZE Y_SIZE Z_SIZE

#### Start Position file

    #List of starting positions, separated by a tab.# A starting position per line
    X Y

## Running the Robogen Simulator

Once the simulator compiled succesfully, remember to copy the robogen-simulator/models/ folder into the folder from which you
will run your executable, otherwise the 3D models for the ROBOGEN robots cannot be loaded.
Check in the examples/ folder for sample configuration files and 5 examples robot structure that can be visualized.
