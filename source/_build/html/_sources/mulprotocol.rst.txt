==============
高频头
==============

------------
简介
------------




1.1 Serial mode
-----------------

1.1.1
=========

1.2 serial Protocol
=====================

比特率 100000

1.2.1 Stream[0]
---------------
* 0x55	Sub_Protocol values are 0..31	Stream contains channels

* 0x54	Sub_Protocol values are 32..63	Stream contains channels

* 0x57	sub_protocol values are 0..31	Stream contains failsafe

* 0x56	sub_protocol values are 32..63	Stream contains failsafe

1.2.2 Stream[1]
---------------

sub_protocol is 0..31 (bits 0..4)


+--------+--------+---------+---------+---------+---------+--------+--------+
| 7      |     6  |      5  |    4    |   3     |  2      |   1    | 0      |
+--------+--------+---------+---------+---------+---------+--------+--------+
| BindBit| AutoBB |  RangC  |  sub_protocol                                 |
+--------+--------+---------+---------+---------+---------+--------+--------+

* 位 7: 绑定标志位(BindBit)

  * 0 No
  * 1 Yes
* 位 6: 自动绑定标志位(AutoBindBit)

  * 0 No
  * 1 Yes
* 位 5 : (RangeCheck)

  * 0 No
  * 1 Yes
* 位 4:0 : 协议序号(sub_protocol)

  * 当Byte 0为0x54，该直加上32才为对应的协议序号


1.2.3 Stream[2]

+--------+--------+---------+---------+---------+---------+--------+--------+
| 7      |     6  |      5  |    4    |   3     |  2      |   1    | 0      |
+--------+--------+---------+---------+---------+---------+--------+--------+
| PowerV |         Type               |   RxNum                             |
+--------+--------+---------+---------+---------+---------+--------+--------+


* 位 7： Power value

  * 0 : low
  * 1 : hight

- 位 6:4 : Type Value

  - sub_protocol==Flysky

    - 0 : flasky
    - 1 : V9x9
    - 2 : V6x6
    - 3 : V912
    - 4 : CX20

  - sub_protocol==FRSKYX

    - 0 : CH_16
    - 1 : CH_8
    - 2 : EU_16
    - 3 :EU_8

  - sub_protocol==FRSKYX_RX

    - 0 : FCC
    - 1 :	LBT

  - sub_protocol==Hubsan

    - 0 : H107
    - 1 : H301
    - 2 : H501

  - sub_protocol==Hisky

    - 0 : Hisky
    - 1 : HK310

  - sub_protocol==DSM

    - 0 : DSM2_22
    - 1 : DSM2_11
    - 2 : DSMX_22
    - 3 : DSMX_11
    - 4 : DSM_AUTO

  - sub_protocol==YD717

    - 0 : YD717
    - 1 : SKYWLKR
    - 2 : SYMAX4
    - 3 : XINXUN
    - 4 : NIHUI

  - sub_protocol==KN

    - 0 : WLTOYS
    - 1 : FEILUN

  - sub_protocol--SYMAX

    - 0 : SYMAX
    - 1 : SYMAX5C

  - sub_protocol==CX10

    - 0 : CX10_GREEN
    - 1 : CX10_BLUE
    - 2 : DM007
    - 3 : ----
    - 4 : JC3015_1
    - 5 : JC3015_2
    - 6 : MK33041

  - sub_protocol==Q2X2

    - 0 : Q222
    - 1 : Q242
    - 3 : Q282

  - sub_protocol==CG023

    - 0 : CG023
    - 1 : YD829

  - sub_protocol==BAYANG

    - 0 : BAYANG
    - 1 : H8S3D
    - 2 : X16_AH
    - 3 : IRDRONE
    - 4 : DHD_D4

  - sub_protocol==MT99XX

    - 0 : MT99
    - 1 : H7
    - 2 : YZ
    - 3 : LS
    - 4 : FY805

  - sub_protocol==MJXQ

    - 0 : WLH08
    - 1 : X600
    - 2 : X800
    - 3 : H26D
    - 4 : E010
    - 5 : H26WH
    - 6 : PHOENIX

  - sub_protocol==HONTAI

    - 0 : HONTAI
    - 1 : JJRCX1
    - 2 : X5C1
    - 3 : FQ777_951

  - sub_protocol==AFHDS2A

    - 0 : PWM_IBUS
    - 1 : PPM_IBUS
    - 2 : PWM_SBUS
    - 3 : PPM_SBUS

  - sub_protocol==V2X2

    - 0 : V2X2
    - 1 : JXD506

  - sub_protocol==FY326

    - 0 : FY326
    - 1 : FY319

  - sub_protocol==WK2x01

    - 0 : WK2801
    - 1 : WK2401
    - 2 : W6_5_1
    - 3 : W6_6_1
    - 4 : W6_HEL
    - 5 : W6_HEL_I

  - sub_protocol==Q303

    - 0 : Q303
    - 1 : CX35
    - 2 : CX10D
    - 3 : CX10WD

  - sub_protocol==CABELL

    - 0 : CABELL_V3
    - 1 : CABELL_V3_TELEMETRY
    - 6 : CABELL_SET_FAIL_SAFE
    - 7 : CABELL_UNBIND

  - sub_protocol==H8_3D

    - 0 : H8_3D
    - 1 : H20H
    - 2 : H20MINI
    - 3 : H30MINI

  - sub_protocol==CORONA

    - 0 : COR_V1
    - 1 : COR_V2
    - 2 : FD_V3

  - sub_protocol==HITEC

    - 0 : OPT_FW
    - 1 : OPT_HUB
    - 2 : MINIMA

  - sub_protocol==SLT

    - 0 : SLT_V1
    - 1 : SLT_V2
    - 2 : Q100
    - 3 : Q200
    - 4 : MR100		4

  - sub_protocol==E01X

    - 0 : E012
    - 1 : E015
    - 2 : E016H

  - sub_protocol==GD00X

    - 0 : GD_V1
    - 1 : GD_V2

  - sub_protocol==REDPINE

    - 0 : RED_FAST
    - 1 : RED_SLOW

  - sub_protocol==TRAXXAS

    - 0 : RX6519

- 位 3:0 : RxNum



1.2.3 : Stream[3]
-----------------

option protocol,values is -128..127


1.2.4 : Stream[4] 到 Steam[25]
-------------------------------

16 Channels on 11 bits (0..2047)，16个通道，每个通道占用11位，值得范围为0~2047

16 Channels * 11Bits = 22 bytes

=========  =======
通道直      百分比
=========  =======
  0         -125%
204         -100%
1024        +0%%
1843        +100%

2047        +125%
=========  =======


Values are concatenated to fit in 22 bytes like in SBUS protocol.
Failsafe values have exactly the same range/values than normal
channels except the extremes where
0=no pulse, 2047=hold. If failsafe is not set or RX then failsafe packets should not be sent.

1.3 遥测协议(Telemetry protocol)
================================

+-------+--------+--------+--------+--------+--------+--------+-------------+
| Stream| 0      | 1      | 2      |  3     | 4      | 4+n    | 3+leng      |
+-------+--------+--------+--------+--------+--------+--------+-------------+
| DATA  | 0x4D   |  0x50  | Type   |  leng  | data[0]| data[n]| data[leng-1]|
+-------+--------+--------+--------+--------+--------+--------+-------------+

- Stream[0]:head
  该数值固定为0x4D

- Stream[1]:head
  该数值固定为0x50

- Stream[2]:Type

  - 0x01 : Multimodule Status
  - 0x02 : Frksy S.port telemetry(Frsky S.port 遥测的回传数据)
  - 0x03 : Frsky Hub telemetry
  - 0x04 : Spektrum telemetry data
  - 0x05 : DSM bind data
  - 0x06 : Flysky AFHDS2 telemetry data
  - 0x0A : Hitec telemetry data
  - 0x0B : Spectrum Scanner telemetry data
  - 0x0C : Flysky AFHDS2 telemetry data
  - 0x0D : RX channels forwarding

1.3.1 高频头的状态信息(Multimodule Status)
-------------------------------------------

+-------+--------+--------+--------+--------+--------+--------+-------+----------+------------+
| Stream| 0      | 1      | 2      |  3     | 4      | 5      | 6     | 7        | 8          |
+-------+--------+--------+--------+--------+--------+--------+-------+----------+------------+
| DATA  | 0x4D   |  0x50  | 0x01   |  5     | Flag   | major  | minor | revision | patchlevel |
+-------+--------+--------+--------+--------+--------+--------+-------+----------+------------+

- Stream[4]: 状态标志(Flags)

  - 0位 :Imput signal

    - 1 : detected
    - 0 : No detected

  - 1位 :Serial mode

    - 1 : enabled
    - 0 : Disable

  - 2位 : Protocol Status

    - 1 : valid
    - 0 : unvalid

  - 3位 :

    - 1 : Module is in binding mode
    - 0 : Unbinding

  - 4位

    - 1 : Module waits a bind event to load the protocol
    - 0 :

  - 5位

    - 1 :Failsafe supported by currently running protocol
    - 0 :不支持失控保护

- Stream[5-8]:
  version of multi code ,should be displayed as major.minor.revision.patchlevel


1.3.2 Frksy S.port telemetry
------------------------------

+-------+--------+--------+--------+--------+--------+--------+-------+--------+
| Stream| 0      | 1      | 2      |  3     | 4      | 5      | 6     | 7      |
+-------+--------+--------+--------+--------+--------+--------+-------+--------+
| DATA  | 0x4D   |  0x50  | 0x02   |  0x09  | PhsicId| primid | Id             |
+-------+--------+--------+--------+--------+--------+--------+-------+--------+

+-------+--------+--------+--------+--------+--------+
| Stream| 8      | 9      | 10     |  11    | 12     |
+-------+--------+--------+--------+--------+--------+
| DATA  | data[0]| data[1]| data[2]| data[3]| crc    |
+-------+--------+--------+--------+--------+--------+

- Stream[4]:物理Id(PhsicalId),也称端口号(Instance)

- Stream[5]:primid,用于指示当前遥测数据包的数据类型

  - 0x10 :表明当前为传感器的数据.

- Stream[6-7]: 传感器Id,每个传感器都有一个唯一的Id.

  - ALT_FIRST_ID              0x0100

  - ALT_LAST_ID               0x010f

  - VARIO_FIRST_ID            0x0110

  - VARIO_LAST_ID             0x011f

  - CURR_FIRST_ID             0x0200

  - CURR_LAST_ID              0x020f

  - VFAS_FIRST_ID             0x0210

  - VFAS_LAST_ID              0x021f

  - CELLS_FIRST_ID            0x0300

  - CELLS_LAST_ID             0x030f

  - T1_FIRST_ID               0x0400

  - T1_LAST_ID                0x040f

  - T2_FIRST_ID               0x0410

  - T2_LAST_ID                0x041f

  - RPM_FIRST_ID              0x0500

  - RPM_LAST_ID               0x050f

  - FUEL_FIRST_ID             0x0600

  - FUEL_LAST_ID              0x060f

  - ACCX_FIRST_ID             0x0700

  - ACCX_LAST_ID              0x070f

  - ACCY_FIRST_ID             0x0710

  - ACCY_LAST_ID              0x071f

  - ACCZ_FIRST_ID             0x0720

  - ACCZ_LAST_ID              0x072f

  - GPS_LONG_LATI_FIRST_ID    0x0800

  - GPS_LONG_LATI_LAST_ID     0x080f

  - GPS_ALT_FIRST_ID          0x0820

  - GPS_ALT_LAST_ID           0x082f

  - GPS_SPEED_FIRST_ID        0x0830

  - GPS_SPEED_LAST_ID         0x083f

  - GPS_COURS_FIRST_ID        0x0840

  - GPS_COURS_LAST_ID         0x084f

  - GPS_TIME_DATE_FIRST_ID    0x0850

  - GPS_TIME_DATE_LAST_ID     0x085f

  - A3_FIRST_ID               0x0900

  - A3_LAST_ID                0x090f

  - A4_FIRST_ID               0x0910

  - A4_LAST_ID                0x091f

  - AIR_SPEED_FIRST_ID        0x0a00

  - AIR_SPEED_LAST_ID         0x0a0f

  - RBOX_BATT1_FIRST_ID       0x0b00

  - RBOX_BATT1_LAST_ID        0x0b0f

  - RBOX_BATT2_FIRST_ID       0x0b10

  - RBOX_BATT2_LAST_ID        0x0b1f

  - RBOX_STATE_FIRST_ID       0x0b20

  - RBOX_STATE_LAST_ID        0x0b2f

  - RBOX_CNSP_FIRST_ID        0x0b30

  - RBOX_CNSP_LAST_ID         0x0b3f

  - SD1_FIRST_ID              0x0b40

  - SD1_LAST_ID               0x0b4f

  - ESC_POWER_FIRST_ID        0x0b50

  - ESC_POWER_LAST_ID         0x0b5f

  - ESC_RPM_CONS_FIRST_ID     0x0b60

  - ESC_RPM_CONS_LAST_ID      0x0b6f

  - ESC_TEMPERATURE_FIRST_ID  0x0b70

  - ESC_TEMPERATURE_LAST_ID   0x0b7f

  - X8R_FIRST_ID              0x0c20

  - X8R_LAST_ID               0x0c2f

  - S6R_FIRST_ID              0x0c30

  - S6R_LAST_ID               0x0c3f

  - GASSUIT_TEMP1_FIRST_ID    0x0d00

  - GASSUIT_TEMP1_LAST_ID     0x0d0f

  - GASSUIT_TEMP2_FIRST_ID    0x0d10

  - GASSUIT_TEMP2_LAST_ID     0x0d1f

  - GASSUIT_SPEED_FIRST_ID    0x0d20

  - GASSUIT_SPEED_LAST_ID     0x0d2f

  - GASSUIT_RES_VOL_FIRST_ID  0x0d30

  - GASSUIT_RES_VOL_LAST_ID   0x0d3f

  - GASSUIT_RES_PERC_FIRST_ID 0x0d40

  - GASSUIT_RES_PERC_LAST_ID  0x0d4f

  - GASSUIT_FLOW_FIRST_ID     0x0d50

  - GASSUIT_FLOW_LAST_ID      0x0d5f

  - GASSUIT_MAX_FLOW_FIRST_ID 0x0d60

  - GASSUIT_MAX_FLOW_LAST_ID  0x0d6f

  - GASSUIT_AVG_FLOW_FIRST_ID 0x0d70

  - GASSUIT_AVG_FLOW_LAST_ID  0x0d7f

  - SBEC_POWER_FIRST_ID       0x0e50

  - SBEC_POWER_LAST_ID        0x0e5f

  - DIY_FIRST_ID              0x5100

  - DIY_LAST_ID               0x52ff

  - DIY_STREAM_FIRST_ID       0x5000

  - DIY_STREAM_LAST_ID        0x50ff

  - FACT_TEST_ID              0xf000

  - RSSI_ID                   0xf101

  - ADC1_ID                   0xf102

  - ADC2_ID                   0xf103

  - SP2UART_A_ID              0xfd00

  - SP2UART_B_ID              0xfd01

  - BATT_ID                   0xf104

  - RAS_ID                    0xf105

  - XJT_VERSION_ID            0xf106

  - FUEL_QTY_FIRST_ID         0x0a10

  - FUEL_QTY_LAST_ID          0x0a1f


- Stream[8-11]:用于存储传感器的数据，如果当前传感器的数据不足16个字节，则用0补其，如果超过16
  个字节，则下一次传输.

- Stream[12]:用于数据校验.


RSSI Sensor

- Stream[4]:PhsicId 0x98

- Stream[5]: primid 0x10

- Stream[8-11]: 1个字节(有效数据为1个字节，data[0]),其余为0

XJT VERSION

- Stream[8-11]: 2个字节（有效数据为2个字节, data[0],data[1])

RAS Sensor

- Stream[8-11]: 1个字节

ADC1/ADC2/BATT Sensor

- Stream[8-11]: 1个字节

GPS Sensor


RBOX_BATT1/RBOX_BATT2 Sensor

- Stream[8-11]: 4个字节


CELLS Sensor:电压测量，最多支持6节电池。每次传输2个电压数据,最多3次可以传
完。


- Stream[8-11]: 4个字节

  - Stream[8]: [3:0]电池的总节数,[7:4]当前节数

  - Stream[9-11]:3个字节，低12位存储，高12位存储




