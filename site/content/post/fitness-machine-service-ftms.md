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

Test

```
@ExperimentalUnsignedTypesfun convertBytesAndFeaturesToCharacteristics(bytes: ByteArray, flags: List<INDOOR_BIKE_DATA_FLAGS>):Characteristic{    var currentByteIndex = 2 //First two bytes are used for flags.    val sb = StringBuilder()    for (flag in flags){        val dataEndByteIndex = currentByteIndex + flag.byteSize        val dataBytes = bytes.copyOfRange(currentByteIndex, dataEndByteIndex)        currentByteIndex = dataEndByteIndex        val int = if (!flag.signed) dataBytes.asUByteArray().toInt() else dataBytes.toInt()        val doubleValue = (int * flag.resolution * 10).roundToInt().toDouble() / 10.0 //Removes rounding errors        sb.append("${flag.name}: $doubleValue ${flag.units} \n")    }    return Characteristic("Indoor Bike Data",sb.toString())}
```

This relies on functions to convert between bytearrays and bitsets:

```
fun ByteArray.toInt(): Int {       val numBits = this.size * 8       return BitSet.valueOf(this).toInt(numBits)}fun BitSet.toInt(numBits : Int ): Int {    var value = 0    var isNegative = false    for (i in 0 until this.length()) {        if (i == numBits - 1) isNegative = this[numBits - 1] // handle two's compliment        else value += if (this[i]) 1 shl i else 0    }    if (isNegative) return value * -1    return value}
```



Kotlin specifically also has the experimental unsigned ByteArray.  To convert this to an integer the following custom function is used:

```
@ExperimentalUnsignedTypesfun UByteArray.toInt(): Int {    var result : UInt = 0u    for (i in this.indices){        result = result or (this[i].toUInt() shl 8 * i)    }    return result.toInt()}
```
