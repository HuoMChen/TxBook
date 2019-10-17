map文件分析
=============

c语言存储区域
--------------

1、代码段
    程序语句


2、数据
    已经初始化

        只读数据段(RO Data)

        已经初始化读写数据(RW Data)
            
            已初始化全局变量
            已经初始化的函数static

    未初始化

        未初始化数据段(.bss)

3、堆栈
    堆(heaap):程序员malloc和release

    栈(statk):在函数内部由编译器自动分配和释放
    
ARM程序
--------

1、ARM程序的组成
    RO段(text)、RW段和ZI段(未初始化的数据),这些称为静态存储
    ZI对应C语言的.bss

2、ROM拷贝到RAM
    RW数据和ZI数据会

    Total RO  Size (Code + RO Data)               968004 ( 945.32kB)
    Total RW  Size (RW Data + ZI Data)            175320 ( 171.21kB)
    Total ROM Size (Code + RO Data + RW Data)     977676 ( 954.76kB)

text和data段都在可执行文件中（在嵌入式系统里一般是固化在镜像文件中），由系统从可执行文件中加载；

而bss段不在可执行文件中，由系统初始化

Flash=Code + RO Data + RW Data;

RAM= RW-data+ZI-data + 堆栈;

五、程序中段的使用

C语言中的全局区(静态区)，实际上对应着下述几个段：

只读数据段:RO Data

读写数据段：RW Data

未初始化数据段：BSS Data


在C语言的程序中，对变量的使用需要注意的问题

1、在函数体中定义的变量通常是在栈上，不需要在程序中进行管理，由编译器处理。

2、用malloc，calloc,realoc等分配分配内存的函数所分配的内存空间在堆上，程序必须保证在使用后使用后freee释放，否则会发生内存泄漏。

3、所有函数体外定义的是全局变量，加了static修饰符后的变量不管在函数内部或者外部存放在全局区(静态区)。

4、使用const定义的变量将放于程序的只读数据区。

mdk map文件
-----------

通过仔细阅读.map文件，我们可以大致分为5个部分：

列出不同函数的调用关系

.. testoutput::
    Section Cross References

        stm32f4xx_hal_adc.o(i.ADC_DMAConvCplt) refers to wflyrcdevstd_adc.o(i.HAL_ADC_ConvCpltCallback) for HAL_ADC_ConvCpltCallback
        stm32f4xx_hal_adc.o(i.ADC_DMAError) refers to stm32f4xx_hal_adc.o(i.HAL_ADC_ErrorCallback) for HAL_ADC_ErrorCallback
            

列出被MDK优化的冗余函数

.. testoutput::
    Removing Unused input sections from the image.

    Removing stm32f4xx_hal_adc.o(.rev16_text), (4 bytes).
    Removing stm32f4xx_hal_adc.o(.revsh_text), (4 bytes).
    Removing stm32f4xx_hal_adc.o(.rrx_text), (6 bytes).
           

列出局部标签和全局标签

    
.. testoutput::
    Image Symbol Table

    Local Symbols

    Symbol Name                              Value     Ov Type        Size  Object(Section)

    ..//Src/system_stm32f4xx.c               0x00000000   Number         0  system_stm32f4xx.o ABSOLUTE
    ../Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal.c 0x00000000   Number         0  stm32f4xx_hal.o ABSOLUTE
    ...
    
    RESET                                    0x08008000   Section      392  startup_stm32f407xx.o(RESET)
    !!!main                                  0x08008188   Section        8  __main.o(!!!main)
    !!!scatter                               0x08008190   Section       52  __scatter.o(!!!scatter)
    !!dclz77c                                0x080081c4   Section      100  __dclz77c.o(!!dclz77c)
    ...
    .conststring                             0x080f44dc   Section       42  freertos.o(.conststring)
    locale$$data                             0x080f4528   Section       28  lc_numeric_c.o(locale$$data)
    __lcnum_c_name                           0x080f452c   Data           2  lc_numeric_c.o(locale$$data)
    __lcnum_c_start                          0x080f4534   Data           0  lc_numeric_c.o(locale$$data)
    __lcnum_c_point                          0x080f4540   Data           0  lc_numeric_c.o(locale$$data)
    __lcnum_c_thousands                      0x080f4542   Data           0  lc_numeric_c.o(locale$$data)
    __lcnum_c_grouping                       0x080f4543   Data           0  lc_numeric_c.o(locale$$data)
    __lcnum_c_end                            0x080f4544   Data           0  lc_numeric_c.o(locale$$data)
    .ARM.__AT_0x10000000                     0x10000000   Section    64512  guiconf.o(.ARM.__AT_0x10000000)
    aMemory                                  0x10000000   Data       64512  guiconf.o(.ARM.__AT_0x10000000)
    .data                                    0x20003000   Section        4  stm32f4xx_hal.o(.data)
    .data                                    0x20003004   Section       24  heap_4.o(.data)
    xStart                                   0x20003004   Data           8  heap_4.o(.data)
    pxEnd                                    0x2000300c   Data           4  heap_4.o(.data)
    

列出映像文件的内存映射

.. testoutput::
    Memory Map of the image

    Image Entry point : 0x08008189

    Load Region LR_IROM1 (Base: 0x08008000, Size: 0x000f2180, Max: 0x000f8000, ABSOLUTE, COMPRESSED[0x000eeb0c])

    Execution Region ER_IROM1 (Base: 0x08008000, Size: 0x000ec544, Max: 0x000f8000, ABSOLUTE)

    Base Addr    Size         Type   Attr      Idx    E Section Name        Object

    0x08008000   0x00000188   Data   RO         4419    RESET               startup_stm32f407xx.o
    0x08008188   0x00000008   Code   RO        28410  * !!!main             c_w.l(__main.o)
    0x08008190   0x00000034   Code   RO        28836    !!!scatter          c_w.l(__scatter.o)
    0x080081c4   0x00000064   Code   RO        28834    !!dclz77c           c_w.l(__dclz77c.o)
    0x08008228   0x0000001c   Code   RO        28838    !!handler_zi        c_w.l(__scatter_zi.o)
    
        0x080f4508   0x00000020   Data   RO        28832    Region$$Table       anon$$obj.o
    0x080f4528   0x0000001c   Data   RO        28725    locale$$data        c_w.l(lc_numeric_c.o)


    Execution Region RW_IRAM1 (Base: 0x20003000, Size: 0x0001b0d8, Max: 0x0001d000, ABSOLUTE, COMPRESSED[0x000025c8])

    Base Addr    Size         Type   Attr      Idx    E Section Name        Object

    0x20003000   0x00000004   Data   RW         3201    .data               stm32f4xx_hal.o
    0x20003004   0x00000018   Data   RW         3332    .data               heap_4.o
    0x2000301c   0x0000000c   Data   RW         3417    .data               port.o
    
        0x2001d8d8   0x00000400   Zero   RW         4418    HEAP                startup_stm32f407xx.o
    0x2001dcd8   0x00000400   Zero   RW         4417    STACK               startup_stm32f407xx.o



  Load Region LR$$.ARM.__AT_0x10000000 (Base: 0x10000000, Size: 0x00000000, Max: 0x0000fc00, ABSOLUTE)

    Execution Region ER$$.ARM.__AT_0x10000000 (Base: 0x10000000, Size: 0x0000fc00, Max: 0x0000fc00, ABSOLUTE, UNINIT)

    Base Addr    Size         Type   Attr      Idx    E Section Name        Object

    0x10000000   0x0000fc00   Zero   RW         6879    .ARM.__AT_0x10000000  guiconf.o


列出映像文件的组件大小
 
.. testoutput::
    Code (inc. data)   RO Data    RW Data    ZI Data      Debug   

    623734      70342     344270      23612     151708    2378227   Grand Totals
    623734      70342     344270       9672     151708    2378227   ELF Image Totals (compressed)
    623734      70342     344270       9672          0          0   ROM Totals


    Total RO  Size (Code + RO Data)               968004 ( 945.32kB)
    Total RW  Size (RW Data + ZI Data)            175320 ( 171.21kB)
    Total ROM Size (Code + RO Data + RW Data)     977676 ( 954.76kB)






