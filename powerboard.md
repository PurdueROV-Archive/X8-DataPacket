# PowerBoard CAN Communication

Main Micro -> PowerBoard Micro (Packet 1)
-----------------------------------------
Byte # |   Description   | Type
-------|-----------------|------
  00   |   Packet Type   |  0x48 - 'H'
  01   |        T1       |	Signed int (1 byte)
  02   |        T2       |  Signed int (1 byte)
  03   |        T3       |	Signed int (1 byte)
  04   |        T4       |  Signed int (1 byte)
  05   |       Empty     |	Empty
  06   |       Empty     |  Empty
  07   |     Pump ESC    |  1 byte (0 to 255 PWM Val)
  
Main Micro -> PowerBoard Micro (Packet 2)
-----------------------------------------
Byte # |   Description   | Type
-------|-----------------|------
  00   |   Packet Type   |  0x56 - 'V'
  01   |        T5       |	Signed int (1 byte)
  02   |        T6       |  Signed int (1 byte)
  03   |        T7       |	Signed int (1 byte)
  04   |        T8       |  Signed int (1 byte)
  05   |       Empty     |	Empty
  06   |       Empty     |  Empty
  07   |       Empty     |  1 byte - bool for pid / manual control
