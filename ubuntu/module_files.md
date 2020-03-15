# Writing modulefile for `environment-modules` package

The **name** of _modulefile_ is the name of the module.

All _modulefile_ must start with **`#%Module<version>`**.

**`proc ModulesHelp{}`**: show when `module help <module name>` is called.

**`module-whatis`**: show when `module whatis <module name>` is called.

**`setenv <variable name> <variable value>`**: set an environment variable.

**`unsetenv <variable name> [<variable value>]`**: unset a environment variable and set that variable to the new value if provided.

**`getenv <variable name> [<variable value>]`**: get value of an environment variable, if the target variable is not yet defined, the default value is return (if provided, otherwise `_UNDEFINED_`).

**`append-path [-d C| --delim C] <variable name> <variable value>`** <br/>
**`prepend-path [-d C| --delim C] <variable name> <variable value>`** : append of prepend the current (C-)delimited environment variable with a value.

Examples:

**Cmake 3.16**
```tcl
#%Module3.16######################################
##
## cmake-3.16 modulefile
##
proc ModulesHelp { } {
    puts stderr "\tThis module load cmake version 3.16."
}

module-whatis   "cmake 3.16"

prepend-path    PATH /opt/bin
```