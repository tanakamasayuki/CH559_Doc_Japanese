---
title: 18. 参数
type: docs
weight: 18
BookToC: false
---

# 18.参数

## 18.1 绝对最大值(临界或者超过绝对最大值将可能导致芯片工作不正常甚至损坏)
<table>
    <tr>
        <th>名称</th><th>参数说明</th><th>最小值</th><th>最大值</th><th>单位</th>
    </tr>
    <tr><td>TA</td><td>工作时的环境温度</td><td>-40</td><td>85</td><td>℃</td></tr>
    <tr><td>TS</td><td>储存时的环境温度</td><td>-55</td><td>125</td><td>℃</td></tr>
    <tr><td>VDD33</td><td>内部工作电源电压(VDD33接电源，GND 接地)</td><td>-0.4</td><td>3.6</td><td>V</td></tr>
    <tr><td>VIN5</td><td>外部输入电源电压(VIN5接电源，GND 接地)</td><td>-0.4</td><td>5.6</td><td>V</td></tr>
    <tr><td>VIO5</td><td>支持5V 耐压的输入或者输出引脚上的电压</td><td>-0.4</td><td>VIN5+0.4</td><td>V</td></tr>
    <tr><td>VIO3</td><td>不支持5V耐压的输入或者输出引脚上的电压</td><td>-0.4</td><td>VDD33+0.4</td><td>V</td></tr>
</table>

## 18.2 电气参数(测试条件：TA=25℃，VIN5=5V或3.3V，VDD33=3.3V，Fsys=12MHz)
<table>
    <tr>
        <th>名称</th><th colspan="2">参数说明</th><th>最小值</th><th>典型值</th><th>最大值</th><th>单位</th>
    </tr>
    <tr><td>VDD33</td><td colspan="2">VDD33引脚内部工作电源电压</td><td>2.85</td><td>3.3</td><td>3.6</td><td>V</td></tr>
    <tr><td rowspan="2">VIN5</td><td rowspan="2">VIN5引脚外部输入电源电压</td><td>VDD33仅外接电容</td><td>3.6</td><td>5</td><td>5.5</td><td>V</td></tr>
    <tr><td>VDD33短接到VIN5</td><td>2.85</td><td>3.3</td><td>3.6</td><td>V</td></tr>
    <tr><td>ICC</td><td colspan="2">工作时的总电源电流</td><td>4</td><td>8</td><td>50</td><td>mA</td></tr>
    <tr><td>ISLP</td><td colspan="2">完全睡眠后的总电源电流</td><td></td><td>0.1</td><td>0.2</td><td>mA</td></tr>
    <tr><td>VIL</td><td colspan="2">低电平输入电压</td><td>-0.4</td><td></td><td>0.8</td><td>V</td></tr>
    <tr><td>VIH</td><td colspan="2">高电平输入电压</td><td>2.0</td><td></td><td>VDD33+0.4</td><td>V</td></tr>
    <tr><td>VOL</td><td colspan="2">低电平输出电压(4mA 吸入电流)</td><td></td><td></td><td>0.4</td><td>V</td></tr>
    <tr><td>VOH</td><td colspan="2">高电平输出电压(4mA 输出电流)</td><td>VDD33-0.4</td><td></td><td></td><td>V</td></tr>
    <tr><td>IIN</td><td colspan="2">无上拉电阻输入端的输入电流</td><td>-5</td><td>0</td><td>5</td><td>uA</td></tr>
    <tr><td>IUP</td><td colspan="2">带上拉电阻输入端的输入电流</td><td>20</td><td>40</td><td>80</td><td>uA</td></tr>
    <tr><td>IDN</td><td colspan="2">带下拉电阻输入端的输入电流</td><td>-20</td><td>-40</td><td>-80</td><td>uA</td></tr>
    <tr><td>IUPX</td><td colspan="2">带上拉输入端由低向高翻转时的输入电流</td><td>200</td><td>300</td><td>500</td><td>uA</td></tr>
    <tr><td>Vpot</td><td colspan="2">电源上电复位的门限电压</td><td>2.4</td><td>2.55</td><td>2.7</td><td>V</td></tr>
</table>
备注：所有上拉电流都是拉到VDD33电压，而非VIN5 电压。

## 18.3 时序参数(测试条件：TA=25℃，VIN5=5V或3.3V，VDD33=3.3V，Fsys=12MHz)
<table>
    <tr>
        <th>名称</th><th>参数说明</th><th>最小值</th><th>典型值</th><th>最大值</th><th>单位</th>
    </tr>
    <tr><td>Fxt</td><td>外部晶体频率或者XI 输入时钟频率</td><td>4</td><td>12</td><td>20</td><td>MHz</td></tr>
    <tr><td>Fosc</td><td>经校准后的内部时钟频率</td><td>11.82</td><td>12</td><td>12.18</td><td>MHz</td></tr>
    <tr><td>Fpll</td><td>倍频后的PLL频率</td><td>24</td><td>288</td><td>350</td><td>MHz</td></tr>
    <tr><td rowspan="2">Fusb4x</td><td>使用USB 主机功能时，USB 采样时钟频率</td><td>47.98</td><td>48</td><td>48.02</td><td>MHz</td></tr>
    <tr><td>使用USB 设备功能时，USB 采样时钟频率</td><td>47.04</td><td>48</td><td>48.96</td><td>MHz</td></tr>
    <tr><td rowspan="2">Fsys</td><td>系统主频时钟频率（VDD33>=3V）</td><td>1</td><td>12</td><td>56</td><td>MHz</td></tr>
    <tr><td>系统主频时钟频率（VDD33<3V）</td><td>1</td><td>12</td><td>50</td><td>MHz</td></tr>
    <tr><td>Tpor</td><td>电源上电复位延时</td><td>15</td><td>17</td><td>20</td><td>mS</td></tr>
    <tr><td>Trst</td><td>从RST外部输入有效复位信号的宽度</td><td>70</td><td>100</td><td>200</td><td>nS</td></tr>
    <tr><td>Trdl</td><td>热复位延时</td><td>35</td><td>60</td><td>100</td><td>uS</td></tr>
    <tr><td>Twdc</td><td>看门狗溢出周期/定时周期的计算公式</td><td colspan="4">262144 * ( 0x100 - WDOG_COUNT ) / Fsys</td></tr>
    <tr><td rowspan="2">Tusp</td><td>USB 主机模式下检测USB自动挂起时间</td><td>2</td><td>3</td><td>4</td><td>mS</td></tr>
    <tr><td>USB 设备模式下检测USB自动挂起时间</td><td>4</td><td>5</td><td>6</td><td>mS</td></tr>
    <tr><td>Twak</td><td>芯片睡眠后唤醒完成时间</td><td>1</td><td>40</td><td>100</td><td>uS</td></tr>
</table>
