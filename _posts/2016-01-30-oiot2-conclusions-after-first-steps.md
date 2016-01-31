---
layout: post

edition: second  

title: Conclusions after my first steps
subtitle: "IoT-ing a mountain’s hut off-grid solar power system"
cover_image: mountain_3.jpg

author:
  name: BITtailor
  twitter: bittailor_ch
  image: avatar.png
  bio: passion for the bits and bytes
---
[GPRS Shield]: http://www.seeedstudio.com/wiki/GPRS_Shield_V3.0
[Adafruit FONA]: https://www.adafruit.com/product/1946
[Arduino UNO]:https://www.arduino.cc/en/Main/ArduinoBoardUno
[Adafruit Feather M0 Basic Proto]: https://www.adafruit.com/products/2772
[Arduino Zero]: https://www.arduino.cc/en/Main/ArduinoBoardZero

So after my [first steps]({% post_url 2016-01-14-oiot2-first-steps %}) I draw some conclusions on how to continue ...

<!-- more -->  

### GPRS connection

For me programming against the AT command interface of the [GPRS Shield] shield is difficult, I was able to implement a simple happy case for a TCP/IP connection but for a 24/7 unattended sensor device, that should just work, also catching all the other paths and particularly reconnecting after some issues is important. So I searched for libraries but did not find one that works well with my [GPRS Shield]. Most of the libraries I found used pins like **<tt>RESET</tt>** or **<tt>Status</tt>** which are not accessible on my [GPRS Shield].

<i class="fa fa-arrow-right fa-2x"></i> I decided to look for an other GPRS breakout. I chose for an [Adafruit FONA] since this includes a library with TCP/IP support out of the box and provides a small footprint.

### Controller MCU

I started with an [Arduino UNO], which was perfect because I always have one of them in my tinkering box and Arduino provides you lots of libraries. But already after my first steps I see that the limited 2K SRAM of the [Arduino UNO] could provide a challenge. Already my first sketch with GPRS, display and current sensor used 87% of the SRAM for global variables. So avoid to spend endless time with optimizing memory usage, I decided to look for an other controller MCU. 

<i class="fa fa-arrow-right fa-2x"></i> I decided to look for an other controller MCU to avoid to spend endless time with optimizing memory usage. I chose for and [Adafruit Feather M0 Basic Proto] which supports the Arduino ecosystem, uses like the [Arduino Zero] an ATSAMD21G18 ARM Cortex M0 processor with 16x the SRAM (32K) of an UNO and provides a small footprint which will help to but everything into a enclosing.

So it was time to use my Adafruit gift certificate to get the parts to continue with the next steps of my project. And a little more that a week later I received a parcel:

![Adafruit_1.jpg](/images/Adafruit_1.jpg)

Now my prototype for the solar monitor sensor device looks like this:

![proto_2.jpg](/images/proto_2.jpg) 
