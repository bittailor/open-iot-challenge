---
layout: post

title: Project idea details
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
[ArduinoUno]: http://arduino.cc/en/Main/ArduinoBoardUno
[RPi]: http://www.raspberrypi.org/
[Node-RED]: http://nodered.org/
[ThingSpeak]: https://thingspeak.com
[xively]: https://xively.com/
[ATmega328]: http://www.atmel.com/devices/atmega328p.aspx
[Kura]: https://eclipse.org/kura/
[Paho]: https://eclipse.org/paho/
[Mosquitto]: http://mosquitto.org/
[Raspbian]: http://raspbian.org/
[ArduinoLibraries]: http://arduino.cc/en/Reference/Libraries

Let's dive into some details after the [last post]({% post_url 2015-01-25-project-idea %}) gave an overview of my project idea.

<!-- more -->

![Overview](/images/project-overview.png)


## Sensor and Actor Nodes
All the sensor and actor nodes will be based on the [Arduino][Arduino] open-source physical computing platform. They will use a [nRF24L01+][nRF24] ultra low power 2.4GHz transceiver to communicate over the [wireless sensor area network](#SensorAreaNetwork). They will use [MQTT-SN][MQTT-SN] to publish the sensor data and subscribe to actor commands.

I will create two types of nodes:

+ **Battery powered sensor nodes:** They will be powered with two AAA batteries and they should run for more than six months without changing the batteries. I will use a [ATmega328][ATmega328] running at 8MHz and operated with 3.3V. They will periodically read all the sensors, publish the data to the gateway and then sleep till the next read period, utilizing the [MQTT-SN][MQTT-SN] support of sleeping clients. I will create a custom PCB for a sensor node with a temperature & humidity sensor and a barometric pressure sensor.

+ **Mains powered actor nodes:** Since they have no power constrains I will use the [ATmega328][ATmega328] running at 16MHz and operated with 5V. I will create an actor node with a relay that switches a mains socket.


## The Gateway Node
The gateway will be built with a [Raspberry Pi][RPi]. It will use a [nRF24L01+][nRF24] 2.4GHz transceiver to communicate over the [wireless sensor area network](#SensorAreaNetwork). There will be three main services on the gateway node:

+ A **MQTT-SN gateway** which *gateways* the MQTT-SN messages of the sensor area network to MQTT messages of the local area network. I will build a transparent MQTT-SN gateway with the help of the [Kura][Kura] framework and the [Paho][Paho] libraries.

+ A [Mosquitto][Mosquitto] instance as a local **MQTT Broker** instance.

+ A [Node-RED][Node-RED] instance used to:
  + Create the wiring and logic for the home automation.
  + Forward the sensor data to cloud services.
  + Create further API's.

## <a name="SensorAreaNetwork"></a>The wireless sensor area network
The wireless sensor area network will be built based on the [nRF24L01+][nRF24] ultra low power 2.4GHz transceivers. I will use them since the are very popular in the maker area, very inexpensive and [widely available](http://www.ebay.com/sch/i.html?_nkw=NRF24L01%2B). The transceiver provides a link layer that features automatic packet assembly and timing, automatic acknowledgement and retransmissions of packets. On top of this a simple tree based sensor area network layer will be implemented on the sensor and actor nodes and on the gateway node. No fragmentation and reassembly will be implemented in the sensor area network layers, therefore the maximum packet size is restricted and so is the the maximum length of a [MQTT-SN][MQTT-SN] message.

## Recap

### Hardware

+ [Raspberry Pi][RPi]
+ [ATmega328][ATmega328] ([Arduino UNO][ArduinoUno] based)
+ [nRF24L01+][nRF24] transivers
+ ...

### Software
+ [Kura][Kura] framework
+ [Paho][Paho] libraries
+ [Mosquitto][Mosquitto]
+ [Node-RED][Node-RED]
+ Various [Arduino libraries][ArduinoLibraries]
+ [Raspbian][Raspbian]
+ ...

### Protocols
+ [MQTT-SN][MQTT-SN]
+ [MQTT][MQTT]
+ ...


> enough about ideas ... let's go to work and have FUN
