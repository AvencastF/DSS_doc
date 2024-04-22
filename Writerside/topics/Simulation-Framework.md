# DarkSHINE Software Overview

<format style="bold" color="DarkViolet">D</format>ark<format style="bold" color="DarkViolet">S</format>HINE <format style="bold" color="DarkViolet">S</format>oftware (<format style="bold" color="DarkViolet">DSS</format>) is a software package designed for the DarkSHINE experiment. 
It encompasses a comprehensive set of tools for simulation, analysis, and data visualization. 
The package consists of five key components: `DSimu`, `DAna`, `DDis`, `DPlot`, and `DEvent`.

## Components of DSS

### DSimu
`DSimu` is the simulation program built on Geant4. 
It simulates the DarkSHINE detector setup and is controlled through YAML configuration files, allowing for easy configuration and adjustment. 
`DSimu` generates the data that is later analyzed and reconstructed in other components of DSS.

### DAna
`DAna` is a framework for the analysis and reconstruction tools. 
It works with the output ROOT files from `DSimu`, 
processing data related to the detector geometry, magnetic fields, and events. 
`DAna` provides the infrastructure needed to analyze and reconstruct experimental data from the DarkSHINE experiment.

### DDis
`DDis` is the event display for DSS. 
It visualizes the geometry and events from the simulation and reconstruction processes. 
This component allows researchers to view and analyze events in a graphical format, 
providing visualization information into the experiment’s outcomes.

### DPlot
`DPlot` is a quick plotting program designed for ease of use. 
It's ideal for new users or those who need to generate plots quickly without complex setups. 
`DPlot` simplifies the process of creating visual representations of data from DSS.

### DEvent
`DEvent` is the generic data structure in DSS. 
It serves as a common format for storing and processing event-related data, 
allowing for seamless integration between the different components of the DSS package.
