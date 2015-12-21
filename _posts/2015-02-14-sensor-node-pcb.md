---
layout: post

edition: first

title: Sensor Node PCB
subtitle: Creating a maker friendly DIY IoT home automation solution

author:
  name: BITtailor
  twitter: bittailor_ch
  image: avatar.png
  bio: passion for the bits and bytes
---
[1]:http://smart-prototyping.com
[ATmega328]: http://www.atmel.com/devices/atmega328p.aspx
[NCP1402]: https://www.sparkfun.com/products/10967
[nRF24]: https://www.nordicsemi.com/eng/Products/2.4GHz-RF/nRF24L01P
[BatteryHolder]: http://www.mouser.com/ds/2/215/2468-285365.pdf
[DHT22]: http://www.adafruit.com/product/385
[BMP085]: https://www.adafruit.com/product/391

Before starting with the *soft* parts, I had to tackle the PCB design of the sensor nodes. This since my favorite PCB manufacturer was just about to close for Chinese New Year. I had to place the order before February 7, so that they get manufactured and shipped before the holiday. This week I just received an update:

> Your order has been updated to the following status:
> *Shipped*
>
> <small> 12.Feb - [Smart-Prototyping][1]</small>

<i class="fa fa-thumbs-o-up"></i> My sensor nodes PCB's are on their way.

<!-- more -->

Since I was kind of in a hurry, I used auto routing and did not do a lot of checking before ordering them. Let's hope they work and that they do not contain too many mistakes that couldn't be fixed by some extra wiring. Here are the top and bottom views:

Top:
![SensorNodeTop](/images/SensorNodeTop.png)
Bottom:
![SensorNodeBottom](/images/SensorNodeBottom.png)

The sensor nodes PCB connects:

  - the [ATmega328][ATmega328] with the 8MHz crystal oscillator
  - the [nRF24L01+][nRF24] transceiver module
  - a [DHT22][DHT22] temperature and humidity sensor
  - a [BMP085][BMP085] barometric pressure and altitude sensor
  - a [battery holder][BatteryHolder] for 2 AAA batteries
  - a [NCP1402][NCP1402] 3.3V Step-Up power converter
  - and a simple voltage divider to monitor the battery level



I will publish the design after I verified (and probably correted) it with the first batch of PCB's.

So now I move on to the *soft* parts.
