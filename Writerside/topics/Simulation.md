# Simulation: DSimu 

`DSimu` is the simulation program built on Geant4.
It simulates the DarkSHINE detector setup and is controlled through YAML configuration files, allowing for easy configuration and adjustment.
`DSimu` generates the data that is later analyzed and reconstructed in other components of DSS.


## Command-Line Arguments for DSimu {collapsible="true"}

{type="medium" sorted="desc"}
-m, --macro (<format color="Red">DEPRECATED</format>)
: **Type**: `std::string`  
**Description**: Name of the macro file to be used by the simulation. It can be used to control the simulation setup.

-o, --opticalMacro
: **Type**: `std::string`  
**Description**: Name of the optical macro file for configuring optical properties in the simulation.

-y, --yaml
: **Type**: `std::string`  
**Description**: YAML configuration file for setting up the simulation parameters.

-s, --seed
: **Type**: `long`  
**Description**: Random seed for the simulation. If not specified, it's read from the YAML configuration.

-f, --outfile_Name
: **Type**: `std::string`  
**Description**: Name of the output file. If not set, the name is read from the YAML configuration.

-n, --Run_Number
: **Type**: `int`  
**Description**: Run number for this job. It is used to calculate the EventID, where `EventID = id + beam_on * Run_Number`. If not set, the value is read from the YAML file.

-b, --beam_on
: **Type**: `int`  
**Description**: Number of beams to be simulated. If not specified, the value is read from the YAML configuration.

-G, --gui
: **Type**: `bool`  
**Description**: Flag to indicate if the GUI mode should be enabled.
Default is `false`.
Two macro files (`init_vis.mac` and `gui.mac`) are needed for display in the run directory. 
**Not recommended unless you know the geometry**

--save_geometry
: **Type**: `bool`  
**Description**: Flag to indicate if the geometry should be saved. Default is `true`. If not set, the value is read from the YAML configuration.

-v, --version
: **Type**: `bool`  
**Description**: Flag to indicate if the program version should be printed. Default is `false`.

-a, --animation
: **Type**: `bool`  
**Description**: Flag to indicate if animation information should be stored for DDis. Default is `false`. This is useful for creating animations in the event display.

