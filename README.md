# X8-DataPacket
Repository for keeping track of data packets needed.

Battlestation -> Main Micro Packet
--------------------------------------------
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
  14   |  Camera Servo  | Signed int (1 byte), Offset from 0
  15   |   Solenoids    | 1 byte (2 bits per solonoid)
  16   |   Thrusters    | 1 byte - Controls thruster's on status
  17   |  Stabilization | 1 byte - bool for pid / manual control
  18   | Rotation P Val | Signed int (2 bytes), High order
  19   | Rotation P Val | Signed int (2 bytes), Low  order
  20   | Rotation I Val | Signed int (2 bytes), High order
  21   | Rotation I Val | Signed int (2 bytes), Low  order
  22   | Location P Val | Signed int (2 bytes), High order
  23   | Location P Val | Signed int (2 bytes), Low  order
  24   | Location I Val | Signed int (2 bytes), High order
  25   | Location I Val | Signed int (2 bytes), Low  order
  26   |   CRC8 Check   | Use 0xD5 as polynomial
  27   |   Tail Byte    | 0x13 - 19 decimal   
  
Always set command to 0x01. In the future, there will be further options to program options are ask for certain data using this byte, but nothing is set yet.  
  
Stabilization Byte Acting Axis
------------------------
 Bit # |   Description
-------|----------------
  00   |       Rx
  01   |       Ry
  02   |       Rz
  03   |       Lx
  04   |       Ly
  05   |       Lz
  06   |     Unused
  07   |     Unused


Main Micro -> Battlestation Packet
---------------------------------------------
Byte # |   Description   | Type
-------|-----------------|-------------------
  00   |      Header     | 0x12 - 18 decimal
  01   |   Packet Type   | 0x01 == Sensor Data
  02   | Thruster Status | 1 bit per thruster
  03   |    Pressure-1   | 32-bit float
  04   |    Pressure-2   | 32-bit float
  05   |    Pressure-3   | 32-bit float
  06   |    Pressure-4   | 32-bit float
  07   |      Temp-1     | 32-bit float
  08   |      Temp-2     | 32-bit float
  09   |      Temp-3     | 32-bit float
  10   |      Temp-4     | 32-bit float
  11   |      IMU-Lx     | Signed int (2 bytes), High order
  12   |      IMU-Lx     | Signed int (2 bytes), Low  order
  13   |      IMU-Ly     | Signed int (2 bytes), High order
  14   |      IMU-Ly     | Signed int (2 bytes), Low  order
  15   |      IMU-Lz     | Signed int (2 bytes), High order
  16   |      IMU-Lz     | Signed int (2 bytes), Low  order
  17   |      IMU-Rx     | Signed int (2 bytes), High order
  18   |      IMU-Rx     | Signed int (2 bytes), Low  order
  19   |      IMU-Ry     | Signed int (2 bytes), High order
  20   |      IMU-Ry     | Signed int (2 bytes), Low  order
  21   |      IMU-Rz     | Signed int (2 bytes), High order
  22   |      IMU-Rz     | Signed int (2 bytes), Low  order
  23   |    Thruster 1   | Thruster Value, Offset from 0
  24   |    Thruster 2   | Thruster Value, Offset from 0
  25   |    Thruster 3   | Thruster Value, Offset from 0
  26   |    Thruster 4   | Thruster Value, Offset from 0
  27   |    Thruster 5   | Thruster Value, Offset from 0
  28   |    Thruster 6   | Thruster Value, Offset from 0
  29   |    Thruster 7   | Thruster Value, Offset from 0
  30   |    Thruster 8   | Thruster Value, Offset from 0
  31   |    CRC8 Check   | Use 0xD5 as polynomial 
  32   |    Tail Byte    | 0x13 - 19 decimal  
  
  
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
