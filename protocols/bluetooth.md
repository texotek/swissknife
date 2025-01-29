# Bluetooth

## Controller Mode
The controller modes refer to different Bluetooth transport modes that can be set for Bluetooth controllers.
Based on the information provided, there are three possible values for the ControllerMode setting:

1. "dual": This is the default mode, which enables both BR/EDR (Basic Rate/Enhanced Data Rate) and LE (Low Energy) when supported by the hardware[4].

2. "bredr": This mode stands for "Basic Rate/Enhanced Data Rate," which is also known as classic Bluetooth[4].

3. "le": This mode stands for "Low Energy," which is a newer type of Bluetooth designed for low-power consumption[4].

The ControllerMode setting in the configuration file (/etc/bluetooth/main.conf) restricts all controllers to the specified transport mode.
Setting it to "bredr" can sometimes resolve connection issues with certain devices, particularly when using classic Bluetooth accessories[4].

Citations:
[1] https://docs.nefarius.at/projects/DsHidMini/v2/HID-Device-Modes-Explained/
[2] https://core-electronics.com.au/guides/8bitdo-controller-modes-setup/
[3] https://www.reddit.com/r/8bitdo/comments/10y77fm/ultimate_bluetooth_controller_xinputdinputswitch/
[4] https://unix.stackexchange.com/questions/736933/what-is-the-bluetooth-controllermode-in-etc-bluetooth-main-conf-what-is-bredr
[5] https://stadia.google.com/controller/
