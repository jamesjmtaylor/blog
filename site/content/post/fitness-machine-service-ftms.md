---
title: FiTness Machine Service (FTMS)
date: '2020-03-01T08:50:00-08:00'
---
<img style="float: right; margin:0 0 1em 1em; width: 50%" src="/img/blog/ble.png"/> 

Under BLE (Bluetooth Low-Energy) GATT (General ATTributes Profile) there are "services" which act in the same way as protocols function under normal Bluetooth.  They identify various bit and byte contracts that bluetooth broadcasts must conform to.

Android will not wait for BLE notification flags to be set.  If you read immediately after write you will get a garbage response.

Services have nested characteristics.  These characteristics can either be a READ property (static) or a NOTIFY property (stream).  You must register for the latter using the descriptor of the characteristic.

You can not connect a BLE device while scanning.  

The "properties" int value for a characteristic in Android is actually a set of true/false bit flags for supported features.

The "value" ByteArray of 8 Bytes repeats the different possible values for each feature from the properties flags.

A byte (8 bits) can be represented by two hexes (0-F). This is because each hex represents 4 bits, with 16 posible values.

Most BLE characteristics use Byte squishing.  A characteristic's supported features will be indicated in the first 2 bytes, and the following bytes will be the data of the supported features.  Unsupported features will not be represented, even by 0 byte values,, allowing more features to be sent.

MTU (Max Transmission Unit) estables the maximum number of bytes transmitted in a single transmission.  Data that exceeds the number of bytes will need to be sent in a subsequent transmission.

If you have signed bytes, you should test at the byte boundry.

<img style="float: right; margin: 1em 0 1em 0; width: 100%" src="/img/blog/bitbyte.png"/>
