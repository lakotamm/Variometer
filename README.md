# Variometer
This is repo of a variometer for Hang gliders measuring 3D vector of the wind. 

The idea is to use IMU + barometer + a magnetometer to determine orientation and vertical movement of the hang glider, GPS to determine movement against ground and a 5 port pitot tube to measure vector of the air movement. 

Altogether, this should give us 3D vector of the wind, with a specifically high resolution and fast update rate of the vertical vector. In this way we will be able to indicate more precisely to the glider pilot where thermals are occurring.

Miro board:

[https://miro.com/app/board/uXjVEePOPy4=/](https://miro.com/app/board/uXjVEePOPy4=/?share_link_id=711769667885)


# Bluetooth HC-42 module setup:
Set baud rate to 115200

"AT+UART=115200"

Increase broadcast interval to 20 ms

"AT+AINT=20"

Disable sleep to improve latency

"AT+PM=0"

# Ublox DAN-10N GPS module setup in PyGPSClient

1. Increase the Baud Rate to 115,200

Madflight can auto-detect baud rates if configured to do so, but a fixed 115,200 baud is the standard for high-refresh drone GPS units.
• Go to the CFG-UART1 group.
• Change CFG-UART1-BAUDRATE to 115200.
• Note: Once you hit send, you will lose connection in PyGPSClient until you manually switch PyGPSClient's connection speed to 115,200.

2. Increase the Navigation Rate to 10Hz
• Go to the CFG-RATE group.
• Change CFG-RATE-MEAS (Measurement Period) to 100 ms (which corresponds to 10Hz).
• Ensure CFG-RATE-NAV remains set to 1 (one navigation solution per measurement cycle).
• Check RAM, BBR, and Flash layers so the override survives power cycles.
• Click Send.

4.Enable UBX Binary Positional Data

Madflight natively handles UBLOX binary protocol. To ensure smooth flight controller reading:

• Look up group CFG_MSGOUT_UBX
• Inside that group, look for the specific key named MSGOUT-UBX_NAV_PVT_UART1 (or CFG_MSGOUT_UBX_NAV_PVT_UART1).
• Change its value to 1.
• Check the RAM, BBR, and Flash layers at the bottom.
• Click Send.


5. Turn off NMEA on UART1

• In PyGPSClient's UBX Config tab, navigate to the CFG_UART1OUTPROT group (or search for it in the configuration database).
• Locate the specific key: CFG_UART1OUTPROT-NMEA
• Change its value to 0 (False / Disabled).
• Select RAM, BBR, and Flash, then click Send.

6. Enable L5 

• In the UBX Config tab, search or browse for the CFG-SIGNAL configuration group.
• Locate the key: CFG-SIGNAL-GPS_L5_HEALTH.
• Change its value to 1 (which corresponds to "Override L5 health with the L1 health of the same satellite").
• Check RAM, BBR, and Flash layers so the override survives power cycles.

5. Save to Flash and BBR

Always make sure these changes survive your drone's battery swaps.
• Select the RAM, BBR, and Flash destination layers when clicking Send in PyGPSClient.
