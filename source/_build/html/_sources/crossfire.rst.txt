====================
Crossfire Protocol
====================

简介
================

The TBS CROSSFIRE is a long range R/C link based on the newest RF technology,
capable of self-healing two-way communications, ultra-low latency and range
beyond comprehension.

协议格式
================

+--------+--------------+----------+-----------+-----------+-------------+--------+
| Stream | 0            |  1       |  2        | 3         | leng-1      | leng+1 |
+--------+--------------+----------+-----------+-----------+-------------+--------+
| DATA   | Device addre | leng     | Frame id  | Data[0]   | Data[n]     | Crc    |
+--------+--------------+----------+-----------+-----------+-------------+--------+

- Stream[0]:Device address

  - BROADCAST ADDRESS:  0x00
  - RADIO ADDRESS: 0xEA
  - **MODULE ADDRESS**: 0xEE

- Stream[1]: leng

  该长度为后面的Frame长度

- Stream[2]: Frame id

  - GPS_ID:                         0x02
  - CF_VARIO_ID:                    0x07
  - BATTERY_ID:                     0x08
  - LINK_ID:                        0x14
  - **CHANNELS_ID**:                    0x16
  - ATTITUDE_ID:                    0x1E
  - FLIGHT_MODE_ID:                 0x21
  - PING_DEVICES_ID:                0x28
  - DEVICE_INFO_ID:                 0x29
  - REQUEST_SETTINGS_ID:            0x2A

- Stream[3-(leng-1)]: 数据

- Stream[leng+1]: Crc8校验

TBS外置高频头传送通道数据
---------------------------------

以发射16个通道数据为列子，协议规定每个通道为11bit

**leng = 11bit*16/8=24byte**

+---------+---------+---------+---------+-----------+-----------+---------+
| Stream  | 0       | 1       | 2       | 3         | 24        | 25      |
+---------+---------+---------+---------+-----------+-----------+---------+
| DATA    | 0xEE    | 0x18    | 0x16    | Data[0]   | Data[n]   | Crc     |
+---------+---------+---------+---------+-----------+-----------+---------+



