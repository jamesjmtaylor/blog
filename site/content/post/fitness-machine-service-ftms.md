---
title: FiTness Machine Service (FTMS)
date: '2020-03-01T08:50:00-08:00'
---
<img style="float: right; margin:0 0 1em 1em; width: 50%" src="/img/blog/ble.png"/> 

I recently created an Android app for debugging exercise equipment that advertise under the FiTness Machine Service (FTMS).  The Nautilus Quality Assurance team now uses the app extensively for claims testing of third-party open-standard equipment. As an added benefit I also learned a great deal about BLE, Android, and byte decoding while creating the app.

Under BLE (Bluetooth Low-Energy) GATT (General ATTributes Profile) there are "services" which act in the same way as protocols function under normal Bluetooth.  They identify various bit and byte contracts that bluetooth broadcasts must conform to. FTMS is one of these services.  All services including FTMS have nested characteristics.  These characteristics can either be READ properties (static) or NOTIFY properties (stream).  You must register for the latter by writing to the descriptor notification property of the characteristic. Android specifically will not wait for the BLE notification flag to be set after writing to the descriptor.  If you try to read immediately after the write you will get a non-deterministic response.  To avoid this you must write to the descriptor, wait for a notification that the descriptor has been written to, and then begin listening for characteristic changes.

Mostyou have signed bytes, you should test at the byte boundry.

<img style="float: right; margin: 1em 0 1em 0; width: 100%" src="/img/blog/bitbyte.png"/> 

The code below illustrates how this might be accomplished in Kotlin using experimental unsigned types.  Java, for reference, does not support unsigned types.

```
@ExperimentalUnsignedTypes
fun convertBytesAndFeaturesToCharacteristics(bytes: ByteArray, flags: List<INDOOR_BIKE_DATA_FLAGS>):Characteristic{
    var currentByteIndex = 2 //First two bytes are used for flags.
    val sb = StringBuilder()
    for (flag in flags){
        val dataEndByteIndex = currentByteIndex + flag.byteSize
        val dataBytes = bytes.copyOfRange(currentByteIndex, dataEndByteIndex)
        currentByteIndex = dataEndByteIndex
        val int = if (!flag.signed) dataBytes.asUByteArray().toInt() else dataBytes.toInt()
        val doubleValue = (int * flag.resolution * 10).roundToInt().toDouble() / 10.0 //Removes rounding errors
        sb.append("${flag.name}: $doubleValue ${flag.units} \n")
    }
    return Characteristic("Indoor Bike Data",sb.toString())
}
```

This function relies on the functions below to convert between bytearrays and bitsets:

```
fun ByteArray.toInt(): Int {
       val numBits = this.size * 8
       return BitSet.valueOf(this).toInt(numBits)
}fun BitSet.toInt(numBits : Int ): Int {
    var value = 0
    var isNegative = false
    for (i in 0 until this.length()) {
        if (i == numBits - 1) isNegative = this[numBits - 1] // handle two's compliment
        else value += if (this[i]) 1 shl i else 0
    }
    if (isNegative) return value * -1
    return value
}
```

Kotlin specifically also has the experimental unsigned ByteArray.  To convert this to an integer the following custom function is used:

```
@ExperimentalUnsignedTypes
fun UByteArray.toInt(): Int {
    var result : UInt = 0u
    for (i in this.indices){
        result = result or (this[i].toUInt() shl 8 * i)
    }
    return result.toInt()
}
```

Most BLE characteristics use Byte squishing.   A byte (8 bits) can be represented by two hexes (0-F). This is because each hex represents 4 bits, with 16 possible values.  A characteristic's supported features will be indicated in the first 2 bytes.  This is the "properties" value and is actually a set of true/false bit flags for supported features.  .The following "value" ByteArray represents the different possible values for each feature from the properties flags. Unsupported features will not be represented, even by 0 byte values.  This allows more features to be sent in a single transmission.

The maximum number of bytes transmitted in a single transmission is determined by the MTU (Max Transmission Unit).  The MTU is established between the client and the server as part of their pairing handshake.  Data that exceeds the number of bytes will need to be sent in a subsequent transmission. The larger the MTU, the longer that it takes to transmit, receive, and decode a single transmission.
