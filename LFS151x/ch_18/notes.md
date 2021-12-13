# Chapter 18: Internet of Things (IoT)

## Tasks
- [x] None

## Introduction

**Internet of Things** (**IoT**) is made of two terms *Internet* and *Things*. By *Internet*, we mean that the *Things* are connected. By *Things*, however, we don't just mean the end devices, but also people and interconnected systems. Collecting, processing, analyzing data, and then taking action accordingly also comes under IoT.

According to [Wikipedia](https://en.wikipedia.org/wiki/Internet_of_things):

>”The Internet of Things (IoT) is the network of physical devices, vehicles, home appliances, and other items embedded with electronics, software, sensors, actuators, and connectivity which enables these things to connect and exchange data, creating opportunities for more direct integration of the physical world into computer-based systems, resulting in efficiency improvements, economic benefits, and reduced human exertions."

We now have sensors in our watches, shoes, cars, and appliances, and these IoT-enabled devices are now becoming part of our day-to-day lives. For example, we now have IoT-enabled refrigerators, that track an inventory of goods available in the refrigerator, and order them in advance, depending on our consumption. IoT is making our air travel safer, reducing cost by automation, and helping us remotely monitor other devices, goods, and locations. There are many use cases like these, which we will explore in the next section.

Looking at the trends, in the next few years, all of the devices would be IoT-enabled by default.

## Network for IoT

As there are so many IoT devices, the IPv4 network range cannot accommodate them. We need to use the IPv6 network to address those devices. These devices can be installed at remote locations; they can be continuous in motion, etc. Other than the traditional network, community and companies have built different network standards to fulfill these unique needs. Let's list some of them, based on the network ranges:

**Short-range Wireless**
* [Bluetooth mesh networking](https://en.wikipedia.org/wiki/Bluetooth_mesh_networking)
* [Light fidelity (Li-Fi)](https://en.wikipedia.org/wiki/Li-Fi)
* [Radio-frequency identification (RFID)](https://en.wikipedia.org/wiki/Radio-frequency_identification)
* [Near-field communication (NFC)](https://en.wikipedia.org/wiki/Near-field_communication)
* [Wi-Fi](https://en.wikipedia.org/wiki/Wi-Fi)
* [Zigbee](https://en.wikipedia.org/wiki/Zigbee)
**Medium-range Wireless**
* [Wi-Fi HaLow](https://en.wikipedia.org/wiki/IEEE_802.11ah)
* [LTE Advanced](https://en.wikipedia.org/wiki/LTE_Advanced)
**Long-range Wireless**
* [Low-Power Wide-Area Network (LPWAN)](https://en.wikipedia.org/wiki/LPWAN)
* [Very small aperture terminal (VSAT)](https://en.wikipedia.org/wiki/Very-small-aperture_terminal)
**Wired**
* [Ethernet](https://en.wikipedia.org/wiki/Ethernet)
* [Multimedia over Coax Alliance (MoCA)](https://en.wikipedia.org/wiki/Multimedia_over_Coax_Alliance)
* [Power-line communication (PLC)](https://en.wikipedia.org/wiki/Power-line_communication)

## Computing for IoT

Similar to networking, computing has evolved for IoT. We would need to handle situations like the following:

* Latency between the actual device and the datacenter (cloud) where we store the actual data.
* Not sending unnecessary data, like frequent keep-alive messages from the devices/sensors to the backend datacenter.

To handle such situations, **Distributed Computing Architecture** has evolved to include [edge](https://en.wikipedia.org/wiki/Edge_computing) and [fog](https://en.wikipedia.org/wiki/Fog_computing) computing. By using edge and fog computing we bring computing applications, data, and services away from some central nodes (datacenter) to the other logical extreme (the *edge*) of the Internet.
