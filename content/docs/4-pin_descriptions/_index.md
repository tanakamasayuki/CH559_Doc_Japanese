---
title: 4. ピン概要
type: docs
weight: 4
BookToC: false
---

# 8ビット拡張USBマイクロコントローラCH559

## 4. ピン概要

### CH559L/T ピン定義

<table>
    <tr>
        <th colspan="2">ピン番号</th>
        <th rowspan="2">ピン名<br>(リセット後)</th>
        <th rowspan="2">機能</th>
        <th rowspan="2">機能の説明</th>
    </tr>
    <tr>
        <th>SSOP-20</th>
        <th>LQFP-48</th>
    </tr>
    <tr>
        <td>19</td><td>41</td><td>VIN5</td><td>V5</td><td>5V外部電源入力には、5Vから3.3V電圧レギュレータが内蔵。外付けの0.1uFのバイパスコンデンサが必要です</td>
    </tr>
    <tr>
        <td>20</td><td>42</td><td>VDD33</td><td>VDD/VCC</td><td>内部電圧レギュレータ出力と3.3V動作電源入力。電源電圧が3.6V未満の場合は、VIN5を入力外部電源に接続してください。電源電圧が3.6V以上の場合は、3.3uFのバイパスコンデンサを接続してください</td>
    </tr>
    <tr>
        <td>18</td><td>18</td><td>GND</td><td>VSS</td><td>グランド端子</td>
    </tr>
    <tr>
        <td>--</td><td>40</td><td>P0.0</td><td>AD0/UDTR</td><td rowspan="8"><p>P0ポート: デフォルトは8ビットのオープンドレイン入出力ポート。P0_PUを設定することで内部のプルアップ抵抗を有効にできる。</p><p>P0は外部バスへのアクセス時には入出力データバス AD0～AD7として一時的にプッシュプル出力に切り替わり、多重化アドレスモードで外部バスへのアクセス時には必要に応じてアドレスの下位8ビットを出力。</p><p>UDTR, URTS: UART1の信号出力。</p><p>UCTS, UDSR, URI, UDCD: UART1の信号入力。</p><p>RXD_, TXD_: RXD、TXDのピンマッピング。</p></td>
    </tr>
    <tr>
        <td>--</td><td>39</td><td>P0.1</td><td>AD1/URTS</td>
    </tr>
    <tr>
        <td>17</td><td>38</td><td>P0.2</td><td>AD2/RXD_</td>
    </tr>
    <tr>
        <td>16</td><td>37</td><td>P0.3</td><td>AD3/TXD_</td>
    </tr>
     <tr>
        <td>--</td><td>36</td><td>P0.4</td><td>AD4/UCTS</td>
    </tr>
    <tr>
        <td>--</td><td>35</td><td>P0.5</td><td>AD5/UDSR</td>
    </tr>
    <tr>
        <td>--</td><td>34</td><td>P0.6</td><td>AD6/URI</td>
    </tr>
    <tr>
        <td>--</td><td>33</td><td>P0.7</td><td>AD7/UDCD</td>
    </tr>
    <tr>
        <td>--</td><td>43</td><td>P1.0</td><td>AIN0/T1/CAP1</td><td rowspan="8"><p>AIN0~AIN7: 8チャンネルのADCアナログ信号入力。</p><p>T2: 外部カウント入力/タイマーのクロック出力/カウンタ2。</p><p>T2EX: タイマ/カウンタ2のリロード/キャプチャ入力。</p><p>CAP1, CAP2: タイマーのキャプチャ入力/カウンタ2 1,2。</p><p>CAP3/PWM3: タイマ/イベントカウンタ3のキャプチャ入力/PWM出力。</p><p>SCS, MOSI, MISO, SCK: SPI0インターフェース、SCSはチップセレクト入力、MOSIはマスタ出力/スレーブ入力、MISOはマスタ入力/スレーブ出力、SCKはシリアルクロック。</p></td>
    </tr>
    <tr>
        <td>--</td><td>44</td><td>P1.1</td><td>AIN1/T2EX/CAP2</td>
    </tr>
    <tr>
        <td>1</td><td>45</td><td>P1.2</td><td>AIN2/PWM3/CAP3</td>
    </tr>
    <tr>
        <td>--</td><td>46</td><td>P1.3</td><td>AIN3</td>
    </tr>
    <tr>
        <td>2</td><td>47</td><td>P1.4</td><td>AIN4/SCS</td>
    </tr>
    <tr>
        <td>3</td><td>48</td><td>P1.5</td><td>AIN5/MOSI</td>
    </tr>
    <tr>
        <td>4</td><td>1</td><td>P1.6</td><td>AIN6/MISO</td>
    </tr>
    <tr>
        <td>5</td><td>2</td><td>P1.7</td><td>AIN7/SCK</td>
    </tr>
    <tr>
        <td>--</td><td>21</td><td>P2.0</td><td>A8</td><td rowspan="8"><p>P2が外部バスにアクセスすると、一時的に自動的にプッシュプル出力に切り替わり、必要に応じてアドレスA8～A15の上位8ビットを出力。</p><p>MOSI1, MISO1, SCK1: SPI1インターフェース、MOSI1はマスタ出力、MISO1はマスタ入力、SCK1はシリアルクロック出力。</p><p>PWM1, PWM2: PWM1出力、PWM2出力。</p><p>TNOW: UART1は出力指示を送信。</p><p>T2EX_/CAP2_: T2EX/CAP2のピンマッピング。</p><p>RXD1, TXD1: UART1のシリアルデータ入力、シリアルデータ出力。</p><p>DA7: ダイレクトアドレスモードで外部バスにアクセスする場合の出力アドレスA7。</p></td>
    </tr>
    <tr>
        <td>--</td><td>22</td><td>P2.1</td><td>MOSI1/A9</td>
    </tr>
    <tr>
        <td>--</td><td>23</td><td>P2.2</td><td>MISO1/A10</td>
    </tr>
    <tr>
        <td>--</td><td>24</td><td>P2.3</td><td>SCK1/A11</td>
    </tr>
    <tr>
        <td>--</td><td>25</td><td>P2.4</td><td>PWM1/A12</td>
    </tr>
    <tr>
        <td>11</td><td>26</td><td>P2.5</td><td>TNOW/PWM2/A13/T2EX_/CAP2_</td>
    </tr>
    <tr>
        <td>12</td><td>27</td><td>P2.6</td><td>RXD1/A14</td>
    </tr>
    <tr>
        <td>13</td><td>28</td><td>P2.7</td><td>TXD1/DA7/A15</td>
    </tr>
    <tr>
        <td>--</td><td>4</td><td>P3.0</td><td>RXD</td><td rowspan="8"><p>RXD, TXD: UART0シリアルデータ入力、シリアルデータ出力。</p><p>INT0, INT1: 外部割込み0、外部割込み1入力。</p><p>LED0, LED1, LEDC: LEDシリアルデータ0、1、クロック出力。</p><p>!A15: 外部バスアドレスA15のチップセレクト用反転出力。</p><p>T0, T1: タイマ0、タイマ1の外部入力。</p><p>XCS0: 外部バスアドレス4000h～7FFFhのチップセレクト出力。</p><p>DA6: ダイレクトアドレスモードで外部バスにアクセスする場合の出力アドレスA6。</p><p>WR, RD: 外部バス書き込み信号、読み出し信号。</p></td>
    </tr>
    <tr>
        <td>--</td><td>7</td><td>P3.1</td><td>TXD</td>
    </tr>
    <tr>
        <td>7</td><td>8</td><td>P3.2</td><td>LED0/INT0</td>
    </tr>
    <tr>
        <td>--</td><td>9</td><td>P3.3</td><td>LED1/!A15/INT1</td>
    </tr>
    <tr>
        <td>8</td><td>10</td><td>P3.4</td><td>LEDC/XCS0/T0</td>
    </tr>
    <tr>
        <td>--</td><td>11</td><td>P3.5</td><td>DA6/T1</td>
    </tr>
    <tr>
        <td>--</td><td>12</td><td>P3.6</td><td>WR</td>
    </tr>
    <tr>
        <td>--</td><td>13</td><td>P3.7</td><td>RD</td>
    </tr>
    <tr>
        <td>--</td><td>20</td><td>P4.0</td><td>LED2/A0/RXD1_</td><td rowspan="8"><p>A0~A5: ダイレクトアドレスモードで外部バスにアクセスした時の下位6ビットのアドレスA0～A5を出力。</p><p>LED2, LED3: LEDシリアルデータ2、3を出力。</p><p>RXD1_, TNOW_/TXD1_: RXD1、TNOW/TXD1ピンマッピング。</p><p>PWM3_/CAP3_: PWM3/CAP3ピンマッピング。</p><p>PWM1_, PWM2_: PWM1、PWM2ピンマッピング。</p><p>XI, XO: 外部水晶振動子入力、反転出力端子。</p><p>SCS_, SCK_: SPI0チップセレクトのSCS、SCKピンマッピング。</p></td>
    </tr>
    <tr>
        <td>--</td><td>19</td><td>P4.1</td><td>A1</td>
    </tr>
    <tr>
        <td>--</td><td>15</td><td>P4.2</td><td>PWM3_/CAP3_/A2</td>
    </tr>
    <tr>
        <td>--</td><td>14</td><td>P4.3</td><td>PWM1_/A3</td>
    </tr>
    <tr>
        <td>--</td><td>6</td><td>P4.4</td><td>LED3/TNOW_/TXD1_/A4</td>
    </tr>
    <tr>
        <td>--</td><td>5</td><td>P4.5</td><td>PWM2_/A5</td>
    </tr>
    <tr>
        <td>9</td><td>16</td><td>P4.6</td><td>XI/SCS_</td>
    </tr>
    <tr>
        <td>10</td><td>17</td><td>P4.7</td><td>X0/SCK_</td>
    </tr>
    <tr>
        <td>15</td><td>32</td><td>P5.0</td><td>DM</td><td rowspan="2">DM, DP: USBホストHUB0またはUSBデバイスのD-、D+信号。</td>
    </tr>
    <tr>
        <td>14</td><td>31</td><td>P5.1</td><td>DP</td>
    </tr>
    <tr>
        <td>--</td><td>30</td><td>P5.4</td><td>HM/ALE/XB</td><td rowspan="2"><p>XB, XA: iRS485のB/反転、A/同相信号端子。</p><p>ALE: 多重化アドレスモード時のアドレスラッチ信号出力。</p><p>!A15: 外部バスアドレスA15チップセレクト時の反転出力。</p><p>HM, HP: USBホストHUB1のD-、D+信号。</p></td>
    </tr>
    <tr>
        <td>--</td><td>29</td><td>P5.5</td><td>HP/!A15/XA</td>
    </tr>
    <tr>
        <td>6</td><td>3</td><td>P5.7</td><td>RST</td><td>プルダウン抵抗内蔵の外部リセット入力</td>
    </tr>

</table>