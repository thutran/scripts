# C++
All things C++ in Ubuntu 18.04

## Cmake
Please refer to [fresh_install.md](fresh_install.md)

## Boost for C++
Source: [https://www.boost.org/doc/libs/1_72_0/more/getting_started/unix-variants.html](https://www.boost.org/doc/libs/1_72_0/more/getting_started/unix-variants.html)

Build all libraries in Boost:
```shell
cd ~/Downloads/software
wget https://dl.bintray.com/boostorg/release/1.72.0/source/boost_1_72_0.tar.bz2
# prepare checksum file
echo "59c9b274bc451cf91a9ba1dd2c7fdcaf5d60b1b3aa83f2c9fa143417cc660722 boost_1_72_0.tar.bz2" > boost_1_72_0.tar.bz2.sha
# check, this should return "boost_1_72_0.tar.bz2: OK"
sha256sum --check boost_1_72_0.tar.bz2.sha
# extract (skip verbose)
tar -xjf boost_1_72_0.tar.bz2
cd boost_1_72_0
# optional, show libraries that need building and installing
 ./bootstrap.sh --show-libraries
# configure, default prefix is /usr/local
./bootstrap --prefix=<your prefix> # e.g. `./bootstrap --prefix=/opt`
# install, some properties:
# build shared (dynamic linking) multithreaded libraries, all release type
# this will take some time...
sudo ./b2 install link=shared threading=multi variant=release
```

## fmt for C++
Source: [https://fmt.dev/latest/usage.html#building-the-library](https://fmt.dev/latest/usage.html#building-the-library)

```shell
cd ~/Downloads/software
wget https://github.com/fmtlib/fmt/releases/download/6.1.2/fmt-6.1.2.zip
unzip fmt-6.1.2.zip
cd fmt-6.1.2
mkdir build && cd build
cmake -DCMAKE_INSTALL_PREFIX=<your prefix> -DBUILD_SHARED_LIBS=TRUE .. # e.g. cmake -DCMAKE_INSTALL_PREFIX=/opt -DBUILD_SHARED_LIBS=TRUE ..
sudo make install
```
