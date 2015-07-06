# what & why?

Measuring Co2 and Temperature at [Woogas office](http://www.wooga.com/jobs/office-tour/).

People are sensitive to high levels of Co2 or uncomfortably hot work environments, so we want to 
have some numbers. It turns out that our [office](https://metrics.librato.com/share/dashboards/l7pd2aia) does
vary in temperature and Co2 across the floors.

# requirements 

## hardware

1) [TFA-Dostmann AirControl Mini CO2 Messgerät](http://www.amazon.de/dp/B00TH3OW4Q) -- 80 euro

2) [Raspberry PI 2 Model B](http://www.amazon.de/dp/B00T2U7R7I) -- 40 euro

3) case, 5v power supply, microSD card

## software

1) [Librato](https://www.librato.com) account for posting the data to.

# installation

0) download [Raspbian](https://www.raspberrypi.org/downloads/) and install it on the microSD

1) install python libs
```
sudo apt-get install python-pip
sudo pip install librato-metrics
sudo pip install pyyaml
```

2) create `config.yaml` with [libratro](https://www.librato.com) credentials:
```
user: tim.lossen@wooga.net
token: abc123...
prefix: office.floor3
```

We use [librato](https://www.librato.com) to graph our weather, so you'll need to modify that if you using another service.

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

# credits

based on code by [henryk ploetz](https://hackaday.io/project/5301-reverse-engineering-a-low-cost-usb-co-monitor/log/17909-all-your-base-are-belong-to-us)

# license

[MIT](http://opensource.org/licenses/MIT)
