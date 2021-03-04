---
title: 9. 割り込み
type: docs
weight: 9
BookToC: false
---

# 9. 割り込み

CH559チップは、標準MCS51と互換性のある6セットの割り込みを含む14セットの割り込み信号ソースをサポートしています。INT0、T0、INT1、T1、UART0、T2、拡張8セットの割り込みを含む14セットの割り込み信号源をサポートしています。SPI0, TMR3, USB, ADC, UART1, PWM1 GPIO, WDOGの8組の割り込みがあり、そのうちGPIOの割り込みは7つのI/Oピンから選択可能です。

## 9.1 レジスタ概要

<div>
    <p align="center">表9.1.1 割り込みベクトル一覧</p>
</div>

<table>
    <tr>
        <th>割り込みソース</th><th>エントリーアドレス</th><th>割り込み番号</th><th>概要</th><th>デフォルト優先度</th>
    </tr>
    <tr><td>INT_NO_INT0</td><td>0x0003</td><td>0</td><td>外部割込み0またはLED制御カード割込み。<br />bLED_OUT_EN = 0の時は外部割り込み0<br />
bLED_OUT_EN = 1の時はLED制御カード割り込み</td><td rowspan="14">高優先度<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>低優先度</td></tr>
    <tr><td>INT_NO_TMR0</td><td>0x000B</td><td>1</td><td>Timer0割り込み</td></tr>
    <tr><td>INT_NO_INT1</td><td>0x0013</td><td>2</td><td>外部割り込み1</td></tr>
    <tr><td>INT_NO_TMR1</td><td>0x001B</td><td>3</td><td>Timer1割り込み</td></tr>
    <tr><td>INT_NO_UART0</td><td>0x0023</td><td>4</td><td>UART0割り込み</td></tr>
    <tr><td>INT_NO_TMR2</td><td>0x002B</td><td>5</td><td>Timer2割り込み</td></tr>
    <tr><td>INT_NO_SPI0</td><td>0x0033</td><td>6</td><td>SPI0割り込み</td></tr>
    <tr><td>INT_NO_TMR3</td><td>0x003B</td><td>7</td><td>Timer3割り込み</td></tr>
    <tr><td>INT_NO_USB</td><td>0x0043</td><td>8</td><td>USB割り込み</td></tr>
    <tr><td>INT_NO_ADC</td><td>0x004B</td><td>9</td><td>ADC割り込み</td></tr>
    <tr><td>INT_NO_UART1</td><td>0x0053</td><td>10</td><td>UART1割り込み</td></tr>
    <tr><td>INT_NO_PWM1</td><td>0x005B</td><td>11</td><td>PWM1割り込み</td></tr>
    <tr><td>INT_NO_GPIO</td><td>0x0063</td><td>12</td><td>GPIO割り込み</td></tr>
    <tr><td>INT_NO_WDOG</td><td>0x006B</td><td>13</td><td>ウォッチドッグタイマー割り込み</td></tr>
</table>

<div>
    <p align="center">表9.1.2 割り込み関連レジスタ一覧</p>
</div>

<table>
    <tr>
        <th>名前</th><th>アドレス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>IP_EX</td><td>E9h</td><td>拡張割り込み優先制御レジスタ</td><td>00h</td></tr>
    <tr><td>IE_EX</td><td>E8h</td><td>拡張割り込みイネーブルレジスタ</td><td>00h</td></tr>
    <tr><td>GPIO_IE</td><td>CFh</td><td>GPIO割り込みイネーブルレジスタ</td><td>00h</td></tr>
    <tr><td>IP</td><td>B8h</td><td>割り込み優先制御レジスタ</td><td>00h</td></tr>
    <tr><td>IE</td><td>A8h</td><td>割り込みイネーブルレジスタ</td><td>00h</td></tr>
</table>

### 割り込みイネーブルレジスタ(IE):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>EA</td><td>RW</td><td>グローバル割り込みイネーブル制御ビット<br />1: E_DISは0で割り込みを有効にします。<br />0: 全ての割り込み要求をマスクします。</td><td>0</td></tr>
    <tr><td>6</td><td>E_DIS</td><td>RW</td><td>グローバル割り込みディセーブル制御ビット<br />1: 全ての割り込み要求をマスクします。<br />0: EAは1で割り込みを有効にします。<br />このビットは通常、フラッシュROM操作中に一時的に割り込みを無効にするために使用されます。</td><td>0</td></tr>
    <tr><td>5</td><td>ET2</td><td>RW</td><td>Timer2割り込みイネーブルビット<br />1: T2割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    <tr><td>4</td><td>ES</td><td>RW</td><td>UART0割り込みイネーブルビット<br />1: UART0の割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    <tr><td>3</td><td>ET1</td><td>RW</td><td>Timer1割り込みイネーブルビット<br />1: T1割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    <tr><td>2</td><td>EX1</td><td>RW</td><td>外部割込み1イネーブルビット<br />1: INT1割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    <tr><td>1</td><td>ET0</td><td>RW</td><td>Timer0割り込みイネーブルビット<br />1: T0割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    <tr><td>0</td><td>EX0</td><td>RW</td><td>外部割込み0、LED制御カード割込みイネーブルビット<br />1: bLED_OUT_ENで選択されたINT0/LED割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    
</table>

### 拡張割り込みイネーブルレジスタ(IE_EX):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>IE_WDOG</td><td>RW</td><td>ウォッチドッグタイマ割り込みイネーブルビット<br />1: WDOG割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    <tr><td>6</td><td>IE_GPIO</td><td>RW</td><td>GPIO割り込みイネーブルビット<br />1: GPIO_IEで有効な割り込みを有効にします。<br />0: GPIO_IEのすべての割り込みをマスクします。</td><td>0</td></tr>
    <tr><td>5</td><td>IE_PWM1</td><td>RW</td><td>PWM1割り込みイネーブルビット<br />1: PWM1割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    <tr><td>4</td><td>IE_UART1</td><td>RW</td><td>UART1の割り込みイネーブルビット<br />1: UART1の割り込みを有効にします。<br />0: マスク</td><td>0</td></tr>
    <tr><td>3</td><td>IE_ADC</td><td>RW</td><td>ADC割り込みイネーブルビット<br />1: ADC割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    <tr><td>2</td><td>IE_USB</td><td>RW</td><td>USB割り込みイネーブルビット<br />1: USB割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    <tr><td>1</td><td>IE_TMR3</td><td>RW</td><td>Timer3割り込みイネーブルビット<br />1: Timer3の割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    <tr><td>0</td><td>IE_SPI0</td><td>RW</td><td>SPI0割り込みイネーブルビット<br />1: SPI0割り込みを有効にします。<br />0: マスク。</td><td>0</td></tr>
    
</table>

### GPIO割り込みイネーブルレジスタ(GPIO_IE):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bIE_IO_EDGE</td><td>RW</td><td>GPIO エッジ割り込みモードイネーブル:<br /><br />本ビットは0でレベル割り込みモードを選択します。GPIOピン入力が有効レベルの場合、bIO_INT_ACTが1で、常に割り込み要求を行います。GPIO入力が無効レベルの場合は、bIO_INT_ACTが0で割り込み要求をキャンセルします。<br /><br />本ビットは1でエッジ割り込みモードを選択します。GPIO端子に有効なエッジが入力されると、割り込みフラグbIO_INT_ACTが発生して割り込み要求を行います。この割り込みフラグはソフトウェアではクリアできません。レベル割込みモードでリセットするか、対応する割込みサービスルーチンに入るしかありません。</td><td>0</td></tr>
    <tr><td>6</td><td>bIE_RXD1_LO</td><td>RW</td><td>本ビットは 1 で UART1 の受信ピン割り込みを有効にします（レベルモードはアクティブロー、エッジモードは立下りエッジ）。<br />このビットを0にすると無効になります。<br />iRS485モードではXA/XB差動入力を選択し、非iRS485モードではbIER_PIN_MOD1 = 1/0でRXD1またはRXD1_端子を選択します。</td><td>0</td></tr>
    <tr><td>5</td><td>bIE_P5_5_HI</td><td>RW</td><td>本ビットは1で、P5.5割り込み（レベルモードはアクティブHigh、エッジモードは立上りエッジ）を有効にします。<br />無効にするには、このビットを0にします。</td><td>0</td></tr>
    <tr><td>4</td><td>bIE_P1_4_LO</td><td>RW</td><td>本ビットは1で、P1.4割り込み（レベルモードはアクティブLow、エッジモードは立下りエッジでアクティブ）を有効にします。<br />無効にするには、このビットを0にします。</td><td>0</td></tr>
    <tr><td>3</td><td>bIE_P0_3_LO</td><td>RW</td><td>本ビットは1で、P0.3割り込み（レベルモードではローレベル、エッジモードでは立下りエッジで有効）を有効にします。<br />無効にするには、このビットを0にします。</td><td>0</td></tr>
    <tr><td>2</td><td>bIE_P5_7_HI</td><td>RW</td><td>本ビットは1で、P5.7割り込み（レベルモードはアクティブHigh、エッジモードは立上りエッジ）を有効にします。<br />無効にするには、このビットを0にします。</td><td>0</td></tr>
    <tr><td>1</td><td>bIE_P4_1_LO</td><td>RW</td><td>本ビットは1で、P4.1割り込み（レベルモードではローレベル、エッジモードでは立下りエッジで有効）を有効にします。<br />無効にするには、このビットを0にします。</td><td>0</td></tr>
    <tr><td>0</td><td>bIE_RXD0_LO</td><td>RW</td><td>本ビットを1にするとUART0の受信ピン割り込みが有効になります（レベルモードはアクティブロー、エッジモードはアクティブ立下りエッジ）。<br />無効にするには、本ビットを0にします。<br />bUART0_PIN_X = 0/1でRXD0またはRXD0_ピンを選択します。</td><td>0</td></tr>
    
</table>

### 割り込み優先制御レジスタ(IP):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>PH_FLAG</td><td>R0</td><td>高優先度割り込み実行フラグ</td><td>0</td></tr>
    <tr><td>6</td><td>PL_FLAG</td><td>R0</td><td>低優先度割り込み実行フラグ</td><td>0</td></tr>
    <tr><td>5</td><td>PT2</td><td>RW</td><td>Timer2割り込み優先制御ビット</td><td>0</td></tr>
    <tr><td>4</td><td>PS</td><td>RW</td><td>UART0割り込み優先制御ビット</td><td>0</td></tr>
    <tr><td>3</td><td>PT1</td><td>RW</td><td>Timer1割り込み優先制御ビット</td><td>0</td></tr>
    <tr><td>2</td><td>PX1</td><td>RW</td><td>外部割り込み1優先制御ビット</td><td>0</td></tr>
    <tr><td>1</td><td>PT0</td><td>RW</td><td>timer0割り込み優先制御ビット</td><td>0</td></tr>
    <tr><td>0</td><td>PX0</td><td>RW</td><td>LEDコントロールカードおよび外部割込み0割込み優先制御ビット</td><td>0</td></tr>
    
</table>

### 拡張割り込み優先制御レジスタ(IP_EX):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>備考</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bIP_LEVEL</td><td>R0</td><td>現在の割り込みネスティングレベルフラグビットです。<br />このビットが0の場合、割り込みなしまたは入れ子になっているレベル2の割り込みを意味します。<br />このビットが1の場合、現在のネストされたレベル1の割り込みを意味します。</td><td>0</td></tr>
    <tr><td>6</td><td>bIP_GPIO</td><td>RW</td><td>GPIO割り込み優先制御ビット</td><td>0</td></tr>
    <tr><td>5</td><td>bIP_PWM1</td><td>RW</td><td>PWM1割り込み優先制御ビット</td><td>0</td></tr>
    <tr><td>4</td><td>bIP_UART1</td><td>RW</td><td>UART1割り込み優先制御ビット</td><td>0</td></tr>
    <tr><td>3</td><td>bIP_ADC</td><td>RW</td><td>ADC割り込み優先制御ビット</td><td>0</td></tr>
    <tr><td>2</td><td>bIP_USB</td><td>RW</td><td>USB割り込み優先制御ビット</td><td>0</td></tr>
    <tr><td>1</td><td>bIP_TMR3</td><td>RW</td><td>Timer3割り込み優先制御ビット</td><td>0</td></tr>
    <tr><td>0</td><td>bIP_SPI0</td><td>RW</td><td>SPI0割り込み優先制御ビット</td><td>0</td></tr>
</table>

IPレジスタとIP_EXレジスタは、割り込みの優先度を設定するためのレジスタです。ビットが1に設定されている場合は、対応する割り込みソースの優先度が高くなります。0にクリアすると対応する割り込みソースの優先度が低くなります。同レベルの割り込みソースに対しては、デフォルトの優先順位が設定されています。表9.1.1にデフォルトの優先順位を示します。PH_FLAGとPL_FLAGの組み合わせは、現在の割り込みの優先度を示します。

<div>
    <p align="center">表9.1.3 現在の割り込み優先度ステータス表示
</p>
</div>

<table>
    <tr>
        <th>PH_FLAG</th><th>PL_FLAG</th><th>現在の割り込み優先順位の状態</th>
    </tr>
    <tr><td>0</td><td>0</td><td>現在割り込みなし</td></tr>
    <tr><td>0</td><td>1</td><td>優先度の低い割り込みを実行中</td></tr>
    <tr><td>1</td><td>0</td><td>優先度の高い割り込みを実行中</td></tr>
    <tr><td>1</td><td>1</td><td>予期せぬ状態、未知のエラー</td></tr>
    
</table>