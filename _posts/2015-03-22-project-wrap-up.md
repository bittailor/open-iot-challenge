---
layout: post

edition: first

title: Project Wrap-Up
subtitle: Creating a maker friendly DIY IoT home automation solution

author:
  name: BITtailor
  twitter: bittailor_ch
  image: avatar.png
  bio: passion for the bits and bytes
---
[OpenIoT]: http://iot.eclipse.org/open-iot-challenge/
[ATmega328]: http://www.atmel.com/devices/atmega328p.aspx
[NCP1402]: https://www.sparkfun.com/products/10967
[nRF24]: https://www.nordicsemi.com/eng/Products/2.4GHz-RF/nRF24L01P
[BatteryHolder]: http://www.mouser.com/ds/2/215/2468-285365.pdf
[DHT22]: http://www.adafruit.com/product/385
[BMP085]: https://www.adafruit.com/product/391
[DHT]: http://playground.arduino.cc//Main/DHTLib
[uA]: http://www.rocketscream.com/blog/2011/07/04/lightweight-low-power-arduino-library/
[BtMqttSn]: https://github.com/bittailor/BtMqttSn#btmqttsn
[Arduino]: http://arduino.cc/
[ArduinoUno]: http://arduino.cc/en/Main/ArduinoBoardUno
[Mosquitto]: http://mosquitto.org/
[MQTT-SN]: http://mqtt.org/new/wp-content/uploads/2009/06/MQTT-SN_spec_v1.2.pdf
[RPi]: http://www.raspberrypi.org/
[Kura]: https://eclipse.org/kura/
[Node-RED]: http://nodered.org/


This is the final wrap-up post for my [Eclipse Open IoT Challenge][OpenIoT] project. I set out to build a maker friendly DIY IoT home automation solution, driven by my dissatisfaction with the humidifier control plugs I owned.

So as a proof of concept for my maker friendly DIY IoT home automation solution I built a simple *IoT air humidifier*:

<!-- more -->

<iframe width="560" height="315" src="https://www.youtube.com/embed/9De5VZFZXVg" frameborder="0" allowfullscreen></iframe>

## Overview

![IoTOverviewSolution](/images/IoTOverviewSolution.png)

This shows the overview of what I built which is nearly the same picture as in my [project idea post]({% post_url 2015-01-25-project-idea %}). I did not manage to build and setup everything I had in mind. Due to lack of time I skipped the connection to the various cloud services but I think it is simple to set them up trough configuring bridges on the [Mosquitto][Mosquitto] broker or creating some forwarding flows in [Node-RED][Node-RED].

All the source code for my project can be found on [<i class="fa fa-github fa-2x"></i>](https://github.com/bittailor/BtIot)

But now to the details of what I built:

## The sensor node

### Hardware

![SensorNodeFront](/images/SensorNodeFront.png) ![SensorNodeRear](/images/SensorNodeRear.png)

For the sensor node I created its own PCB that connects:

- the [ATmega328][ATmega328] with the 8MHz crystal oscillator
- the [nRF24L01+][nRF24] transceiver module
- a [DHT22][DHT22] temperature and humidity sensor
- a [BMP085][BMP085] barometric pressure and altitude sensor
- a [battery holder][BatteryHolder] for 2 AAA batteries
- a [NCP1402][NCP1402] 3.3V Step-Up power converter
- and a simple voltage divider to monitor the battery level

### Software
The sensor node software is built with the [Arduino][Arduino] programming platform. Besides the Arduino UNO core it uses the following libarries:

- [DHTLib][DHT]  to read the [DHT22][DHT22] temperature and humidity sensor.
- [uA][uA] a low power library for Arduino to enable sleeping on the sensor node.
- [BtMqttSn][BtMqttSn] my library for a MQTT-SN client communicating over a nRF24L01+ Transceiver.

The code of the sensor node is [here](https://github.com/bittailor/BtIot/blob/master/MqttSnSensorsNode/MqttSnSensorsNode.ino). It publishes regularly the temperature and humidity with [MQTT-SN][MQTT-SN] over the wireless sensor network.

## The actor node

### Hardware

![ActorNode](/images/ActorNode.png)

The actor node is currently just built as a prototype using a relay connected to an [Arduino Uno][ArduinoUno] board to switch a power line. The [nRF24L01+][nRF24] is used to communicate over the wireless sensor network.

### Software

Also the sensor node software is built with the [Arduino][Arduino] programming platform. The code of the actor node is [here](https://github.com/bittailor/BtIot/blob/master/MqttSnActorNode/MqttSnActorNode.ino). It uses [MQTT-SN][MQTT-SN] over the wireless sensor network to subscribes to a topic to control the relay.

## The gateway

### Hardware

![GatewayImage](/images/GatewayImage.jpg)

The gateway is built with a [Raspberry Pi][RPi]. The [nRF24L01+][nRF24] transceiver module is connected trough SPI.

### Software

There are three main services on the gateway:

#### The MQTT-SN gateway
This is the application I built with the [Kura][Kura] framework. It *gateways* the MQTT-SN messages of the wireless sensor network to MQTT messages forwarded to the MQTT broker. The gateway is made up of three OSGI bundles:

- **[ch.bittailor.iot.core](https://github.com/bittailor/BtIot/tree/master/ch.bittailor.iot.core)** Contains the implementation of the [nRF24L01+][nRF24] based wireless sensor network and the MQTT-SN gateway using the wireless sensor network.
- **[ch.bittailor.iot.wsn](https://github.com/bittailor/BtIot/tree/master/ch.bittailor.iot.wsn)** Provides the wireless sensor network packet socket as OSGI service.
- **[ch.bittailor.iot.mqttsn](https://github.com/bittailor/BtIot/tree/master/ch.bittailor.iot.mqttsn)** Provides the MQTT-SN gateway as OSGI service.

I planned to build a transparent gateway but since the Kura [CloudClient](http://download.eclipse.org/kura/releases/1.1.0/docs/apidocs/org/eclipse/kura/cloud/CloudClient.html) already provides an aggregating MQTT service it is an aggregating gateway now.

The gateway currently supports:

- Publish and subscribe with QoS 0.
- Sleeping clients.

It does not yet provide:

- QoS 1 und QoS 2.
- The search gateway feature, since the wireless sensor network does not yet provide a broadcasting.
- The last will feature.

Furhter the wireless sensor network limits the maximum length of a MQTT-SN message to 29 octets.

#### The MQTT broker
Here I just installed the [Mosquitto][Mosquitto] broker.

#### Node-RED
Installed node.js on the [Raspberry Pi][RPi] and then [Node-RED][Node-RED] via [npm](https://www.npmjs.com/).

## The wireless sensor network
The wireless sensor network is built with the [nRF24L01+][nRF24] transceiver modules. The transceiver provides a link layer that features automatic acknowledgement and retransmissions of packets. On top of this a  tree based network layer, where each node has one parent and five child nodes, is implemented on the nodes and on the gateway. No fragmentation and reassembly is implemented in the network layers. Currently broadcasting and encryption are not implemented yet.


## Lessons learned and conclusion
As always I completely underestimated the time I need to implement my ideas. I spent quite some time on understanding or fighting with Tycho and the hole build chain while working with Kura. It was nice that the Kura forum helped me with the (sometimes strange) questions I asked. I could even give a little bit back to the Kura community by my [pull request](https://github.com/eclipse/kura/pull/30) that got meregd to Kura and fixed the SPI jdk.dio.policy.

I think the Kura framework provides a good platform to implement the MQTT-SN gateway. My MQTT-SN gateway is still in a prototype stage and missing some features. It is also too tightly coupled to my wireless sensor network implementation. I think this coupling could be removed and generalized so that other networks like UDP or ZigBee could be plugged in.

I choose to implement my own wireless sensor network based on the [nRF24L01+][nRF24] transceiver modules because they are very inexpensive, are low power and I had a lot of them laying around. I'm not sure that it is the right way to continue, since it puts quite a burden on the sensor nodes to implement the network layer and it is still lacking some important features like encryption, broadcasting, fragmentation and reassembly.

I hope somebody enjoyed reading my posts. I for sure enjoyed working on my [Eclipse Open IoT Challenge][OpenIoT] project and I'm still eager to further *play* with IoT.
