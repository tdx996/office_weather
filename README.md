# installation

0) download the [Raspbian](https://www.raspberrypi.org/downloads/) and install it on the microSD

1) install python libs
```
sudo apt-get install python-pip
sudo pip install librato-metrics
sudo pip install pyyaml
```

2) create `config.yaml` with libratro credentials:
```
user: tim.lossen@wooga.net
token: abc123...
prefix: office.floor3
```

3) fix socket permissions
```
sudo chmod a+rw /dev/hidraw0
```

4) run the script
```
./monitor.py /dev/hidraw0
```
