# OpenFASOC
Working in understanding the flow for FASOC and researching to implement more analog system generation using the similar flow,

First we need to install the necessary tools:-
# OpenFASOC

## Installation of the required tools

1. OpenFASOC:

```
git clone https://github.com/idea-fasoc/openfasoc

cd openfasoc

pip install -r requirements.txt

```

For the complete steps of installing OpenFASOC, refer Manual Installation from  `https://github.com/idea-fasoc/OpenFASOC/blob/main/docs/source/getting-started.rst`.

2. OpenROAD:

```
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD.git

cd OpenROAD

./etc/DependencyInstaller.sh

./etc/DependencyInstaller.sh -run

./etc/DependencyInstaller.sh -dev

mkdir build

cd build

cmake ..

make

```


3. Klayout

Downlaod the latest version of the Klayout from `https://www.klayout.de/build.html`. Install the following dependencies: qt5-default, qttools5-dev, libqt5xmlpatterns5-dev, qtmultimedia5-dev, libqt5multimediawidgets5 and libqt5svg5-dev.
```
sudo apt-get install -y libqt5widgets5

sudo dpkg -i klayout_0.27.11-1_amd64.deb

```

4. Netgen

```
sudo add-apt-repository ppa:ngsolve/ngsolve

sudo apt-get update

sudo apt-get install ngsolve

```


5. OpenPDK

```
 git clone git://opencircuitdesign.com/open_pdks
 open_pdks
 ./configure --enable-sky130-pdk
 make
 sudo make install
```


6. Magic

Installation of Prerequisites:
```
sudo apt-get install m4

sudo apt-get install tcsh

sudo apt-get install csh

sudo apt-get install libx11-dev

sudo apt-get install tcl-dev tk-dev

sudo apt-get install libcairo2-dev

sudo apt-get install mesa-common-dev libglu1-mesa-dev

sudo apt-get install libncurses-dev
```

Magic Installation:
```
git clone https://github.com/RTimothyEdwards/magic

cd magic/

./configure

sudo make

sudo make install
```

7. Yosys


Installation of Prerequisites:
```
sudo apt-get install build-essential clang bison flex \
   libreadline-dev gawk tcl-dev libffi-dev git \
   graphviz xdot pkg-config python3 libboost-system-dev \
   libboost-python-dev libboost-filesystem-dev zlib1g-dev
```

To install the latest version:
```
git clone https://github.com/YosysHQ/yosys.git

make

sudo make install 

make test
```





## Temperature Sensor Generator
An all-digital temperature sensor, that relies on a new subthreshold oscillator (achieved using the auxiliary cell ???Header Cell???) for realizing synthesizable thermal sensors.

The way that works is we have a subthreshold current that has an exponential dependency on the temperature, the frequency generated from the subthreshold ring oscillator is also dependent on temperature. So we can sense the temperature by comparing the difference between the clock frequency generated from a reference oscillator and the clock frequency from the proposed frequency generator.


### Understanding the TEMP_ANALOG.V files:

#### Block Architecture

![image](https://user-images.githubusercontent.com/110079729/199910810-4962f9ed-95e8-4857-ae3f-8acf9d9fe634.png)


Theses are the verilog template files which are used for the creation of netlist verilog files.

The placeholders in the verilog template files are replaced with the aux_cell info. The difference between the template and the generated verilog file can be found below. (Example)


![File1](https://user-images.githubusercontent.com/110079763/200115519-34c977ba-12e5-4acb-98e3-5479c6635ccd.png)



The temp_gen_netlist function can be found in ``` openfasoc/generators/temp-sense-gen/tools/TEMP_netlist.py ```
![image](https://user-images.githubusercontent.com/110079729/199912171-8b1a8c07-f80b-4853-8688-4e7d59bc7eac.png)


The new netlist files are now available in the ```OpenFASOC/openfasoc/generators/temp-sense-gen/src``` folder and the ```OpenFASOC/openfasoc/generators/temp-sense-gen/flow/design/src/tempsense```.

![image](https://user-images.githubusercontent.com/110079729/199912506-0c4df66b-e0fd-4c81-8ed3-f6f8d2a6d967.png)


The generated netlist files are as follows:

* counter.v from counter_generic.v
* TEMP_ANALOG_hv.nl.v from TEMP_ANALOG_hv.v
* TEMP_ANALOG_lv.nl.v from TEMP_ANALOG_lv.v

