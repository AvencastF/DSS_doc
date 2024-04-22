# Workflow

The DarkSHINE Software (DSS) framework
involves [a multi-step workflow](#workflow "The workflow chart for the whole DarkSHINE Software Framework") that
generates and processes data for the DarkSHINE experiment.
This description outlines the data flow through the various stages of the workflow.

## Global Information

During simulation, reconstruction, and analysis,
some global information remains consistent across all stages: the **experiment setup**.
This setup includes the **geometry and material of the detectors**,
as well as the **magnetic fields**, all of which should remain unchanged throughout the process.

Typically, a "run" is defined as a collection of events that **share the same experiment setup**.
Reconstruction and analysis are usually developed and optimized for a specific run,
ensuring that they align with the corresponding experimental conditions.

## Generating Physics Processes in DSS

> The **first** stage in conducting simulations is deciding which process to simulate.
> This decision sets the groundwork for the entire simulation workflow.
> The output of generators is called **parton-level** information.
>
{style="note"}

To simulate the physics processes of interest in the DarkSHINE Software (DSS),
we have two main approaches: using generators like [Madgraph](https://launchpad.net/mg5amcnlo)
and [CalcHEP](https://launchpad.net/calchep),
or using physics processes defined in Geant4.

{collapsible="true"}
Using Generators
: Generators like [Madgraph](https://launchpad.net/mg5amcnlo) and [CalcHEP](https://launchpad.net/calchep)
are widely used for estimating cross-sections and calculating the <tooltip term="diff_xsec">differential
cross-sections</tooltip> for final state particles based on the initial state.
This approach provides high precision in simulating specific processes with specific models and conditions.
:
- **Advantages**: These generators are designed to simulate specific processes with precise conditions, typically in
  collider physics. They require knowledge of the incident particles' 4-momentum and absolute position to estimate
  cross-sections accurately.
- **Disadvantages**: Using these generators may require additional setup, and they are more rigid, focusing on specific
  processes.

Using Geant4 Physics Processes
: Geant4 has
a [set of predefined physics processes](https://geant4-userdoc.web.cern.ch/UsersGuides/ForApplicationDeveloper/html/TrackingAndPhysics/physicsProcess.html)
for simulating particle interactions.
This approach offers a smoother simulation of particle processes without focusing on a specific process.
:
- **Advantages**: This method is more flexible and closely resembles real-world physics processes. It relies on
  theoretical calculations and experimental measurements to simulate processes throughout the detector range.
- **Disadvantages**: If a specific process is not defined in Geant4’s physics list, you’ll need to create your own
  process definition.

### Combining Generators and Geant4 in DSS

In DSS, we combine both approaches:

- **Standard Model Processes**: Rely on the existing Geant4 physics database for well-known Standard Model processes,
  simulating interactions across the whole detector range.
- **Special Physics Processes**: Use generators to simulate special physics processes, like Dark Bremsstrahlung. This
  involves using the incident particles under certain conditions, then inserting final state particles into the
  simulation.

> When using generators for special processes, it’s crucial to estimate the state of the incident particles accurately.
> The impact of varying the state on differential cross-sections should be studied carefully to maintain the accuracy
> and validity of the simulation.

## Event Simulation using DSimu

> This is the **second** stage that the actual simulation program is run using the output from the generators
> (if targeting a certain process).
> This stage is where the **raw simulated events** are generated.
>
{style="note"}

`DSimu` is the primary simulation program for DarkSHINE Software (DSS), generating Monte Carlo data based on Geant4
simulations. It simulates **particle interactions within the detector**, providing key data such as incident particles,
momentum, and other critical parameters.

### Key Points in Simulation

{collapsible="true"}
Incident Particles
{collapsible="true" default-state="expanded"}
: Incident particles are the initial state particles in a fixed-target experiment like DarkSHINE. These particles are
typically part of a beam, with a certain spread along the transverse plane (x, y plane), moving along the z-direction.
Depending on the beam source, incident particles can be electrons, positrons, or protons. In the case of DarkSHINE, the
incident energy is relatively stable, typically 8 GeV (or 4, 16 GeV). This represents the "Particle Gun" in
the [workflow
chart](#workflow "The workflow chart for the whole DarkSHINE Software Framework").

Sensitive Detectors
{collapsible="true" default-state="expanded"}
:
The [sensitive detector](https://geant4-userdoc.web.cern.ch/UsersGuides/ForApplicationDeveloper/html/Detector/hit.html?highlight=sensitive#sensitive-detector)
is a crucial concept in Geant4 simulation. It refers to the sensitive parts of the detector
responsible for recording data. In a real experiment, data is only recorded from specific parts of the detector. For
instance, in a sampling detector, information is gathered from scintillator modules but lost in absorber modules.
Scintillator modules become the sensitive detectors in this context.
: In simulation programs, sensitive detectors must be well-defined, as they determine the raw simulated data. It's
critical to analyze only the information from the sensitive detectors, as that's the only data available in real
experiments. Any additional truth information serves as a supplement to the raw sensitive detector information.

> The terms "raw detector response," "raw detector hit," and "simulated hit" generally refer to the same concept,
> focusing on the data from sensitive detectors.

{collapsible="true"}
User Actions
{collapsible="true" default-state="expanded"}
: [User actions](https://geant4-userdoc.web.cern.ch/UsersGuides/ForApplicationDeveloper/html/UserActions/userActions.html)
in Geant4 define what the simulation program should do during the simulation, such as collecting specific
truth information or controlling the program flow. There are various classes of user actions responsible for different
levels of simulation, including run, event, track, step, and stack. Developers need to balance complexity and time
consumption when implementing user actions.

> Most truth information is recorded through user actions.

#### Data Recording

The global event data structure, `DEvent`, stores the simulation data. Ideally, you'd record as much information as
possible. However, given finite computing power and storage, there's often a trade-off between the level of detail and
the required resources. The goal is to record enough information for meaningful analysis without overwhelming the system
with excessive data.

> Advanced topics about simulation techniques such as biasing, filtering, multi-threading will be discussed in [Simulation](Simulation.md).

## Reconstruction and Analysis using DAna

> This is the **third** part of the workflow. The most crucial aspect of this stage is ensuring that the same algorithms
> are
> used for reconstructing and analyzing **both the simulated events and the real data events**. This consistency is key
> to
> validating the simulation against actual experimental data and drawing meaningful conclusions from the results.
>
{style="note"}

Once the data has been generated and prepared, it undergoes further processing and analysis.
`DAna` is the analysis and reconstruction framework. DAna uses ROOT files generated by `DSimu` to perform detailed
analyses of particle interactions and processes. It is responsible for reconstructing events, extracting relevant
parameters, and analyzing the experimental outcomes.

{collapsible="true"}
Why Digitization is Necessary
: In simulation, raw simulated hits often do not accurately reflect real-world data due to their high precision and lack
of noise or other real-world effects. Digitization introduces necessary modifications to make these raw hits resemble
real data more closely, accounting for factors like detector resolution, radiation effects, and electronic noise. The
goal is to create a simulation that aligns with actual experiment outcomes, ensuring a more reliable reconstruction and
analysis process.

### Digitization and Reconstruction in the Tracking System

The tracking system captures charged particles, and its digitization process transforms raw simulated hits into more
realistic data.

{collapsible="true"}
Digitization for the Tracking System
: The digitization of the tracker is relatively straightforward. The DarkSHINE tracker consists of two sub-layers of
silicon strips crossing at a small angle. When a particle strikes the tracker, it activates two strips. Digitization
involves finding the intersection of these strips, which represents the hit point. This method helps ensure that tracker
hits mimic real detector responses.

Track Reconstruction in the Tracking System
: Track reconstruction in the tracking system aims to determine the number and momentum of tracks. The system comprises
two regions: tagging and recoil. In the tagging region, tracker hits are relatively clean, providing a reliable
benchmark for track reconstruction. In the recoil region, due to complex interactions, track reconstruction is more
challenging but crucial for the experiment's success.

### Digitization and Reconstruction in the Electromagnetic Calorimeter (ECAL)

The electromagnetic calorimeter captures energy from particles entering its range. Digitization for the ECAL introduces
realistic variations to the raw simulated hits.

{collapsible="true"}
Digitization for the ECAL
: The digitization process for the ECAL relies on optical photon simulation and real measurements to calibrate the raw
hits. This involves scaling the raw data by a measured curve, introducing fluctuations in energy and time, reflecting
realistic detector behavior.

Cluster Reconstruction in the ECAL
: Cluster reconstruction in the ECAL focuses on capturing energy deposited by particles. Although the ECAL lacks the
granularity of the tracking system, it offers excellent energy resolution. For high-momentum particles, the ECAL
provides reliable data, while low-momentum particles are better analyzed using the tracking system. A combined approach
helps reconstruct complete particle tracks.

### Digitization and Reconstruction in the Hadronic Calorimeter (HCAL)

The hadronic calorimeter is primarily used to detect neutral particles and minimally ionized particles, often to veto
potential backgrounds.

{collapsible="true"}
Digitization for the HCAL
: The digitization process for the HCAL is similar to that of the ECAL, involving optical photon simulation and real
measurements. It scales raw hits by a measured curve and introduces reasonable fluctuations in energy and time, making
the HCAL data more realistic.

Reconstruction in the HCAL
: Reconstruction in the HCAL is typically focused on detecting neutral particles and minimally ionized particles. Given
its multi-layer structure, HCAL can also be used for basic track reconstruction of these particles, aiding in background
identification and rejection.


## Event Analysis and Sensitivity Study

After reconstruction, event analysis focuses on extracting meaningful information from the simulated and real data. 
This stage involves identifying key features of the reconstructed events, 
such as particle types, energies, and interactions. 
Analysts apply various algorithms and statistical methods to study event characteristics and evaluate the experiment’s performance.

Sensitivity study, on the other hand, assesses how well the experiment can detect specific signals or physics processes. 
This study involves varying experimental conditions, like particle energies and detector settings, 
to understand the experiment's capability to detect rare events or new physics phenomena. 
Sensitivity analysis is critical for optimizing the experiment and guiding future research directions.


## DarkSHINE Software Workflow Chart

![DSS Workflow](DarkSHINE_Software_Workflow.png) { border-effect="rounded" thumbnail="true" id="workflow" }
