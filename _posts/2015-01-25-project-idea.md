---
layout: post

title: My project idea for the Eclipse Open IoT Challenge
subtitle: Creating a maker friendly DIY IoT home automation solution

author:
  name: BITtailor
  twitter: bittailor_ch
  image: avatar.png
  bio: passion for the bits and bytes
---

[OpenIoT]: http://iot.eclipse.org/open-iot-challenge/
[MQTT]: http://mqtt.org
[MQTT-SN]: http://mqtt.org/new/wp-content/uploads/2009/06/MQTT-SN_spec_v1.2.pdf
[nRF24]: https://www.nordicsemi.com/eng/Products/2.4GHz-RF/nRF24L01P
[Arduino]: http://arduino.cc/
[RPi]: http://www.raspberrypi.org/
[Node-RED]: http://nodered.org/
[ThingSpeak]: https://thingspeak.com
[Xively]: https://xively.com/

My idea for the [Eclipse Open IoT Challenge][OpenIoT] came from my need to measure the humidity in my rooms, to track it and also control air humidifiers based on the humidity. I used some humidifier control plugs, but they were just not *smart* enough. Since I like  electronic tinkering and have some interrest in IoT, why not build a DIY solution. For the [Eclipse Open IoT Challenge][OpenIoT] this transformed into the more generic idea to build a maker friendly DIY IoT home automation solution.

<!-- more -->


## Overview

The plan is to create [Arduino][Arduino] based sensor and actor nodes that use [MQTT-SN][MQTT-SN] to communicate over a [nRF24L01+][nRF24] based wireless sensor area network. They send and receive their messages via a [MQTT-SN][MQTT-SN] gateway to a [MQTT] broker. The gateway and the broker run on a [Raspberry Pi][RPi] gateway node. Also on the [Raspberry Pi][RPi] runs an instance of [Node-RED][Node-RED] which is connected to the [MQTT] broker and is used to wire the things and make them *smart*. [Node-RED][Node-RED] will also be used to forward the sensor data to cloud services like [ThingSpeak][ThingSpeak] or [Xively][Xively] to store, analyse and present the data.

![Overview](/images/project-overview.png)

## Outlook

We got new snow today

![SnowToday](/images/snow-today.jpg)

so IoT had to wait for the evening and the details of

 + the wireless sensor area network
 + the sensor and actor nodes
 + the gateway
 + the used software stacks
 + the hardware details
 + ...

have to wait for the next post.
