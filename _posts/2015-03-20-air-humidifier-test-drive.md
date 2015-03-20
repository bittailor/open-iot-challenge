---
layout: post

title: IoT air humidifier test drive
subtitle: Creating a maker friendly DIY IoT home automation solution

author:
  name: BITtailor
  twitter: bittailor_ch
  image: avatar.png
  bio: passion for the bits and bytes
---
[Kura]: https://eclipse.org/kura/
[Hello World Example]: http://eclipse.github.io/kura/doc/hello-example.html
[Getting Started]: http://eclipse.github.io/kura/doc/kura-setup.html
[RPi]: http://www.raspberrypi.org/
[Raspberry Pi Quick Start]: http://eclipse.github.io/kura/doc/raspberry-pi-quick-start.html
[nRF24]: https://www.nordicsemi.com/eng/Products/2.4GHz-RF/nRF24L01P
[Tycho]: https://eclipse.org/tycho/
[OSGI]: http://www.osgi.org/Main/HomePage
[Java]: http://www.oracle.com/technetwork/java/index.html
[Maven]: http://maven.apache.org/
[Continuous Integration]: http://www.martinfowler.com/articles/continuousIntegration.html
[mockito]: http://mockito.org/


With the deadline coming close, I did a first test drive for a IoT air humidifier:

![TestDriveSetup1](/images/TestDriveSetup1.png)

<!-- more -->

The sensor node measures temperature and humidity and publishes them to the topics *Bittailor/PiOne/Node1/Sensors/Temperature*
and *Bittailor/PiOne/Node1/Sensors/Humidity*.

The actor node subscribes to the topic *Bittailor/PiOne/Node2/Actor/Switch* to control the LED which acts as Humidifier.

The MQTT-SN messages are sent over the wireless sensor network to the MQTT-SN gateway running on the gateway node. The MQTT-SN gateway I built with Kura forwards them as MQTT messages to the Mosquitto MQTT broker, also running on the gateway.

And finally Node-RED, also running on the gateway, is used to wire the control logic.

![NodeRedControlFlow](/images/NodeRedControlFlow.png)
![NodeRedControlFlow](/images/NodeRedControlCode.png)

So lets make some dry air and see if it works.

<iframe width="560" height="315" src="https://www.youtube.com/embed/lOs6_VtJEck" frameborder="0" allowfullscreen></iframe>
<br>
It worked <i class="fa fa-thumbs-o-up"></i>

The code for the nodes and the MQTT-SN gateway can be found on [<i class="fa fa-github fa-2x"></i>](https://github.com/bittailor/BtIot). It still needs some further development and clean up, but now I have to heat up my soldering iron and solder my sensor node PCBs.
