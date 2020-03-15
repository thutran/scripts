# Python
Ubuntu 18.04 is shipped with python `2.7` and `3.6`.

## pip (python 2.7)
```shell
sudo apt install python-pip
# from now on, we can call `python -m pip` to install python packages into ~/.local/bin
python -m pip install --upgrade pip
python -m pip --version
```

## pip3
```shell
sudo apt install python3-pip
# from now on, we can call `python3 -m pip` to install python packages into ~/.local/bin
python3 -m pip install --upgrade pip
python3 -m pip --version
```

## python 3.8
```shell
# this allow creating different python environment for different projects
sudo apt install virtualenv
# python 3.8
sudo apt install python3.8 python3.8-dev python3.8-dbg
```



## Juoyter notebook
```shell
# interactive python
python3 -m pip install ipython
# notebook would fail with the new tornado (> 5.1.1)
python3 -m pip install tornado==5.1.1
# jupyter notebook
python3 -m pip install notebook
```