---
title: 13. UART
type: docs
weight: 13
BookToC: false
---

# 13. 非同期送受信UART

## 13.1 UART概要

CH559チップは、2つの全二重非同期シリアルポートを備えています。UART0とUART1です。UART0は、標準的なMCS51シリアルポートです。データの送受信は、物理的に分離された送信／受信レジスタへのSBUFアクセスによって実現されます。SBUFに書き込まれたデータは送信レジスタにロードされ、SBUFへの読み出し操作は受信バッファレジスタに対応します。

UART1は、以下の機能を持つ拡張非同期シリアルポートです:

1. 16C550の非同期式シリアルポートと互換性があり、拡張されています。
2. 5、6、7、8データビットと1、2ストップビットをサポート。
3. 奇数、偶数、パリティなし、ブランク0、フラグ1などの検証方法に対応。
4. プログラム可能な通信ボーレート、115200bpsをサポートし、最大3Mbpsの通信ボーレートを実現。
5. 独立した送受信バッファと8バイトのFIFO FIFOバッファを内蔵し、4つのFIFOトリガレベルをサポート。
6. MODEMモデムの信号CTS、DSR、RI、DCD、DTR、RTSをサポートし、外部からRS232レベルに変換することができます。
7. TL16C550Cと互換性のあるハードウェアフローコントロール信号CTSとRTSの自動ハンドシェイクと自動伝送レートコントロールをサポートしています。
8. シリアルフレームエラー検出、ブレイクライン間隔検出に対応。
9. SIR赤外線コーデックを内蔵し、2400bps～115200bpsのボーレートでのIrDA赤外線通信に対応。
10. 全二重、半二重のシリアル通信に対応し、RS485を切り替えるためのステータス端子を備えています。
11. 半二重差動トランシーバーを内蔵し、RS485バスと同様のシンプルな長距離複合機通信を直接サポートします。
12. 本機をスレーブとして使用する際のアドレスのプリセットをサポートし、複数台通信時にバス上のデータパケットを自動的に一致させるために使用します。

## 13.2 UARTレジスタ

<div>
    <p align="center">表13.2.1 UART関連レジスタ一覧</p>
</div>

<table>
    <tr>
        <th>名前</th><th>アドレス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>SBUF</td><td> 99h</td><td> UART0データレジスタ</td><td> xxh</td></tr>
    <tr><td>SCON</td><td> 98h</td><td> UART0制御レジスタ</td><td> 00h</td></tr>
    <tr><td>SER1_DLL</td><td> 9Ah</td><td>UART1ボーレートディバイザーラッチ下位バイト</td><td> xxh</td></tr>
    <tr><td>SER1_RBR</td><td> 9Ah</td><td>UART1データ受信バッファレジスタ(読み取り専用)</td><td> xxh</td></tr>
    <tr><td>SER1_THR</td><td> 9Ah</td><td>UART1データ送信ホールドレジスタ(書き込み専用)</td><td> xxh</td></tr>
    <tr><td>SER1_FIFO</td><td> 9Ah</td><td>UART1データFIFOリード/ライトレジスタ</td><td> xxh</td></tr>
    <tr><td>SER1_DIV </td><td>97h </td><td>UART1プリスケーラ除算器レジスタ</td><td> xxh</td></tr>
    <tr><td>SER1_ADDR</td><td> 97h</td><td>UART1バスアドレスプリセットレジスタ</td><td>FFh</td></tr>
    <tr><td>SER1_MSR</td><td> 96h</td><td>UART1 MODEMステータスレジスタ(読み取り専用)</td><td> F0h</td></tr>
    <tr><td>SER1_LSR</td><td> 95h</td><td>UART1ラインステータスレジスタ(読み取り専用)</td><td> 60h</td></tr>
    <tr><td>SER1_MCR</td><td> 94h</td><td>UART1 MODEM制御レジスタ</td><td>00h</td></tr>
    <tr><td>SER1_LCR</td><td> 93h</td><td>UART1ライン制御レジスタ</td><td>00h</td></tr>
    <tr><td>SER1_IIR</td><td> 92h</td><td>UART1割り込み識別レジスタ(読み取り専用)</td><td>01h</td></tr>
    <tr><td>SER1_FCR</td><td> 92h</td><td>UART1 FIFO制御レジスタ(書き込み専用)</td><td>00h</td></tr>
    <tr><td>SER1_DLM</td><td> 91h</td><td>UART1ボーレートディバイザラッチ上位バイト</td><td>80h</td></tr>
    <tr><td>SER1_IER</td><td> 91h</td><td>UART1割り込み許可レジスタ</td><td>00h</td></tr>
    
</table>

### 13.2.1 UART0レジスタ概要

### UART0制御レジスタ(SCON):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>SM0</td><td>RW</td><td>UART0作業モード選択ビット0。<br />0: 8ビットデータの非同期通信が選択。<br />1: 9ビットデータの非同期通信が選択。</td><td>0</td></tr>
    <tr><td>6</td><td>SM1</td><td>RW</td><td>UART0作業モード選択ビット1。<br />0: 固定ボーレートを設定。<br />1: T1またはT2によって生成される可変ボーレートを設定。</td><td>0</td></tr>
    <tr><td>5</td><td>SM2</td><td>RW</td><td><p>UART0マルチマシン通信制御ビットです。<br /><br />モード2および3でデータを受信する際、SM2=1のとき、RB8が0ならば、RIが1に設定されていないので、受信は無効です。RB8が1の場合、RIが1に設定されていれば、受信は有効です。<br /><br />SM2＝0のとき、RB8が0または1の場合、データ受信時にRIが設定され、受信は有効となります。モード1では、SM2＝1の場合、有効なストップビットを受信したときのみ、受信が有効になります。モード0では、SM2ビットを0に設定する必要があります</td><td>0</td></tr>
    <tr><td>4</td><td>REN</td><td>RW</td><td>UART0 許可受信制御ビットです。<br />0: 受信無効。<br />1: 受信可能。</td><td>0</td></tr>
    <tr><td>3</td><td>TB8</td><td>RW</td><td>送信データの9ビット目です。モード2および3では、TB8は送信データの9ビット目を書き込むために使用され、パリティビットになることがあります。マルチマシン通信では、ホストがアドレスバイトを送信するかどうかを示すために使用されます。 データバイト、TB8=0はデータ、TB8=1はアドレスです。</td><td>0</td></tr>
    <tr><td>2</td><td>RB8</td><td>RW</td><td>受信データの9ビット目、モード2と3では、RB8を使って受信データの9ビット目を格納します。モード1では、SM2=0の場合、RB8を使用して受信したストップビットを格納します。モード0では、RB8を使用せず。</td><td>0</td></tr>
    <tr><td>1</td><td>TI</td><td>RW</td><td>送信割り込みフラグビット。データバイトが送信された後にハードウェアによって設定され、ソフトウェアによってクリアされる必要があります。</td><td>0</td></tr>
    <tr><td>0</td><td>RI</td><td>RW</td><td>受信割り込みフラグビット。データバイトを受信した後にハードウェアによって設定され、ソフトウェアによってクリアされる必要があります。</td><td>0</td></tr>
    
</table>

<div>
    <p align="center">表13.2.1.1 UART0作業モード選択</p>
</div>

<table>
    <tr>
        <th>SM0</th><th>SM1</th><th>備考</th>
    </tr>
    <tr><td>0</td><td> 0</td><td>モード0。シフトレジスタモード、固定ボーレートはFsys / 12</td></tr>
    <tr><td>0</td><td> 1</td><td>モード1。8ビットの非同期通信モード。可変ボーレート、タイマーT1またはT2で生成されます。</td></tr>
    <tr><td>1</td><td> 0</td><td>モード2。9ビットの非同期通信モード。ボーレートは、Fsys / 128(SMOD = 0)またはFsys / 32(SMOD = 1)です。</td></tr>
    <tr><td>1</td><td> 1</td><td>モード3。9ビット非同期通信。可変ボーレート。タイマーT1またはT2で生成。</td></tr>
    
</table>

モード1および3では、RCLK = 0およびTCLK = 0の場合、UART0のボーレートはタイマーT1によって生成されます。T1はモード2のオートリロード8ビットタイマーモードに設定し、bT1_CTとbT1_GATEは共に0でなければならず、以下のタイプのクロック状況に分けられます。

<div>
    <p align="center">表13.2.1.2 T1で生成されるUART0ボーレートの計算式</p>
</div>

<table>
    <tr>
        <th>bTMR_CLK</th><th>bT1_CLK</th><th>SMOD</th><th>備考</th>
    </tr>
    <tr><td>1</td><td> 1</td><td> 0</td><td> TH1 = 256-Fsys/32/ボーレート</td></tr>
    <tr><td>1</td><td> 1</td><td> 1</td><td> TH1 = 256-Fsys/16/ボーレート</td></tr>
    <tr><td>0</td><td> 1</td><td> 0</td><td> TH1 = 256-Fsys/4/32/ボーレート</td></tr>
    <tr><td>0</td><td> 1</td><td> 1</td><td> TH1 = 256-Fsys/4/16/ボーレート</td></tr>
    <tr><td>X</td><td> 0</td><td> 0</td><td> TH1 = 256-Fsys/12/32/ボーレート</td></tr>
    <tr><td>X</td><td> 0</td><td> 1</td><td> TH1 = 256-Fsys/12/16/ボーレート</td></tr>
    
</table>

モード1および3では、RCLK = 1またはTCLK = 1の場合、UART0のボーレートはタイマーT2によって生成されます。T2は16ビットの自動再搬送レート生成モードに設定し、C_T2とCP_RL2はともに0でなければならず、以下のタイプのクロック状況に分けられます。

<div>
    <p align="center">表13.2.1.3 T2で生成されるUART0ボーレートの計算式</p>
</div>

<table>
    <tr>
        <th>bTMR_CLK</th><th>bT2_CLK</th><th>備考</th>
    </tr>
    <tr><td>1</td><td> 1</td><td> RCAP2 = 65536-Fsys/16/ボーレート</td></tr>
    <tr><td>0</td><td> 1</td><td> RCAP2 = 65536-Fsys/2/16/ボーレート</td></tr>
    <tr><td>X</td><td> 0</td><td> RCAP2 = 65536-Fsys/4/16/ボーレート</td></tr>
    
</table>

### UART0データレジスタ(SBUF):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>SBUF</td><td>RW</td><td>UART0のデータレジスタで、物理的に分離した2つのレジスタの送信と受信を含む。SBUFへのデータ書き込みは、送信データレジスタに対応します。SBUFからのデータの読み出しは、受信データレジスタに対応します。</td><td>xxh</td></tr>
    
</table>

### 13.2.2 UART1関連レジスタ

UART1データFIFOリード＆ライトレジスタSER1_FIFOは、データ受信バッファレジスタSER1_RBRとデータ送信ホールディングレジスタSER1_THRの2つの物理的に独立したレジスタを含む。

### データ受信バッファレジスタ(SER1_RBR), bLCR_DLAB = 0の場合:

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>SER1_RBR</td><td>RO</td><td>シリアルポート受信バッファレジスタ。SER1_LSRのbLSR_DATA_RDYビットが1であれば、このレジスタから受信データを読み出すことができる。bFCR_FIFO_ENが1の場合、シリアルポートのシフトレジスタから受信したデータは、まず受信FIFOに格納されてから、このレジスタのリードに渡される。</td><td>xxh</td></tr>
    
</table>

### データ送信用ホールドレジスタ(SER1_THR), bLCR_DLAB = 0の場合:

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>SER1_THR</td><td>WO</td><td>送信FIFOを含むシリアルポートの送信ホールドレジスタは、送信するデータの書き込みに使用されます。bFCR_FIFO_ENが1の場合、書き込まれたデータは、まず送信FIFOに格納され、その後送信シフトレジスタを介して1つずつ出力される。</td><td>xxh</td></tr>
    
</table>

### 割り込み許可レジスタ(SER1_IER), bLCR_DLAB = 0の場合:

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td> bIER_RESET </td><td>RW</td><td>シリアルポートソフトウェアリセット制御ビット。<br />1: シリアルポートがリセットされます。<br />このビットは、ソフトウェアクリアなしで自動的にクリアすることができます。</td><td> 0</td></tr>
    <tr><td>6</td><td> bIER_EN_MODEM_O </td><td>RW</td><td>UART1モデム信号出力許可ビットです。<br />1: MODEM信号RTS/DTRの出力が有効になります。<br />0: 出力は無効となります。</td><td> 0</td></tr>
    <tr><td>5</td><td> bIER_PIN_MOD1 </td><td>RW</td><td>UART1ピンモード選択上位</td><td> 0</td></tr>
    <tr><td>4</td><td> bIER_PIN_MOD0 </td><td>RW</td><td>UART1ピンモード選択下位</td><td> 0</td></tr>
    <tr><td>3</td><td> bIER_MODEM_CHG </td><td>RW</td><td>モデム入力状態変化割り込み許可ビット。<br />1: モデム入力状態変化割り込みを有効にする。<br />0: 無効にする。</td><td> 0</td></tr>
    <tr><td>2</td><td> bIER_LINE_STAT </td><td>RW</td><td>受信ラインステータス割り込み許可ビット。<br />1: 受信ラインステータス割り込みが有効になります。<br />0: 無効になります。</td><td> 0</td></tr>
    <tr><td>1</td><td> bIER_THR_EMPTY </td><td>RW</td><td>送信保持レジスタ空割り込み許可ビット。<br />1: 送信保持レジスタ空割り込みが有効になります。<br/>0: 無効になります。</td><td> 0</td></tr>
    <tr><td>0</td><td> bIER_RECV_RDY </td><td>RW</td><td>受信データ割り込み許可ビット。<br />1: 受信データ完了割り込みとそれに続く受信データのタイムアウト割り込みの発生が有効になります。<br />0: 無効になります。</td><td> 0</td></tr>
    
</table>

UART1のピンモードは、bIER_PIN_MOD1とbIER_PIN_MOD0の両方と、bUH1_DISABLE、bXBUS_CS_OE、bXBUS_AL_OE、bALE_CLK_ENを組み合わせて、さまざまな構成を選択できます。これらのうち，最後の4つはRS485ENにまとめることができます。:

<div>
    <p align="center">RS485EN = bUH1_DISABLE & ~ ( bXBUS_CS_OE & ~ bXBUS_AL_OE | bALE_CLK_EN )</p>
</div>

<table>
    <tr>
        <th>RS485EN</th><th>bIER_PIN_MOD1</th><th>bIER_PIN_MOD0</th><th>備考</th>
    </tr>
    <tr><td>x</td><td> 0</td><td> 0</td><td>RXD1: P4.0/RXD1_<br />TXD1: 無効</td></tr>
    <tr><td>0</td><td> 1</td><td> 0</td><td>RXD1: P2.6<br />TXD1: P2.7</td></tr>
    <tr><td>0</td><td> 0</td><td> 1</td><td>RXD1: P4.0/RXD1_<br />TXD1: P4.4/TXD1_</td></tr>
    <tr><td>0</td><td> 1</td><td> 1</td><td>RXD1: P2.6<br />TXD1: P2.7<br />TNOW: P2.5</td></tr>
    <tr><td>1</td><td> 1</td><td> 0</td><td>RXD1: iRS485 XAと共有<br />TXD1: iRS485のXBと共有</td></tr>
    <tr><td>1</td><td> 0</td><td> 1</td><td>RXD1: iRS485 XAと共有<br />TXD1: iRS485のXBと共有<br />TNOW: P4.4</td></tr>
    <tr><td>1</td><td> 1</td><td> 1</td><td>RXD1: iRS485 XAと共有<br />TXD1: iRS485のXBと共有<br />TNOW: P2.5</td></tr>
    
</table>

上の表の最後の3つの構成は、iRS485の半二重通信モードです。この時、RS485EN=1、RXD1とTXD1はiRS485の差動ピンXAとXBを併用しています。内蔵の半二重差動トランシーバーを介して、それは直接バスのシンプルなRS485長距離複数のマシンの通信をサポートしています。

iRS485の半二重通信モードでは、以下のパラメータを設定する必要があります:

1. SER1_MCRのbMCR_HALFを1に設定し、半二重トランシーバモードとする。
2. UHUB1_CTRLのbUH1_DISABLEを1に設定することで，HP / HM端子を無効にすることができる。


### 割り込み識別レジスタ(SER1_IIR):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>[7:6]</td><td>MASK_U1_IIR_ID</td><td>R0</td><td>FIFO許可フラグ、11はFIFOが有効であることを意味する</td><td>00b</td></tr>
    <tr><td>[5:4]</td><td>reserved</td><td>R0</td><td>予約</td><td>00b</td></tr>
    <tr><td>[3:0]</td><td>MASK_U1_IIR_INT</td><td>R0</td><td>UART1割り込みステータスフラグ</td><td>0001b</td></tr>
    <tr><td>0</td><td>bIIR_NO_INT</td><td>R0</td><td>UART1は割り込みフラグで、割り込みがないと1になります。0は割込みあり</td><td>1</td></tr>
    
</table>

UART1の割り込み状態は、bIIR_INT_FLAG3、bIIR_INT_FLAG2、bIIR_INT_FLAG1、bIIR_INT_FLAG0の4ビットで構成されています。MASK_U1_IIR_INTは、UART1のシリアルポート割り込みフラグとして使用されます。具体的な割り込みの内容は次の表のとおりです。

<table>
    <tr>
        <th>名前</th><th>アドレス</th><th>割り込み種類</th><th>割り込みソース</th><th>割り込みクリア方法</th>
    </tr>
    <tr><td>U1_INT_SLV_ADDR</td><td>0Eh</td><td>バスアドレス一致</td><td>受信1データはシリアルバスアドレスで、アドレスはプリセット値またはブロードキャストアドレスと一致する</td><td>SER1_IIRの読み出しまたはマルチマシンモードの無効化</td></tr>
    <tr><td>U1_INT_LINE_STAT</td><td>06h</td><td>受信ラインの状態</td><td>bLSR_OVER_ERRまたはbLSR_PAR_ERRまたはbLSR_FRAME_ERRまたはbLSR_BREAK_ERR</td><td>SER1_LSR読み出し</td></tr>
    <tr><td>U1_INT_RECV_RDY</td><td>04h</td><td>受信データあり</td><td>受信したバイト数がFIFOのトリガーポイントに達した場合</td><td>SER1_RBR読み出し</td></tr>
    <tr><td>U1_INT_RECV_TOUT</td><td>0Ch</td><td>受信データタイムアウト</td><td>データを受信したが、次のデータを4データバイト以上受信していない場合</td><td>SER1_RBR読み出し</td></tr>
    <tr><td>U1_INT_THR_EMPTY</td><td>02h</td><td>SER1_THRレジスタが空</td><td>送信ホールディングレジスタが空になり、bIER_THR_EMPTYが0から1に変化して、割り込みが再び有効になる。</td><td>SER1_IIR読み出しかSER1_THR書き込み</td></tr>
    <tr><td>U1_INT_MODEM_CHG</td><td>00h</td><td>MODEMの入力変化</td><td>△CTSまたは△DSRまたは△RIまたは△DCD</td><td>SER1_MSR読み出し</td></tr>
    <tr><td>U1_INT_NO_INTER</td><td>01h</td><td>割り込み無し</td><td>割り込みがない場合</td><td></td></tr>

    
</table>

### FIFO制御レジスタ(SER1_FCR):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bFCR_FIFO_TRIG1</td><td>W0</td><td>受信FIFO割り込みとハードウェアフローコントロールのトリガーポイントがHIGHに設定される</td><td>0</td></tr>
    <tr><td>6</td><td>bFCR_FIFO_TRIG0</td><td>W0</td><td>受信FIFO割り込みとハードウェアフローコントロールのトリガーポイントがLOWに設定される</td><td>0</td></tr>
    <tr><td>[5:3]</td><td>reserved</td><td>R0</td><td>予約</td><td>000b</td></tr>
    <tr><td>2</td><td>bFCR_T_FIFO_CLR</td><td>W0</td><td>送信FIFOデータクリアイ許可ビット。このビットが1の場合、送信FIFO内のデータ（送信中のデータを除く）がクリアされます。このビットは、ソフトウェアでクリアしなくても自動的にクリアできます。</td><td>0</td></tr>
    <tr><td>1</td><td>bFCR_R_FIFO_CLR</td><td>W0</td><td>受信FIFOデータクリア許可ビット。このビットが1の場合、受信FIFO内のデータ（受信中のデータを除く）がクリアされます。このビットは、ソフトウェアでクリアしなくても自動的にクリアできます。</td><td>0</td></tr>
    <tr><td>0</td><td>bFCR_FIFO_EN</td><td>W0</td><td>FIFO許可ビット。<br />1: FIFOが有効になります。<br />0: FIFOは無効になります。<br />FIFOをディセーブルにすると、16C450互換モードとなり、1バイトの深さしかないFIFOと同等になります。FIFOを有効にすることをお勧めします。</td><td>0</td></tr>
    
</table>

bFCR_FIFO_TRIG1とbFCR_FIFO_TRIG0は、MASK_U1_FIFO_TRIGを構成し、受信FIFOの割り込みポイントとハードウェアフローコントロールのトリガーポイントを設定するために使用されます。<br />11: 7バイトに対応しています。つまり、7バイトフルに受信するとデータ受信可能な割り込みが発生します。=1の場合、RTS端子のレベルは自動的に無効になります。<br />10: 4バイトに対応しています。<br />01: 2バイトに対応しています。<br />00: 1バイトに対応しています。

### ライン制御レジスタ(SER1_LCR):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bLCR_DLAB</td><td>RW</td><td>ボーレートディバイザラッチアクセス許可ビット。<br />0: レジスタSER1_RBR, SER1_THR, SER1_IER, SER1_ADRへのアクセスが有効となる。<br />1: レジスタSER1_DLL, SER1_DLM, SER1_DIVへのアクセスが可能となる。</td><td>0</td></tr>
    <tr><td>6</td><td>bLCR_BREAK_EN</td><td>RW</td><td>強制BREAKライン間隔許可ビット。<br />0: BREAK出力は発生しません。<br />1: 強制的にBREAK出力を行います。</td><td>0</td></tr>
    <tr><td>5</td><td>bLCR_PAR_MOD1</td><td>RW</td><td>ハイパリティモード</td><td>0</td></tr>
    <tr><td>4</td><td>bLCR_PAR_MOD0</td><td>RW</td><td>ローパリティモード</td><td>0</td></tr>
    <tr><td>3</td><td>bLCR_PAR_EN</td><td>RW</td><td>パリティ許可ビット。<br />0: パリティビットはありません。<br />1: 送信時にパリティチェックビットを生成し、受信時にパリティチェックビットを受信することができます。</td><td>0</td></tr>
    <tr><td>2</td><td>bLCR_STOP_BIT</td><td>RW</td><td>ストップビットフォーマット設定ビット。<br />0: ストップビットが1つあります。<br />1: ストップビットが2つあります。</td><td>0</td></tr>
    <tr><td>1</td><td>bLCR_WORD_SZ1</td><td>RW</td><td>データワード長セット上位</td><td>0</td></tr>
    <tr><td>0</td><td>bLCR_WORD_SZ0</td><td>RW</td><td>データワード長セット下位</td><td>0</td></tr>
    
</table>

bLCR_PAR_MOD1とbLCR_PAR_MOD0の組み合わせにより、bLCR_PAR_ENが1の時のパリティビットのフォーマットを設定する。<br />00: 奇数パリティ。<br />01: 偶数パリティ。<br />10: フラグビット(MARKを1に設定)。<br />11: ブランクビット(SPACEを0にクリア)。

bLCR_WORD_SZ1とbLCR_WORD_SZ0の組み合わせで、パリティなしの1つのデータのワード長を設定します。<br />00: 5データビット。<br />01: 6データビット。<br />10: 7データビット。<br />11: 8データビット。

### MODEM制御レジスタ(SER1_MCR):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bMCR_HALF</td><td>RW</td><td>半二重送受信モード許可ビット。<br />0: 半二重送受信モードが無効となり、全二重をサポートします。<br />1: 自動半二重送受信モードになります。送信が優先されます。送信中は受信が中断されます。</td><td>0</td></tr>
    <tr><td>6</td><td>bMCR_TNOW</td><td>RW</td><td>RTSピン機能選択ビット。<br />0: 標準的なRTS出力となります。<br />1: TNOW出力となります。出力の状態は、送信中です。<br />このビットは、RS485の半二重モードの制御に使用できます。</td><td>0</td></tr>
    <tr><td>5</td><td>bMCR_AUTO_FLOW</td><td>RW</td><td>CTSとRTSのハードウェア自動フロー制御許可ビット。<br />0: ハードウェア自動フロー制御が無効になります。<br />1: ハードウェア自動フロー制御が有効になります。<br />ハードウェアフロー制御が有効になった後、シリアルポートは、CTSピン入力がアクティブLOWの場合のみ次のデータの送信を継続し、それ以外の場合はシリアルポートの送信を中断します。ハードウェアフロー制御が有効になった後、bMCR_RTSが1の場合、受信FIFOが空になると、シリアルポートは自動的に低レベルのRTSピンを作動させます。受信したバイト数がFIFOのトリガーポイントに達すると、シリアルポートは自動的にRTSピンを無効にし、受信FIFOが空になると再びRTSピンを有効にすることができるようになります。ハードウェアフロー制御を有効にすると、CTSAの入力状態の変化はMODEMの状態割り込みを生成しません。自身のCTSピンを相手のRTSピンに接続し、自身のRTSピンを相手のCTSピンに送ることで、ハードウェアによる自動レートコントロールを実現することができます。</td><td>0</td></tr>
    <tr><td>4</td><td>bMCR_LOOP</td><td>RW</td><td>内部ループのテストモード許可ビットです。<br />0: 内部ループテストは無効となります。<br />1: 内部ループテストが有効になります。<br />内部ループテストモードでは、シリアルポートのすべての外部出力ピンが非アクティブになります。TXD1は内部的にRXD1に、RTSは内部的にCTSに、DTRは内部的にDSRに、OUT1は内部的にRIに、OUT2は内部的にDCDに戻ります。</td><td>0</td></tr>
    <tr><td>3</td><td>bMCR_OUT2</td><td>RW</td><td>シリアルポート割り込み要求出力許可ビット。<br />0: シリアルポート割り込み要求出力は無効になります。<br />1: シリアルポート割り込み要求出力が有効になります。</td><td>0</td></tr>
    <tr><td>2</td><td>bMCR_OUT1</td><td>RW</td><td>ユーザー定義のMODEM制御ビット。実際の出力端子は接続されていません。内部ループテスト、または汎用データビットとして使用します。</td><td>0</td></tr>
    <tr><td>1</td><td>bMCR_RTS</td><td>RW</td><td>RTSピン出力制御ビット。<br />0: RTSピン出力は無効(ハイレベル)となります。<br />1: RTSピン出力は有効(ローレベル)になります。</td><td>0</td></tr>
    <tr><td>0</td><td>bMCR_DTR</td><td>RW</td><td>DTRピン出力制御ビット。<br />0: DTRピン出力が無効（ハイレベル）となります。<br />1: DTRピン出力は有効（ローレベル）となります。</td><td>0</td></tr>
    
</table>

### ラインステータスレジスタ(SER1_LSR):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bLSR_ERR_R_FIFO</td><td>R0</td><td>受信FIFOのエラーフラグ。<br />1: 受信FIFOにbLSR_PAR_ERR、bLSR_FRAME_ERR、bLSR_BREAK_ERRのいずれかのエラーが少なくとも1つのあることを示します。</td><td>0</td></tr>
    <tr><td>6</td><td>bLSR_T_ALL_EMP</td><td>R0</td><td>送信関連レジスタフルエンプティフラグビット。<br />1: 送信ホールディングレジスタSER1_THR、FIFO、送信シフトレジスタが空であることを意味します。</td><td>1</td></tr>
    <tr><td>5</td><td>bLSR_T_FIFO_EMP</td><td>R0</td><td>1: 送信ホールディングレジスタSER1_THRおよびFIFOが空であることを示します。</td><td>1</td></tr>
    <tr><td>4</td><td>bLSR_BREAK_ERR</td><td>R0</td><td>1: BREAKラインのインターバル状態が検出されたことを示します。</td><td>0</td></tr>
    <tr><td>3</td><td>bLSR_FRAME_ERR</td><td>R0</td><td>1: 受信FIFOの現在のデータがフレーミングされており、有効なストップビットがないことを示します。</td><td>0</td></tr>
    <tr><td>2</td><td>bLSR_PAR_ERR</td><td>R0</td><td>1: 受信FIFO内の現在のデータのパリティエラーを示します。</td><td>0</td></tr>
    <tr><td>1</td><td>bLSR_OVER_ERR</td><td>R0</td><td>1: 受信FIFOバッファのオーバーフローを示します。</td><td>0</td></tr>
    <tr><td>0</td><td>bLSR_DATA_RDY</td><td>R0</td><td>1: 受信FIFOに受信データがあることを示します。<br />FIFO内のすべてのデータを読み出した後、このビットは自動的に0にクリアされます。</td><td>0</td></tr>
    
</table>

### MODEMステータスレジスタ(SER1_MSR):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bMSR_DCD</td><td>R0</td><td>DCD端子のビット反転。<br />1: DCD端子が有効(アクティブLOW)であることを示します。</td><td>1</td></tr>
    <tr><td>6</td><td>bMSR_RI</td><td>R0</td><td>RI端子のビット反転。<br />1: RIピンが有効(アクティブLOW)であることを示す。</td><td>1</td></tr>
    <tr><td>5</td><td>bMSR_DSR</td><td>R0</td><td>DSR端子のビット反転。 <br />1: DSR端子が有効(アクティLOW)であることを示します。</td><td>1</td></tr>
    <tr><td>4</td><td>bMSR_CTS</td><td>R0</td><td>CTSピンのビット反転。<br />1: CTSピンが有効(アクティブLOW)であることを示します。</td><td>1</td></tr>
    <tr><td>3</td><td>bMSR_DCD_CHG</td><td>R0</td><td>1: DCDピンの入力状態が変化したことを示します。</td><td>0</td></tr>
    <tr><td>2</td><td>bMSR_RI_CHG</td><td>R0</td><td>1: RIピンの入力状態が変化したことを示します。</td><td>0</td></tr>
    <tr><td>1</td><td>bMSR_DSR_CHG</td><td>R0</td><td>1: DSRピンの入力状態が変化したことを示します。</td><td>0</td></tr>
    <tr><td>0</td><td>bMSR_CTS_CHG</td><td>R0</td><td>1: CTSピンの入力状態が変化したことを示します。</td><td>0</td></tr>
    
</table>

### UART1バスアドレスプリセットレジスタ(SER1_ADDR), bLCR_DLAB = 0の時:

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>SER1_ADDR</td><td>RW</td><td>複数のマシンの通信における自動比較のためのプリセットバスアドレス</td><td>FFh</td></tr>
    
</table>

SER1_ADDRは、本機がスレーブとして使用される際のアドレスをプリセットする。複数台通信時に受信アドレスを自動的に比較し、アドレスが一致した場合やブロードキャストアドレス0FFHを受信した場合に割り込みを発生させ、同時に後続のデータパケットの受信を許可するために使用する。アドレスが一致しないうちは、データを受信しないでください。データの送信を開始した後、またはSER1_ADDRレジスタを書き換えた後は、次にアドレスが再び一致するかブロードキャストアドレスを受信するまでデータの受信を停止する。

SER1_ADDRが0FFHまたはbLCR_PAR_EN=0の場合、バスアドレスの自動比較は無効となる。

SER1_ADDRが0FFHではなく、bLCR_PAR_EN = 1の場合、バスアドレスの自動比較が有効となり、以下のパラメータを設定する必要がある。bLCR_WORD_SZ1とbLCR_WORD_SZ0は共に1で、8データビットを選択する。BLCR_PAR_MOD1は常に1です。アドレスバイトがMARKの場合 データバイトのビット9が0の場合は、bLCR_PAR_MOD0を1に設定してください。アドレスバイトがSPACEの場合(データバイトのビット9が1の場合)は、bLCR_PAR_MOD0を0にして、データバイトに応じて選択します。

### UART1ボーレートディバイザラッチ(SER1_DLM, SER1_DLL), bLCR_DLAB = 1の時:

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>SER1_DLL</td><td>RW</td><td rowspan="2">SER1_DLLは下位バイト、SER1_DLMは上位バイトである。この2つは16ビットの除算器を形成し、16ビットカウンタで構成されるシリアルボーレートジェネレータに使用される。これらのレジスタはbLCR_DLABが1の時のみ読み書き可能である。<br />除算器 = Fsys * 2 / SER1_DIV / 16 / ボーレート</td><td>xxh</td></tr>
    <tr><td>[7:0]</td><td>SER1_DLM</td><td>RW</td><td>80h</td></tr>
    
</table>

### UART1プリスケーラディバイザレジスタ(SER1_DIV), bLCR_DLAB = 1の時:

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>SER1_DIV</td><td>RW</td><td>システムのメインクロックFsysを乗算した後、シリアルポートのボーレートジェネレータの内部基準クロックを生成するために前置分周するのに使用されます。このレジスタは、bLCR_DLABが1のときのみ読み書きできる。</td><td>xxh</td></tr>
    
</table>

## 13.3 UARTアプリケーション

### UART0アプリケーション:

1. UART0のボーレートジェネレータを選択します。タイマーT1またはT2から選択し、対応するカウンタを設定します。
2. タイマをスタート
3. SCONのSM0、SM1、SM2を設定して、シリアルポート0の動作モードを選択します。RENを1に設定して、UART0の受信を有効にします。
4. シリアルポートの割り込みを設定したり、RI、TIの割り込み状態を問い合わせたりすることができます。
5. 読み書き可能なSBUFは、シリアルデータの送受信を実装しており、シリアルポートの受信信号の許容ボーレート誤差は2%以下です。

### UART1アプリケーション:

1. SER1_LCRのbLCR_DLABビットを1に設定する。UART1 プリスケーラレジスタSER1_DIVを書き込む。ボーレートに応じてボーレート除算器を計算し、除算器の上位バイトと下位バイトをそれぞれSER1_DLMとSER1_DLLに書き込む。<br />除算器 = Fsys/8/STER1_DIV/ボーレート
2. SER1_LCRを設定し、適切なシリアルデータフォーマット、データバイト、パリティモードを選択します。
3. オプション設定SER1_IER、UART1割り込みステータストリガを選択。
4. 割り込みモードを使用する場合、割り込み出力を有効にするにはSER1_MCRのbMCR_OUT2ビットを1に設定する必要があります。そうでない場合は、割り込みステータスビットをアクティブにクエリーする必要があります。
5. SER1_FIFOを読み書きすることでシリアルデータの送受信を実現し、シリアルポート受信信号の許容ボーレート誤差は2%以下です。
