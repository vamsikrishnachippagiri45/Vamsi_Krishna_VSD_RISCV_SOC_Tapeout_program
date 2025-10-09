
# Timing Analysis using OpenSTA

## OpenSTA Setup

Installed dependencies for OpenSTA build:
```
sudo apt-get update
sudo apt-get install -y cmake gcc g++ tcl-dev tcl-tclreadline libeigen3-dev swig bison flex automake
```

CUDD library installed for STA:
```
git clone https://github.com/ivmai/cudd.git
cd cudd
./configure --prefix=/usr/local
make -j$(nproc)
sudo make install
```

OpenSTA Build:
```
cd ~/VLSI/OpenSTA
mkdir -p build
cd build
cmake ..
make -j$(nproc)
sudo make install
```

Check STA binary:
```
which sta
sta
```



