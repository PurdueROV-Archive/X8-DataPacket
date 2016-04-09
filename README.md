# X8-DataPacket
Repository for keeping track of data packets needed.

Battlestation -> Main Micro Packet
----------------------------------
Byte # |   Description  | Type
-------|----------------|-------------------
  00   |     Header     | 0x12 - 18 decimal
  01   |     Control    | 0x01 == Control
  02   |       X-1      | Signed int (2 bytes), High order
  03   |       X-2      | Signed int (2 bytes), Low  order
  04   |       Y-1      | Signed int (2 bytes), High order
  05   |       Y-2      | Signed int (2 bytes), Low  order
  06   |       Z-1      | Signed int (2 bytes), High order
  07   |       Z-2      | Signed int (2 bytes), Low  order
  08   |     Roll-1     | Signed int (2 bytes), High order
  09   |     Roll-2     | Signed int (2 bytes), Low  order
  10   |     Pitch-1    | Signed int (2 bytes), High order
  11   |     Pitch-2    | Signed int (2 bytes), Low  order
  12   |      Yaw-1     | Signed int (2 bytes), High order
  13   |      Yaw-2     | Signed int (2 bytes), Low  order
  14   |   Camera Servo | 1 byte, Servo Angle + 90 degrees
  15   |    Solenoids   | 1 byte (2 bits per solonoid)
  16   | Hydaulics Pump | 1 byte (0 to 255 PWM Val)  
  17   |     LEDs       | 1 byte (0 to 255 PWM Val)  
  18   |   Thrusters    | 1 byte - Controls thruster's on status
  19   |  PID Control   | 1 byte - bool for pid / manual control
  20   | PID Tuning A-1 | Signed int (2 bytes), High order
  21   | PID Tuning A-2 | Signed int (2 bytes), Low  order
  22   | PID Tuning B-1 | Signed int (2 bytes), High order
  23   | PID Tuning B-2 | Signed int (2 bytes), Low  order
  24   | PID Tuning C-1 | Signed int (2 bytes), High order
  25   | PID Tuning C-2 | Signed int (2 bytes), Low  order
  26   |  PID Pivot X   | Signed int (1 byte)
  27   |  PID Pivot Y   | Signed int (1 byte)
  28   |  PID Pivot Z   | Signed int (1 byte)
  29   |   CRC8 Check   | Use 0xD5 as polynomial
  30   |   Tail Byte    | 0x13 - 19 decimal  
  
Always set command to 0x01. In the future, there will be further options to program options are ask for certain data using this byte, but nothing is set yet.  

Main Micro -> Battlestation Packet
----------------------------------
Byte # |   Description   | Type
-------|-----------------|-------------------
  00   |      Header     |  0x12 - 18 decimal
  01   | Thruster Status |  1 bit per thruster
  02   |    Pressure-1   |  32-bit float
  03   |    Pressure-2   |  32-bit float
  04   |    Pressure-3   |  32-bit float
  05   |    Pressure-4   |  32-bit float
  06   |      Temp-1     |  32-bit float
  07   |      Temp-2     |  32-bit float
  08   |      Temp-3     |  32-bit float
  09   |      Temp-4     |  32-bit float
  10   |      IMU-Lx     |  32-bit float 
  11   |      IMU-Lx     |  32-bit float 
  12   |      IMU-Lx     |  32-bit float 
  13   |      IMU-Lx     |  32-bit float 
  14   |      IMU-Ly     |  32-bit float 
  15   |      IMU-Ly     |  32-bit float 
  16   |      IMU-Ly     |  32-bit float 
  17   |      IMU-Ly     |  32-bit float 
  18   |      IMU-Lz     |  32-bit float 
  19   |      IMU-Lz     |  32-bit float 
  20   |      IMU-Lz     |  32-bit float 
  21   |      IMU-Lz     |  32-bit float
  22   |      IMU-Rx     |  32-bit float
  23   |      IMU-Rx     |  32-bit float
  24   |      IMU-Rx     |  32-bit float
  25   |      IMU-Rx     |  32-bit float
  26   |      IMU-Ry     |  32-bit float
  27   |      IMU-Ry     |  32-bit float
  28   |      IMU-Ry     |  32-bit float
  29   |      IMU-Ry     |  32-bit float
  30   |      IMU-Rz     |  32-bit float
  31   |      IMU-Rz     |  32-bit float
  32   |      IMU-Rz     |  32-bit float
  33   |      IMU-Rz     |  32-bit float
  34   |    CRC8 Check   | Use 0xD5 as polynomial 
  35   |    Tail Byte    | 0x13 - 19 decimal  
  
  
C :: CRC-8 Checksum Code
------------------------
```c
//loops over array of bytes and returns checksum
//skips first and last two bytes (header, checksum, tail)
char crc8(char bytes[], int size) {
  char crc = 0;
  char val;
  char mix;
  for (int i = 1; i < size - 2; ++i) {
    val = bytes[i];
    for (int j = 8; j; --j) {
      mix = (crc ^ val) & 0x01;
      crc >>= 1;
      if (mix) {
        crc ^= 0xD5;
      }
      val >>= 1;
    }
  }
  return crc;
}
```
