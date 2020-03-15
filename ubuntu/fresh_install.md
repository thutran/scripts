# Fresh installation of Ubuntu 18.04 LTS

**Contents** <br/>
1. [Separating root `/` and data `/home`](#sep_root_home) <br/>
2. [Update, upgrade, install `build-essential`](#update_upgrade_build_ess) <br/>
3. [Other software](#sw) <br/>



## Separating `/` and `/home` partitions <a name="sep_root_home"/>
The idea is to have all system files in `/` and `/home` so that in case I screw up the system (again), I can just reinstall Ubuntu without losing data in `/home`.

Source: [https://askubuntu.com/questions/643441/how-to-create-a-separate-home-partition-after-installing-ubuntu-under-single-p](https://askubuntu.com/questions/643441/how-to-create-a-separate-home-partition-after-installing-ubuntu-under-single-p)

1. Creat a new partition <br/>
   If you don't have `gparted`, install it: `sudo apt install gparted`

   If you have an empty partition, great; you can just run `sudo gparted `.
   
   If your whole hard drive is currently in use, you won't be able to resize it. So, get a Live Boot Ubuntu USB, install and run`gparted`.

   Once in `gparted`, it's pretty straightforward to (resize and) make a new partition by right-click on the corresponding partition. Plesae make sure the format of the new partition is `ext4`. It might be useful to set partition names as well.

2. Copy files in old `/home` to the new partition<br/>
   Now, in the freshly installed Ubuntu, open _Disks_, right-click on the new partition, mount it, and get the mount point (something starting with `/media/`). Then, run:
   ```shell
   sudo cp -Rp /home/* /new-partition-mount-point
   ```

3. Get the new partition's UUID:
   UUID of the new partition can be found in the output of this command:
   ```shell
   sudo blkid
   ```
   Copy the **UUID** to a text file so that you can use it in the next step.

4. Add the partition info to `fstab`:
   Open `fstab` file:
   ```shell
   sudo gedit /etc/fstab
   ```
   Append the following line to the file: 
   ```shell
   UUID=New-partition-UUID    /home     ext4     nodev,nosuid     0     2
   ```

5. Move home directory and restart
   Now let's move the current home directory in the old partition to a backup location.
   ```shell
   cd /
   sudo mv /home /home_backup
   # create an empty folder which the new partition will be mounted to
   sudo mkdir /home
   ```
   Restart and check if everything is good. Delete the backup home by `sudo rm -rf /home_backup`.


## Update, upgrade, install `build-essential` <a name="update_upgrade_build_ess"/>
Run the following commands:
```shell
# update
sudo apt update
# upgrade
sudo apt upgrade
# autoremove
sudo apt autoremove
sudo apt autoclean
# install build-essential
sudo apt install build-essential
```


## Other software <a name="sw"/>
### Offline (via Terminal) dictionary
```shell
# client
sudo apt install dict
# local server
sudo apt install dictd
# databases
sudo apt install dict-gcide # Collective Intl Dictionary of English
sudo apt install dict-moby-thesaurus # Moby Thesaurus
sudo apt install dict-wn # Wordnet
# test
dict hello

```

### Environment modules
This is useful when you have multiple users or multiple versions of one software.

Source: [https://modules.readthedocs.io/en/stable/module.html](https://modules.readthedocs.io/en/stable/module.html)

Install (version `4.1.1`):
```shell
sudo apt install tcl
sudo apt install environmental-modules
```

Setup
```shell
# prepend sourcing of /skel/csh.modules or /skel/profile.modules to 
# ~/.cshrc, ~/.login, ~/.profile, ~/.bashrc
add.modules
```

You now need to initialize the package before using it. The initialization scripts are stored in `/usr/share/modules/init/`. Each shell type has its own initialization script, e.g. C shell uses `/usr/share/modules/init/csh`, sh shell uses `/usr/share/modules/init/csh`. I used Bourne Shell (i.e. sh) so I ran the following command:
```shell
. /usr/share/modules/init/sh
# if initialization is successful, you should be able to call `module`
module -h
```

This package uses _modulefile_ to change contents of `PATH` and `MANPATH` environment variables. More details and examples can be found in [module_files.md](module_files.md)


### Git
Source: [https://git-scm.com/download/linux](https://git-scm.com/download/linux)

Install the stable version (`2.17.1`) for Ubuntu:
```shell
sudo apt install git
git --version
```

If you need the latest stable upstream git:
```shell
# add git ppa
sudo add-apt-repository ppa:git-core/ppa
# update and install
sudo apt update
sudo apt install git
git --version
```

### Cmake
Source: [https://cmake.org/install/](https://cmake.org/install/)

Install the stable version (`3.10.2`) for Ubuntu:
```shell
sudo apt install cmake
cmake --version
```

If you need a different (possibly latest) version of Cmake, use their precompiled binary or download the source and build. Example buiding cmake `3.16.5`
```shell
# cmake requires `libssl-dev` so you need to install it first
sudo apt install libssl-dev
# obtain cmake source
cd ~/Downloads/software
wget https://github.com/Kitware/CMake/releases/download/v3.16.5/cmake-3.16.5.tar.gz
# extract
tar -xzvf cmake-3.16.5.tar.gz
# install
cd cmake-3.16.5
# prepare, the default prefix is `/usr/local`
./bootstrap --prefix=<your prefix> # e.g. `./bootstrap --prefix=/opt`
make -j 4 # build with 4 cores; the normal `make` should always work
sudo make install # or sudo make install
# test
cmake --version
```


### R and RStudio
Install the stable version of **R** for Ubuntu:
```shell
sudo apt install r-base
```

If you need the latest version from [CRAN](https://cran.r-project.org):
```shell
# add public key
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
# append the link to CRAN to sources list
sudo vi /etc/apt/sources.list
# add the following link to the end of the file 
# (navigate to the end, press key "i" to insert; press key "ESC" then ":wq!" to save and exit)
# deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/

# update and install R
sudo apt update
sudo apt install r-base 
# if you need to compile R packages from source at some points, install this as well
sudo apt install r-base-dev
```

**RStudio**
```shell
cd ~/Downloads/software
wget https://download1.rstudio.org/desktop/bionic/amd64/rstudio-1.2.5033-amd64.deb
# (optional) import public key of RStudio 
#gpg --keyserver keys.gnupg.net --recv-keys 3F32EE77E331692F
# (optional) validate key
#dpkg-sig --verify rstudio-1.2.5033-amd64.deb
# install, this will probably fail due to dependencies 
sudo dpkg -i rstudio-1.2.5033-amd64.deb
# install dependencies
```