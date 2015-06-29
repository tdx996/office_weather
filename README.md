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

5) run on startup

To get everything working on startup, need to add 2 crontabs, one for root
and the other for the pi user:

Roots:

```
SHELL=/bin/bash
* * * * * if [ $(find /dev/hidraw0 -perm a=rw | wc -l) -eq 0 ] ; then chmod a+rw /dev/hidraw0 ; fi
```

Pi:

```
SHELL=/bin/bash
* * * * * if ( ! pidof python ) ; then /usr/bin/python /home/pi/monitor.py /dev/hidraw0 > /dev/null 2>&1 ; fi
```

This assumes that only one python is running on the box...
