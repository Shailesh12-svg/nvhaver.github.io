# Sniffing

## WiFi

1. Set up a monitoring interface on the netork interface you want to sniff:
`airmon-ng start wlan0`
2. Kill processes that could interfere using kill. These are pointed out by airmon-ng when setting up the monitor.
3. Dump the network trafic: `airodump-ng wlan0mon`

## Bluetooth

First start up the bluetooth daemon in the background: `bluetoothd &`
Next start using the bluetooth controller: `bluetoothctl`
In the controller, you can start scanning to see nearby devices by issuing `scan on`.

