# LoRaWAN Versions

![](../.gitbook/assets/artboard-copy-23.jpg)

Currently, [RouterV3](https://github.com/helium/routerv3) is LoRaWAN 1.0.2 compatible. We plan to make it 1.0.3, 1.0.4, and 1.1 compatible in the future.

## Routing & Authentication

Devices with arbitrary AppEUI and DevEUI can use the Helium Network only as long as they authenticate using Over-the-Air-Authentication \(OTAA\). After OTAA, a DevAddr is assigned which allows for routing on the Helium Network. Read more about this [here](longfi-routing.md).

## Roaming

Despite multiple Network Servers working on the LongFi Network \(as opposed to a traditional LoRaWAN network\), the DevAddr space is arbitrated such that roaming from one Helium hotspot to another provides seamless connection without requiring new Joins.

## Regional Parameters

{% hint style="info" %}
Currently Helium is only available in the United States. Additional regions will be available in Q2 2020.
{% endhint %}

### United States \(US915\)

Helium operates on sub-band 2 \(Channels 8-15, 903.9-905.3 MHz\). RouterV3 will send a MAC ADR command with the appropriate submask until confirmation from the device. These are sent during the receive windows after Data uplinks.

Generally, this will work well with stacks that implement the full LoRaWAN specification. That is to say, devices should send Join requests on all 64 channels.

They may, however, be biased towards a certain sub-band \(preferably 2\), such that their first Join attempt is on one of the 8 channels of that preferred sub-band.

After a successful Join, the devices should also be transmitting Data frames on all 64 channels; similarly, biasing towards subband 2 is preferable. This will allow them to eventually transmit and then receive the MAC ADR command on a Helium subband and thus receive the channel mask update.

Unfortunately, many devices do not use all 64 channels and will constrain themselves to a programmed sub-band. These devices will require a firmware update or configuration change to join the Helium network.

In the future, we plan to be 1.1 compatible and to thus implement CFList in the JoinResponse.

RouterV3 implements the typical LoRaWAN US915 receive windows.

On Join request:

* Receive Window 1 = 5 seconds
* Receive Window 2 = 6 seconds

On Data Confirmed/Unconfirmed:

* Receive Window 1 = 1 second
* Receive Window 2 = 2 seconds

In the future, RXDelay may be changed from 1 second in the Join-Accept, so make sure your devices LoRaWAN stack obeys the specification and parses this value from the Join-Accept.

### Europe \(EU868\)

Coming soon.

### Asia \(AS923\)

Coming soon.

## Adaptive Data Rate

Other than for configuring channel mask, Adaptive Data Rate \(ADR\) algorithms are not yet implemented on RouterV3. This means that the configuration of your device is responsible for configuring it’s own Spreading Factor \(SF\) and power output.

