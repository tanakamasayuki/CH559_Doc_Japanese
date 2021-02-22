---
title: 17. LED屏接口
type: docs
weight: 17
BookToC: false
---

# 17.LED屏接口

## 17.1 LED 控制卡接口
CH559提供了LED 屏控制卡数据传输接口，内置4级FIFO，支持DMA 和中断，节约CPU处理时间，支持1/2/4路数据线接口。

<center>表17.1.1 LED屏接口相关寄存器列表</center>
<table>
    <tr>
        <th>名称</th><th>地址</th><th>描述</th><th>复位值</th>
    </tr>
    <tr><td>LED_STAT</td><td>2880h</td><td>LED状态寄存器</td><td>010x 0000b</td></tr>
    <tr><td>LED_CTRL</td><td>2881h</td><td>LED控制寄存器</td><td>0000 0010b</td></tr>
    <tr><td>LED_FIFO_CN</td><td>2882h</td><td>FIFO计数状态寄存器(只读)</td><td>0000 0000b</td></tr>
    <tr><td>LED_DATA</td><td>2882h</td><td>LED数据寄存器(只写)</td><td>xxxx xxxxb</td></tr>
    <tr><td>LED_CK_SE</td><td>2883h</td><td>LED时钟分频设置寄存器</td><td>0001 0000b</td></tr>
    <tr><td>LED_DMA_AH</td><td>2884h</td><td>DMA当前缓冲区地址高字节</td><td>000x xxxxb</td></tr>
    <tr><td>LED_DMA_AL</td><td>2885h</td><td>DMA当前缓冲区地址低字节</td><td>xxxx xxx0b</td></tr>
    <tr><td>LED_DMA</td><td>2884h</td><td>LED_DMA_AL 和LED_DMA_AH 组成16位SFR</td><td>xxxxh</td></tr>
    <tr><td>LED_DMA_CN</td><td>2886h</td><td>LED DMA 剩余计数寄存器</td><td>xxxx xxxxb</td></tr>
    <tr><td>LED_DMA_XH</td><td>2888h</td><td>DMA当前辅助缓冲区地址高字节</td><td>000x xxxxb</td></tr>
    <tr><td>LED_DMA_XL</td><td>2889h</td><td>DMA当前辅助缓冲区地址低字节</td><td>xxxx xxx0b</td></tr>
    <tr><td>LED_DMA_X</td><td>2888h</td><td>LED_DMA_XL 和LED_DMA_XH 组成16位SFR</td><td>xxxxh</td></tr>
    <tr><td>pLED_*</td><td>298*h</td><td>在bXIR_XSFR置1后，该名称用于以pdata类型寻址上述xSFR，比xdata类型寻址更快捷<td></td></tr>
</table>

### LED 状态寄存器(LED_STAT):

<table>
   <tr>
      <th>位</th><th>名称</th><th>访问</th><th>描述</th><th>复位值</th>
   </tr>
   <tr><td>7</td><td>bLED_IF_DMA_END</td><td>RW</td><td>DMA 完成中断标志位，该位为1 表示有中断；该位为0 则无中断。写1清零或写LED_DMA_CN 时清零</td><td>0</td></tr>
   <tr><td>6</td><td>bLED_FIFO_EMPTY</td><td>RO</td><td>FIFO空状态指示位，为1表示FIFO空</td><td>0</td></tr>
   <tr><td>5</td><td>bLED_IF_FIFO_REQ</td><td>RW</td><td>该位为1 表示请求向FIFO写入数据中断标志位，由FIFO<=2触发；该位为0 则无中断。写1 清零</td><td>0</td></tr>
   <tr><td>4</td><td>bLED_CLOCK</td><td>R0</td><td>LED 时钟信号的当前电平</td><td>x</td></tr>
   <tr><td>3</td><td>保留</td><td>RO</td><td>保留</td><td>0</td></tr>
   <tr><td>[2:0]</td><td>MASK_LED_FIFO_CNT</td><td>R0</td><td>LED 的FIFO 当前计数</td><td>000b</td></tr>
</table>

### LED 控制寄存器(LED_CTRL):

<table>
   <tr>
      <th>位</th><th>名称</th><th>访问</th><th>描述</th><th>复位值</th>
   </tr>
   <tr><td>7</td><td>bLED_CHAN_MOD1</td><td>RW</td><td>LED 通道模式高位</td><td>0</td></tr>
   <tr><td>6</td><td>bLED_CHAN_MOD0</td><td>RW</td><td>LED 通道模式低位</td><td>0</td></tr>
   <tr><td>5</td><td>bLED_IE_FIFO_REQ</td><td>RW</td><td>该位为1 使能请求FIFO数据中断；该位为0 关闭使能</td><td>0</td></tr>
   <tr><td>4</td><td>bLED_DMA_EN</td><td>RW</td><td>该位为1 使能LED的DMA和DMA中断；为0 则关闭使能</td><td>0</td></tr>
   <tr><td>3</td><td>bLED_OUT_EN</td><td>RW</td><td>该位为1 使能LED 信号输出；该位为0 则禁止</td><td>0</td></tr>
   <tr><td>2</td><td>bLED_OUT_POLAR</td><td>RW</td><td>LED 数据输出极性控制位，该位为0 则直通输出，数据 0 输出低电平，数据1 输出高电平；该位为1 则反极性输出，数据0 输出高电平，数据1 输出低电平</td><td>0</td></tr>
   <tr><td>1</td><td>bLED_CLR_ALL</td><td>RW</td><td>该位为1 清空LED 中断标志和FIFO，需要软件清零</td><td>1</td></tr>
   <tr><td>0</td><td>bLED_BIT_ORDER</td><td>RW</td><td>数据字节的位序控制位，最前位通过LED0 发送，该位为0则LSB低位在前；该位为1则MSB 高位在前</td><td>0</td></tr>
</table>

<center>表17.1.2 LED通道模式</center>
<table>
    <tr>
        <th>bLED_CHAN_MOD1</th><th>bLED_CHAN_MOD0</th><th>描述</th>
    </tr>
    <tr><td>0</td><td>0</td><td>单通道数据输出：LED0</td></tr>
    <tr><td>0</td><td>1</td><td>双通道数据输出：LED0和LED1</td></tr>
    <tr><td>1</td><td>0</td><td>4 通道数据输出：LED0～LED3<br>DMA 只使用主缓冲区，按字节依次向LED0～LED3提供数据</td></tr>
    <tr><td>1</td><td>1</td><td>4 通道数据输出：LED0和LED1成组、LED2 和LED3成组 DMA 使用主缓冲区LED_DMA 向LED0和LED1 提供数据，同时使用辅助缓冲区LED_DMA_X 向LED2和LED3 提供数据</td></tr>
</table>

### FIFO计数状态寄存器(LED_FIFO_CN):

<table>
   <tr>
      <th>位</th><th>名称</th><th>访问</th><th>描述</th><th>复位值</th>
   </tr>
   <tr><td>[7:0]</td><td>LED_FIFO_CN</td><td>RO</td><td>FIFO当前数据字节数，仅低3 位有效，高5位固定为0</td><td>00h</td></tr>
</table>

### LED 数据寄存器(LED_DATA):

<table>
   <tr>
      <th>位</th><th>名称</th><th>访问</th><th>描述</th><th>复位值</th>
   </tr>
   <tr><td>[7:0]</td><td>LED_DATA</td><td>WO</td><td>LED 数据端口，用于将数据写入FIFO</td><td>xxh</td></tr>
</table>

### LED 时钟分频设置寄存器(LED_CK_SE):

<table>
   <tr>
      <th>位</th><th>名称</th><th>访问</th><th>描述</th><th>复位值</th>
   </tr>
   <tr><td>[7:0]</td><td>LED_CK_SE</td><td>RW</td><td>设置LED 输出时钟的分频系数</td><td>10h</td></tr>
</table>

### DMA 当前缓冲区地址(LED_DMA):

<table>
   <tr>
      <th>位</th><th>名称</th><th>访问</th><th>描述</th><th>复位值</th>
   </tr>
   <tr><td>[7:0]</td><td>LED_DMA_AH</td><td>RW</td><td>当前DMA 地址高字节，可预置初值，DMA 后自动增加，仅低5位有效，高3位固定为0</td><td>xxh</td></tr>
   <tr><td>[7:0]</td><td>LED_DMA_AL</td><td>RW</td><td>当前DMA 地址低字节，可预置初值，DMA 后自动增加，仅高7位有效，最低位固定为0，仅支持偶地址</td><td>xxh</td></tr>
</table>

### DMA 当前辅助缓冲区地址(LED_DMA_X):

<table>
   <tr>
      <th>位</th><th>名称</th><th>访问</th><th>描述</th><th>复位值</th>
   </tr>
   <tr><td>[7:0]</td><td>LED_DMA_XH</td><td>RW</td><td>辅助缓冲区当前DMA 地址高字节，可预置初值，DMA 后自动增加，仅低5 位有效，高3位固定为0</td><td>xxh</td></tr>
   <tr><td>[7:0]</td><td>LED_DMA_XL</td><td>RW</td><td>辅助缓冲区当前DMA 地址低字节，可预置初值，DMA 后自动增加，仅高7 位有效，最低位固定为0，仅支持偶地址</td><td>xxh</td></tr>
</table>

### DMA 剩余计数寄存器(LED_DMA_CN):

<table>
   <tr>
      <th>位</th><th>名称</th><th>访问</th><th>描述</th><th>复位值</th>
   </tr>
   <tr><td>[7:0]</td><td>LED_DMA_CN</td><td>RW</td><td>LED_DMA 主缓冲区中当前DMA 剩余计数，以双字节为单位，可预置初值，DMA 操作后自动减少。不含辅助缓冲区LED_DMA_X 中的剩余计数</td><td>00h</td></tr>
</table>

## 17.2 LED 控制应用

1. 设置LEDC和必要的LED0～LED3 为输出，可选地，设置相应I/O的驱动能力；
2. 设置LED_CK_SE 选择LED输出时钟频率；
3. 设置DMA起始地址LED_DMA 指向准备输出数据的缓冲区，即主缓冲区；
4. 如果是LED 通道模式3，那么还要设置辅助DMA 起始地址LED_DMA_X指向辅助缓冲区；
5. 设置LED 控制寄存器LED_CTRL，选择通道模式、输出极性、位顺序、启用中断和DMA 等，例如LED_CTRL = bLED_CHAN_MOD0 | bLED_DMA_EN | bLED_OUT_EN；
6. 设置DMA计数，启动DMA发送，或者用写FIFO的方式发送数据；
7. 查询或者使用中断处理中断相应状态。
