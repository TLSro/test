# RN2903-Demo

Source code for the [Microchip RN2903 Module](https://www.beyondlogic.org/microchip-rn2903-lora-transceiver-breakout-board/).

![Microchip RN2903 LoRa Breakout Board](https://www.beyondlogic.org/wp-content/uploads/2018/09/Microchip-RN2903-LoRa-Breakout-3-678x381.png)

This example shows how to run your own code on the RN2903 LoRaWAN Module, eliminating the requirement for a 2nd microcontroller. This code interrogates a Sensirion [SHTC3 Temperature and Humidity Sensor](https://sensirion.com/products/catalog/SHTC3/) every 10 minutes and sends the data to a LoRaWAN Network - tested with The Things Network & Helium.

The RN2903 Module includes a:
* [Microchip PIC18LF46K22 MCU](https://www.microchip.com/stellent/groups/picmicro_sg/documents/devicedoc/cn547043.pdf)
* [Semtech SX1276 Radio Transceiver](https://semtech--c.na98.content.force.com/sfc/dist/version/download/?oid=00DE0000000JelG&ids=0682R000006TQEPQA4&d=%2Fa%2F2R0000001Rbr%2F6EfVZUorrpoKFfvaF_Fkpgp5kzjiNyiAbqcpqh9qSjE&operationContext=DELIVERY&asPdf=true&viewId=05H2R000002WGmXUAW&dpt=)
* [Microchip 24AA02E64T 2Kb I2C Serial EEPROM with Pre-Programmed EUI-64™ MAC ID](https://ww1.microchip.com/downloads/en/DeviceDoc/24AA02E48-24AA025E48-24AA02E64-24AA025E64-Data-Sheet-20002124H.pdf)

This is a demo project using the Microchip LoRaWAN Library Stack on the RN2903A Module. For more information, please visit:
* [RN2903: Using the LoRaWAN™ Library Plug-in for MPLAB® Code Configurator and customising for the AU915 Frequency Plan](https://www.beyondlogic.org/rn2903-using-the-lorawan-library-plug-in-for-mplab-code-configurator-and-customising-for-the-au915-frequency-plan/)

## Frequency Band

This code is set-up for the AU915 Frequency Plan. 

Channels 8 to 15 (Sub Band 2) are enabled for use with The Things Network or Helium. 

# March 2022 Update

While Microchip has never updated the LoRaWAN Library in MPLAB Code Configurator (MCC), recently they released the source code for the 'UART Modules' and this uses the same stack. We have copied/updated to the mature stack that is running on the RN2903 SA AU915 v1.0.3 modules. 
* https://github.com/MicrochipTech/RN2xx3_LORAWAN_FIRMWARE

Please note the project requires the XC8 V1.45 Cross Compiler. Using V2.x of the compiler appears to result in timing issues, i.e. the Receive Window 1 and 2 can be either increased four fold, or fails to operate at all. 

# The Things Network

Below is Javascript for the Payload Formatter for use on [The Things Network](https://www.thethingsnetwork.org/)

```
function decodeUplink(input) {
	var data = {};
	var temp_raw = (input.bytes[1] << 8) + input.bytes[0];
	var humd_raw = (input.bytes[3] << 8) + input.bytes[2];
	data.temp = -45 + 175 * (temp_raw / 65536);
	data.RH = 100 * (humd_raw / 65536);
  	return {
      	data: data,
  	};
}
```
# Helium Network

Example session operating on the Helium Network:

```
RN2903 Test Program
Beyondlogic.org
Channel 08 Enabled: 916800000Hz
Channel 09 Enabled: 917000000Hz
Channel 10 Enabled: 917200000Hz
Channel 11 Enabled: 917400000Hz
Channel 12 Enabled: 917600000Hz
Channel 13 Enabled: 917800000Hz
Channel 14 Enabled: 918000000Hz
Channel 15 Enabled: 918200000Hz
Channel 65 Enabled: 917500000Hz
Join Response Successful
Sent message
Packet Received, 0 bytes, status = 1
Sent message
Packet Received, 0 bytes, status = 1
Sent message
Packet Received, 0 bytes, status = 1
Sent message
Packet Received, 0 bytes, status = 1
```

Helium Console:

![Helium Device Console](https://beyondlogic.org/i/Helium_EventLog.png)
