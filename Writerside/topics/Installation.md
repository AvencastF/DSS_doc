# Installation Guide for DarkSHINE Software (DSS)

This guide provides step-by-step instructions for installing and setting up DarkSHINE Software (DSS) for the DarkSHINE experiment.

## Cloning the Repository

> Make sure you have access to IHEP GitLab.
> 
{style="note"}

DarkSHINE Software can be downloaded from GitLab. Use the following command to clone the repository:

```bash
git clone git@code.ihep.ac.cn:darkshine/darkshine-simulation.git
```

<tabs>
<tab title="Baseline %version% (latest)">
Using master branch as default, but always check the tags. 
</tab>
<tab title="Baseline 1.0">
If you need to run Baseline 1 samples, use this command to switch to the correct branch:

```bash
    git checkout tags/baseline1
```
</tab>
</tabs>



**Note**: The current version of configuration files for DSimu and DAna might not be compatible with previous releases. For more information, check the [Wiki page](https://code.ihep.ac.cn/darkshine/darkshine-simulation/-/wikis/Sample-Production).

## Check Dependencies

Before installing DSS, ensure you have the following dependencies on your machine:

- **C++17**
- **Geant4 10.06**
- **ROOT 6 (&#8805; 6.20)**
- **gsl**
- **yaml-cpp**
- **[nlohmann/json](https://github.com/nlohmann/json)**

Alternatively, if you're using clusters with CVMFS installed, you can directly source the LCG environment with this command:

```shell
source /cvmfs/sft.cern.ch/lcg/views/LCG_97rc4python3/x86_64-centos7-gcc9-opt/setup.sh
```

## Build and Install DSS

With the necessary dependencies, you can build and install DSS:

```bash
cd darkshine-simulation   # Source directory
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=<your-install-directory> ../
# Use parallel build to speed up the process
make -j100  # not necessarily 100 😇
make install
```

## Export Environment Variables

To ensure DSS runs properly, create a script to set up [environment variables](https://en.wikipedia.org/wiki/Environment_variable). 
Here is an example of what `setup.sh` should contain:

```shell
source /cvmfs/sft.cern.ch/lcg/views/LCG_97rc4python3/x86_64-centos7-gcc9-opt/setup.sh
DSS_DIR=<your-install-directory>
export PATH=${DSS_DIR}/bin:${PATH}
export LD_LIBRARY_PATH=${DSS_DIR}/lib:${LD_LIBRARY_PATH}
```

After creating the script, source it to apply the environment variables:

```bash
source setup.sh
```

## Final Check and Quick Guide on DSS

With everything set up, check your install directory to ensure everything is in place. 
Now you’re ready to explore the software and start using it.

### Quick Guide on DSS

For example scripts, check the "scripts" folder in the corresponding source directories. 
This folder contains various examples to help you get started with DSS.

