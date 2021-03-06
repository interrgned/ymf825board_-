# YMF825(SD-1) IF 规范

## 注意

尽管本手册是基于SD-1规范创建的，但仅描述了YMF825Board所需的信息。 因此，请事先了解它不是SD-1的完整规格。
此外，请理解我们不会保证根据本手册中公布的内容使用SD-1的任何结果。

## 特征

+ 16复音FM合成器
+ 29种预设运算器波形和8种FM合成算法
+ 同步串行连接的主机接口
+ 集成扬声器驱动（还支持连接外部放大器）
+ 集成3段均衡器
+ 集成16位单声道DAC

## 接口寄存器

### 系统设置

+ I_ADR#0-2, 4, 29 can be accessed even when the ALRST is "1".
正如所料，只有当ALRST为“0”时才能访问其他寄存器。


| I_ADR | 寄存器名 | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | 复位初值 |
|-|-|-|-|-|-|-|-|-|-|-|-|
|#0| 时钟使能 |W/R|"0"|"0"|"0"|"0"|"0"|"0"|"0"|CLKE|00H |
|#1|复位|W/R|ALRST|"0"|"0"|"0"|"0"|"0"|"0"|"0"|80H|
|#2|模拟部分掉电控制|W/R|"0"|"0"|"0"|"0"|AP3|AP2|AP1|AP0| 0FH|
|#3|扬声器放大增益设置|W/R|"0"|"0"|"0"|"0"|"0"|"0"|GAIN1|GAIN0|01H|
|#4|硬件ID|R|"0"|"0"|"0"|"0"|"0"|"0"|"0"|"1"|01H|
|#5|中断|W/R R|"0"|"0"|"0"|EMP_DW|"0"|FIFO|"0"|SQ_STP|00H|
|#6||W/R|"0"|EIRQ|"0"|EEMP_DW|"0"|EFIFO|"0"|ESQ_STP|00H|
|#7|内容数据写入端口|W|DT7|DT6|DT5|DT4|DT3|DT2|DT1|DT0|00H|
|#8|音序器设置|W/R|AllKeyOff|AllMute|AllEGRst|R_FIFOR|REP_SQ|R_SEQ| R_FIFO|START|00H|
|#9||W/R|SEQ_Vol4|SEQ_Vol3|SEQ_Vol2|SEQ_Vol1|SEQ_Vol0|DIR_SV|"0"|SIZE8|00H|
|#10||W/R|SIZE7|SIZE6|SIZE5|SIZE4|SIZE3|SIZE2|SIZE1|SIZE0|00H|
|#11|合成器设置|W/R|"0"|"0"|"0"|"0"|CRGD_VNO3|CRGD_VNO2|CRGD_VNO1|CRGD_VNO0|00H|
|#12||W|"0"|VoVol4|VoVol3|VoVol2|VoVol1|VoVol0|"0"|"0"|60H|
|#13||W|"0"|"0"|FNUM9|FNUM8|FNUM7|BLOCK2|BLOCK1|BLOCK0|00H|
|#14||W|"0"|FNUM6|FNUM5|FNUM4|FNUM3|FNUM2|FNUM1|FNUM0|00H|
|#15||W|"0"|KeyOn|Mute|EG_RST|ToneNum3|ToneNum2|ToneNum1|ToneNum0|00H|
|#16||W|"0"|ChVol4|ChVol3|ChVol2|ChVol1|ChVol0|"0"|DIR_CV|60H|
|#17||W|"0"|"0"|"0"|"0"|"0"|XVB2|XVB1|XVB0|00H|
|#18||W|"0"|"0"|"0"|INT1|INT0|FRAC8|FRAC7|FRAC6|08H|
|#19||W|"0"|FRAC5|FRAC4|FRAC3|FRAC2|FRAC1|FRAC0|"0"|00H|
|#20||W|"0"|"0"|"0"|"0"|"0"|"0"|"0"|DIR_MT|00H|
|#21|控制寄存器读出端口|W/R|RDADR_CRG7|RDADR_CRG6|RDADR_CRG5|RDADR_CRG4|RDADR_CRG3|RDADR_CRG2|RDADR_CRG1|RDADR_CRG0|00H|
|#22||R|"0"|RDDATA_CRG6|RDDATA_CRG5|RDDATA_CRG4|RDDATA_CRG3|RDDATA_CRG2|RDDATA_CRG1|RDDATA_CRG0|-|
|#23|音序器时间单位设置|W/R|"0"|MS_S13|MS_S12|MS_S11|MS_S10|MS_S9|MS_S8|MS_S7|00H|
|#24||W/R|"0"|MS_S6|MS_S5|MS_S4|MS_S3|MS_S2|MS_S1|MS_S0|00H|
|#25|主音量|W/R|MASTER_VOL5|MASTER_VOL4|MASTER_VOL3|MASTER_VOL2|MASTER_VOL1|MASTER_VOL0|"0"|"0"|00H|
|#26|软复位|W/R|SFTRST7|SFTRST6|SFTRST5|SFTRST4|SFTRST3|SFTRST2|SFTRST1|SFTRST0|00H|
|#27|Sequencer Delay, Recovery Function Setting, Volume Interpolation Setting|W/R|"0"|DADJT|MUTE_ITIME1|MUTE_ITIME0|CHVOL_ITIME1|CHVOL_ITIME0|MVOL_ITIME1|MVOL_ITIME0|00H|
|#28|LFO复位|W/R|"0"|"0"|"0"|"0"|"0"|"0"|"0"|LFO_RST|00H|
|#29|电源通道设置|W/R|"0"|"0"|"0"|"0"|"0"|"0"|"0"|DRV_SEL|00H|
|#30|保留|||||||||||
|#31|保留|||||||||||

注意
+ Before you write I_ADR#12-19 of Synthesizer Setting, you first have to write channel number to CRGD_VNO[3:0] (I_ADR#11).


### EQ系数设定

|I_ADR|寄存器名|W/R|D7-D0|复位初值|
|-|-|-|-|-|
|#32|EQ BAND0 coefficient Write Port|W|W_CEQ0[7:0]|00H|
|#33|EQ BAND1 coefficient Write Port|W|W_CEQ1[7:0]|00H|
|#34|EQ BAND2 coefficient Write Port|W|W_CEQ2[7:0]|00H|
|#35|Equalizer Coefficient Read Ports|R|CEQ00[23:16]|10H|
|#36||R|CEQ00[15:8]|00H|
|#37||R|CEQ00[7:0]|00H|
|#38||R|CEQ01[23:16]|00H|
|#39||R|CEQ01[15:8]|00H|
|#40||R|CEQ01[7:0]|00H|
|#41||R|CEQ02[23:16]|00H|
|#42||R|CEQ02[15:8]|00H|
|#43||R|CEQ02[7:0]|00H|
|#44||R|CEQ03[23:16]|00H|
|#45||R|CEQ03[15:8]|00H|
|#46||R|CEQ03[7:0]|00H|
|#47||R|CEQ04[23:16]|00H|
|#48||R|CEQ04[15:8]|00H|
|#49||R|CEQ04[7:0]|00H|
|#50||R|CEQ10[23:16]|10H|
|#51||R|CEQ10[15:8]|00H|
|#52||R|CEQ10[7:0]|00H|
|#53||R|CEQ11[23:16]|00H|
|#54||R|CEQ11[15:8]|00H|
|#55||R|CEQ11[7:0]|00H|
|#56||R|CEQ12[23:16]|00H|
|#57||R|CEQ12[15:8]|00H|
|#58||R|CEQ12[7:0]|00H|
|#59||R|CEQ13[23:16]|00H|
|#60||R|CEQ13[15:8]|00H|
|#61||R|CEQ13[7:0]|00H|
|#62||R|CEQ14[23:16]|00H|
|#63||R|CEQ14[15:8]|00H|
|#64||R|CEQ14[7:0]|00H|
|#65||R|CEQ20[23:16]|10H|
|#66||R|CEQ20[15:8]|00H|
|#67||R|CEQ20[7:0]|00H|
|#68||R|CEQ21[23:16]|00H|
|#69||R|CEQ21[15:8]|00H|
|#70||R|CEQ21[7:0]|00H|
|#71||R|CEQ22[23:16]|00H|
|#72||R|CEQ22[15:8]|00H|
|#73||R|CEQ22[7:0]|00H|
|#74||R|CEQ23[23:16]|00H|
|#75||R|CEQ23[15:8]|00H|
|#76||R|CEQ23[7:0]|00H|
|#77||R|CEQ24[23:16]|00H|
|#78||R|CEQ24[15:8]|00H|
|#79||R|CEQ24[7:0]|00H|

### 软件通讯检查

| I_ADR | 寄存器名 | W/R | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | 复位初值 |
|-|-|-|-|-|-|-|-|-|-|-|-|
|#80|软件测试通信|W/R|COMM7|COMM6|COMM5|COMM4|COMM3|COMM2|COMM1|COMM0|00H|


## 读/写接口寄存器的访问

### SPI 规范
+ 数据高位优先
+ 模式 0
+ 最大频率 10MHz

### 单次写入

单次写入需要2个字节（16位）：1字节写命令和1字节写数据。
务必以两个字节（16位）访问寄存器。
每写入两个字节，应当将/SS引脚置高。

![Single Write](spi_single.png)

### 突发写入

在突发写入操作中，数据可以连续写入相同的接口寄存器地址。
为一个写命令连续输入多个数据，如下所示：[写指令 + 数据 + 数据 + ...]。
该器件将/SS引脚置“H”理解为写操作的结束；因此，请确保在突发写入操作后将此引脚设置为“H”。
每个数据应为一个字节。如果/SS引脚在小于1字节的时候置“H”，例如6位，则可能因为非法写操作而发生异常。

![Burst Write](spi_burst.png)

### 读出访问

将WR（命令）位设置为“1”表示读访问命令。
从第9个时钟开始，与SCK的下降沿同步地发送读取数据。
数据采用MSB优先格式（D7→D0）。
以下显示了SO引脚的详细信息：
+ 在后8个时钟周期期间，读取数据（D[7:0]）以MSB优先格式发送。
+ 只要没有读取数据，SO引脚就会进入高阻态（Hi-Z）。

![Single Read](spi_read.png)


## 音调参数存储器映射

|T_ADR|寄存器名|D7|D6|D5|D4|D3|D2|D1|D0|
|-|-|-|-|-|-|-|-|-|-|
|#0+30x[tn]|整个音调设置|"0"|"0"|"0"|"0"|"0"|"0"|BO1|BO0|
|#1+30x[tn]||LFO1|LFO0|"0"|"0"|"0"|ALG2|ALG1|ALG0|
|#2+30x[tn]|运算器1设置|SR3|SR2|SR1|SR0|XOF|"0"|"0"|KSR|
|#3+30x[tn]||RR3|RR2|RR1|RR0|DR3|DR2|DR1|DR0|
|#4+30x[tn]||AR3|AR2|AR1|AR0|SL3|SL2|SL1|SL0|
|#5+30x[tn]||TL5|TL4|TL3|TL2|TL1|TL0|KSL1|KSL0|
|#6+30x[tn]||"0"|DAM1|DAM0|EAM|"0"|DVB1|DVB0|EVB|
|#7+30x[tn]||MULTI3|MULTI2|MULTI1|MULTI0|"0"|DT2|DT1|DT0|
|#8+30x[tn]||WS4|WS3|WS2|WS1|WS0|FB2|FB1|FB0|
|#9+30x[tn]|运算器2设置|SR3|SR2|SR1|SR0|XOF|"0"|"0"|KSR|
|#10+30x[tn]||RR3|RR2|RR1|RR0|DR3|DR2|DR1|DR0|
|#11+30x[tn]||AR3|AR2|AR1|AR0|SL3|SL2|SL1|SL0|
|#12+30x[tn]||TL5|TL4|TL3|TL2|TL1|TL0|KSL1|KSL0|
|#13+30x[tn]||"0"|DAM1|DAM0|EAM|"0"|DVB1|DVB0|EVB|
|#14+30x[tn]||MULTI3|MULTI2|MULTI1|MULTI0|"0"|DT2|DT1|DT0|
|#15+30x[tn]||WS4|WS3|WS2|WS1|WS0|"0"|"0"|"0"|
|#16+30x[tn]|运算器3设置|SR3|SR2|SR1|SR0|XOF|"0"|"0"|KSR|
|#17+30x[tn]||RR3|RR2|RR1|RR0|DR3|DR2|DR1|DR0|
|#18+30x[tn]||AR3|AR2|AR1|AR0|SL3|SL2|SL1|SL0|
|#19+30x[tn]||TL5|TL4|TL3|TL2|TL1|TL0|KSL1|KSL0|
|#20+30x[tn]||"0"|DAM1|DAM0|EAM|"0"|DVB1|DVB0|EVB|
|#21+30x[tn]||MULTI3|MULTI2|MULTI1|MULTI0|"0"|DT2|DT1|DT0|
|#22+30x[tn]||WS4|WS3|WS2|WS1|WS0|FB2|FB1|FB0|
|#23+30x[tn]|运算器4设置|SR3|SR2|SR1|SR0|XOF|"0"|"0"|KSR|
|#24+30x[tn]||RR3|RR2|RR1|RR0|DR3|DR2|DR1|DR0|
|#25+30x[tn]||AR3|AR2|AR1|AR0|SL3|SL2|SL1|SL0|
|#26+30x[tn]||TL5|TL4|TL3|TL2|TL1|TL0|KSL1|KSL0|
|#27+30x[tn]||"0"|DAM1|DAM0|EAM|"0"|DVB1|DVB0|EVB|
|#28+30x[tn]||MULTI3|MULTI2|MULTI1|MULTI0|"0"|DT2|DT1|DT0|
|#29+30x[tn]||WS4|WS3|WS2|WS1|WS0|"0"|"0"|"0"|

注意
+ T_ADR means Tone Setting format of Cotents Format(below).
+ tn: 声音编号(0-15)


### 内容格式

The contents format specifies tone parameters and the sequence of data that can be played back with this device consists of melody contents.
The contents are written into the register (I_ADR#7: CONTENTS_DATA_REG) via the CPU interface.

#### 数据格式

+ Header: 1byte(80H + Maximum Tone Number)
+ Tone Setting 30 to 480bytes(it depends on the number of the configured tones)
+ Sequence Data(any size)
+ End(80H,03H,81H,80H)

#### 音调设定

The tone parameters are set by the number of tones set to the Header.
The parameter consists of 30 bytes of data for one tone.
The data are transferred and assigned to the Tone parameter memory from Tone 0 in the order they are written; therefore, parameters of an intermediate Tone number cannot be written first.
For details of the tone parameters, see "Tone Parameter"(fbd_spec3.md).

## 初始化程序

1. 为设备供电。 

	当无法满足上电复位要求时，RST_N引脚应保持“L”。

1. 电源电压上升后等待100us

	达到指定的电压。
	这段时间是稳压器到达稳定状态所需的时间。
	
1. 将RST_N引脚设置为“H”。

	硬件复位状态被取消。
	No need to set to "H" when this device is used with RST_N pin held "H".

1. 当此器件用于单5V电源配置时，将DRV_SEL设置为“0”。
当此设备用于双电源配置时，将DRV_SEL设置为“1”。

	选择电源输入。

1. 将AP0设置为“0”。

	VREF已供电。

1. 等待时钟稳定

	等待晶振到稳定状态

1. 将CLKE设置为“1”。

	时钟提供给内部电路。

1. 将ALRST设置为“0”。

	清除内部电路的复位状态。

1. 将SFTRST设置为“A3H”。

	合成器模块初始化。

1. 将SFTRST设置为“00H”。

1. 在步骤10之后等待30ms。

	这段时间是VREF稳定和SFTRST完成所需的时间。
	
1. 将AP1和AP3设置为“0”。

	清除音频输出的断电状态。

1. 等待10us。

	这段时间是防止爆音所需的时间。 使用这段时间来设置合成器等。

1. 将AP2设置为“0”。 

	清除音频输出的断电状态。


