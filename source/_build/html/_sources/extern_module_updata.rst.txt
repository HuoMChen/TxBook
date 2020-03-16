===========================
升级说明
===========================

1.2.1.67 到 1.3.0.53
==============================

外置高频头协议更改
------------------------

- 1：遥测增加SCANNER、AFHDS2A_AC、RX_CHANNERS、HOTT四种数据
- 2：新增加52到61协议的命名
- 3：以下3个协议添加子协议

  - ：sub_protocol==TRAXXAS

    - :RX6519      0

  - :sub_protocol==ESKY150

    - :ESKY150_4CH 0
    - :ESKY150_7CH 1

  - :sub_protocol==V911S

    - :V911S_STD 0
    - :V911S_STD 1

- 4：高频头协议增加多个字节

  - Stream[26]   = sub_protocol bits 6 & 7|RxNum bits 4 & 5|Telemetry_Invert 3|Future_Use 2|Disable_Telemetry 1|Disable_CH_Mapping 0

    sub_protocol is 0..255 (bits 0..5 + bits 6..7)

    RxNum value is 0..63 (bits 0..3 + bits 4..5)

    Telemetry_Invert		=> 0x08	0=normal, 1=invert

    Future_Use			=> 0x04	0=      , 1=

    Disable_Telemetry	=> 0x02	0=enable, 1=disable

    Disable_CH_Mapping	=> 0x01	0=enable, 1=disable

  - Stream[27.. 35] = between 0 and 9 bytes for additional protocol data

- 5：多协议状态信息新增加字节、Flags字节新增位

  - [2] Flags

    - 0x20 = Current protocol supports failsafe

    - 0x40 = Current protocol supports disable channel mapping

    - 0x80 = Data buffer is almost full

  - [9] channel order: CH4|CH3|CH2|CH1 with CHx value A=0,E=1,T=2,R=3

  - [10] Next valid protocol number, can be used to skip invalid protocols

  - [11] Prev valid protocol number, can be used to skip invalid protocols

  - [12..18] Protocol name [7], not null terminated if prototcol len == 7

  - [19>>4] Option text to be displayed:

    - OPTION_NONE		0

    - OPTION_OPTION	1

    - OPTION_RFTUNE	2

    - OPTION_VIDFREQ	3

    - OPTION_FIXEDID	4

    - OPTION_TELEM	5

    - OPTION_SRVFREQ	6

    - OPTION_MAXTHR	7

    - OPTION_RFCHAN	8

  - [19&0x0F] Number of sub protocols

  - [20..27] Sub protocol name [8], not null terminated if sub prototcol len == 8

- 6：失控保护修改为0=no pulse, 2047=hold

- 7:新增以下遥测数据包

  - Type 0x08 Input synchronisation

    - Informs the TX about desired rate and current delay

    - length: 4

    - data[0-1]     Desired refresh rate in ??s

    - data[2-3]     Time (??s) between last serial servo input received and servo input needed (lateness), TX should adjust its
      sending time to minimise this value.

    - data[4]		  Interval of this message in ms

    - data[5]		  Input delay target in 10??s

    - Note that there are protocols (AFHDS2A) that have a refresh rate that is smaller than the maximum achievable

    - refresh rate via the serial protocol, in this case, the TX should double the rate and also subract this

    - refresh rate from the input lag if the input lag is more than the desired refresh rate.

    - The remote should try to get to zero of  (inputdelay+target*10).

  - Type 0x0B Spectrum Scanner telemetry data

    - length: 6

    - data[0] = start channel (2400 + x*0.333 Mhz)

    - data[1-5] power levels

  - Type 0x0C Flysky AFHDS2 telemetry data type 0xAC

    - length: 29

    - data[0] = RSSI value

    - data[1-28] telemetry data

  - Type 0x0D RX channels forwarding

    - length: variable

    - data[0] = received packets per second

    - data[1] = rssi

    - data[2] = start channel

    - data[3] = number of channels to follow

    - data[4-]= packed channels data, 11 bit per channel

  - Type 0x0E HoTT telemetry

    - length: 14

    - data[0] = TX_RSSI

    - data[1] = TX_LQI

    - data[2] = type

    - data[3] = page

    - data[4-13] = data

- frsky sprot 新增加两种传感器 TX_RSSI_ID,TX_LQI_ID，和一小部分完善。

- flysky ibus 信增加38种传感器，需要添加相应的解析程序。

- frsky d 遥测要完善。

