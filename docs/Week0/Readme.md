***Task-1 : Create GitHub repo. Document summary of previous video(Getting Started with Digital VLSI SOC Design and planning) in the GitHub repo.***

Summary of week0:

Chip modelling :
Write a Test bench in C language(The application that is going to be designed)

1) Test above application using GCC complier --> output is O0

2) Test the same application using specs GCC(eg: RISCV GCC) --> output is O1
If O0 = O1, Then our specifications are fixed.

3) Write softcopy of Hardware using RTL(Verilog) and Test with the application --> output is O2
If O1 = O2, Then our hardware softcopy is also fine.

  Then, Design SoC
  (i) Processor -> Gate level Netlist (synth)
  (ii) Peripherals/IPs -> Macros(synth RTL)   and   -> Analog IPs (func RTL)

4) Test the application with the SoC Integration --> output is O3. 
Then O2 = O3.
Then comes Floorplanning, Placement, Routing, Physical design. 
This combinely completes RTL to GDSII flow.
The GDSII is sent to factory(Tape out) after DRC/LVS checks.

5) After integration of physical chip with pheripherals, run the application --> output is O4. 
Then O0 = O1 = O2 = O3 = O4.



***Task-2 : Install tools listed in the document using the machine configuration mentioned. Update
your GitHub repo with Tool snapshot.***

**System Check**

6GB RAM, 50 GB HDD

Ubuntu 20.04+

4vCPU

**Tool check**

**YOSYS**


```bash
$ sudo apt-get update
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make (If make is not installed please install it)
$ sudo apt-get install build-essential clang bison flex \
libreadline-dev gawk tcl-dev libffi-dev git \
graphviz xdot pkg-config python3 libboost-system-dev \
libboost-python-dev libboost-filesystem-dev zlib1g-dev
#Yosys depends on a GitHub submodule called **abc**, which must be initialized before building. If not, you may see errors during compilation. 
$ git submodule update --init --recursive
$ make config-gcc
$ make
$ sudo make install
```
<img width="1436" height="827" alt="Screenshot from 2025-09-19 10-21-19" src="https://github.com/user-attachments/assets/21ad404c-2b18-4063-a461-7c6235a83b24" />

**IVerilog**


```bash
$ sudo apt-get update
$ sudo apt-get install iverilog
```
<img width="1093" height="751" alt="Screenshot from 2025-09-20 12-19-45" src="https://github.com/user-attachments/assets/fe17d327-89bf-4163-a4a6-2262eeba98b4" />

**gtkwave**


```bash
$ sudo apt-get update
$ sudo apt-get install gtkwave
```
<img width="818" height="152" alt="Screenshot from 2025-09-20 12-23-22" src="https://github.com/user-attachments/assets/fed61913-2ff9-4526-935e-bf2ed93ca89a" />

**NGSpice** 

After downloading the tarball from https://sourceforge.net/projects/ngspice/files/ to a local directory, unpack it using:

```bash
$ tar -zxvf ngspice-45.2.tar.gz
$ cd ngspice-45.2
$ mkdir release
$ cd release
$ ../configure --with-x --with-readline=yes --disable-debug
$ make
$ sudo make install
```
<img width="990" height="664" alt="Screenshot from 2025-09-20 12-41-23" src="https://github.com/user-attachments/assets/ffc9e005-dd1f-4db8-88bf-3428c48966da" />


**magic** 
```bash
$ sudo apt-get install m4
$ sudo apt-get install tcsh
$ sudo apt-get install csh
$ sudo apt-get install libx11-dev
$ sudo apt-get install tcl-dev tk-dev
$ sudo apt-get install libcairo2-dev
$ sudo apt-get install mesa-common-dev libglu1-mesa-dev
$ sudo apt-get install libncurses-dev
$ git clone https://github.com/RTimothyEdwards/magic
$ cd magic
$ ./configure
$ make
$ make install
```
<img width="1506" height="830" alt="Screenshot from 2025-09-20 12-48-57" src="https://github.com/user-attachments/assets/ad34b6c2-a82f-448e-ba45-a50d956d739d" />


**Openlane**
```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt install -y build-essential python3 python3-venv python3-pip make git
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
$ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https: download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt sources.list.d/docker.list > /dev/null
$ sudo apt update
$ sudo apt install docker-ce docker-ce-cli containerd.io
$ sudo docker run hello-world
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ sudo reboot
# After reboot
$ docker run hello-world
```
**check dependencies**

<img width="867" height="384" alt="Screenshot from 2025-09-20 13-06-58" src="https://github.com/user-attachments/assets/2e3f8827-06ce-46cc-8e83-4cff0dc94efa" />

<img width="1000" height="785" alt="Screenshot from 2025-09-20 13-07-41" src="https://github.com/user-attachments/assets/7691bed2-5511-482c-91b7-0d154dab3e1b" />

**Below steps installs PDKs and Tools**
```bash
$ cd $HOME
$ git clone https://github.com/The-OpenROAD-Project/OpenLane
$ cd OpenLane
$ make
$ make test
```
<img width="1854" height="890" alt="Screenshot from 2025-09-20 13-12-15" src="https://github.com/user-attachments/assets/23f430b9-5557-4559-8cdd-baf37d16d108" />

<img width="1854" height="890" alt="Screenshot from 2025-09-20 13-12-23" src="https://github.com/user-attachments/assets/5198c264-ad70-43c9-ab67-eb7934466f63" />

