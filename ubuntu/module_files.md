# Writing modulefile for `environment-modules` package
These files must be saved in `${MODULESHOME}/modulefiles` folder (e.g. `/usr/share/modules/modulefiles`).

Some details:

* The **name** of _modulefile_ is the name of the module.
* All _modulefile_ must start with **`#%Module<version>`**.
* **`proc ModulesHelp{}`**: show when `module help <module name>` is called.
* **`module-whatis`**: show when `module whatis <module name>` is called.
* **`setenv <variable name> <variable value>`**: set an environment variable.
* **`unsetenv <variable name> [<variable value>]`**: unset a environment variable and set that variable to the new value if provided.
* **`getenv <variable name> [<variable value>]`**: get value of an environment variable, if the target variable is not yet defined, the default value is return (if provided, otherwise `_UNDEFINED_`).
* **`append-path [-d C| --delim C] <variable name> <variable value>`** <br/>
  **`prepend-path [-d C| --delim C] <variable name> <variable value>`** : append of prepend the current (C-)delimited environment variable with a value.

Examples:

**Cmake 3.16.5**
```tcl
#%Module3.16.5######################################
##
## cmake-3.16.5 modulefile
##
proc ModulesHelp { } {
    puts stderr "\tThis module loads cmake version 3.16.5."
}

module-whatis   "cmake 3.16.5"

prepend-path    PATH /opt/bin
```

**Boost 1.72.0**
```tcl
#%Module1.72.0######################################
##
## boost-1.72.0 modulefile
##
proc ModulesHelp { } {
    puts stderr "\tThis module loads boost C++ libraries version 1.72.0."
}

module-whatis   "boost 1.72.0"

## hints for Cmake
setenv  BOOST_ROOT  /opt
setenv  BOOST_INCLUDEDIR    /opt/include
setenv  BOOST_LIBRARYDIR    /opt/lib

## prepend-path    PATH /opt/include
```

**fmt 6.1.2**
```tcl
#%Module6.1.2######################################
##
## fmt 6.1.2 modulefile
##
proc ModulesHelp { } {
    puts stderr "\tThis module loads fmt C++ library version 6.1.2."
}

module-whatis   "fmt 6.1.2"

# hints for Cmake
setenv  FMT_ROOT  /opt
setenv  FMT_INCLUDEDIR    /opt/include
setenv  FMT_LIBRARYDIR    /opt/lib

## prepend-path    PATH /opt/include
```