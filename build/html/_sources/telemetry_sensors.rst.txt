==================
Telemetry Sensors
==================

简介
===============

telemetry_sensor.c文件的主要作用就是把遥测回来的传感器数据数据按照用户的
在界面的设置进行运算处理、单位转换、精度转换等相关处理，其实这部分内容已经是
属于应用层次。

一些概念
================

id、subid、instances
---------------------

传感器类型
=================

传感器类型分为两类：Custom和Caculated

为什么要这样分？
---------------

Custom类传感器
-----------------

这类传感器的数据来源于外部模块，是真实的传感器，所以可以设置Id和Subid用于
匹配外部模块的传感器。

Caculated类传感器
--------------------

这类传感器的数据源来自已经连接上的外部传感器，是虚拟的传感器，所以不能设置
Id和Subid。根据需要对该传感器进行各类运算操作。


一些功能
==========

传感器丢失检测
---------------

能够检测传感器是否处于实时连接、当传感器断开10S则为丢失。

精度选择
------------

对于传感器的数据可以进行

数据结构
================

TelemetrySensor
-----------------------

.. code-block:: c

  typedef struct{
    union {
      uint16_t id;                   // data identifier, for FrSky we can reuse existing ones. Source unit is derived from type.
      uint16_t persistentValue;
    }identifier;
    union {
      struct {
        uint8_t physID:5;
        uint8_t rxIndex:3; // 1 bit for module index, 2 bits for receiver index
      }frskyInstance;
      uint8_t instance;              // instance ID to allow handling multiple instances of same value type, for FrSky can be the physical ID of the sensor
      uint8_t formula;
    }ins;
    char     label[TELEM_LABEL_LEN]; // user defined label
    uint8_t  subId;   /*每种传感器不一样*/
    uint8_t  type:1; // 0=custom / 1=calculated // user can choose what unit to display each value in
    uint8_t  spare1:1;
    uint8_t  unit:6;  /*精度*/
    uint8_t  prec:2;  /*单位*/
    uint8_t  autoOffset:1;
    uint8_t  filter:1;
    uint8_t  logs:1;  /*用于表示传感器数据是否存于外部存储*/
    uint8_t  persistent:1;
    uint8_t  onlyPositive:1;
    uint8_t  spare2:1;
    union {
      struct{
        uint16_t ratio;   /*比例*/
        int16_t  offset;  /*偏移*/
      }custom;
      struct{
        uint8_t source;   /*数据源*/
        uint8_t index;    
        uint16_t spare;
      }cell;
      struct{
        int8_t sources[4];
      }calc;
      struct{
        uint8_t source;
        uint8_t spare[3];
      }consumption;
      struct{
        uint8_t gps;
        uint8_t alt;
        uint16_t spare;
      }dist;
      uint32_t param;
    }data;

  }TelemetrySensor;

这部分数据是用户可设置的，需要存储。

TelemetryItem
---------------

.. code-block:: c

  typedef struct{
      union {
        int32_t  value;           // value, stored as uint32_t but interpreted accordingly to type
        uint32_t distFromEarthAxis;
      }value;

      union {
        int32_t valueMin;         // min store
        int32_t pilotLongitude;
      }min;

      union {
        int32_t valueMax;         // max store
        int32_t pilotLatitude;
      }max;

      uint8_t lastReceived;       // for detection of sensor loss

      union {
        struct {
          int32_t  offsetAuto;
          int32_t  filterValues[TELEMETRY_AVERAGE_COUNT];
        } std;
        struct {
          uint16_t prescale;
        } consumption;
        struct {
          uint8_t   count;
          CellValue values[6];
        } cells;
        struct {
          uint16_t year;          // full year (4 digits)
          uint8_t  month;
          uint8_t  day;
          uint8_t  hour;
          uint8_t  min;
          uint8_t  sec;
        } datetime;
        struct {
          int32_t latitude;
          int32_t longitude;
          // pilot longitude is stored in min
          // pilot latitude is stored in max
          // distFromEarthAxis is stored in value
        } gps;
        char text[16];
      }telemetry;

  }TelemetryItem;

该结构体主要存储传感器的实时数值、最大、最小和状态信息，不需要存储。