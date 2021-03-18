---
title: 12. タイマ
type: docs
weight: 12
BookToC: false
---

# 12. タイマ

## 12.1 Timer0/1

Timer0/1は2つの16ビットタイマ/カウンタです。TCONとTMODは、Timer0とTimer1を構成するために使用されます。TCONは、タイマ/カウンタT0とT1の起動制御とオーバーフロー割り込みと外部割り込み制御に使用されます。各タイマは、2つの8ビットレジスタからなる16ビットのタイミングユニットです。Timer0の上位バイトカウンタはTH0、下位バイトはTL0です。Timer1の上位バイトカウンタはTH1、下位バイトはTL1です。Timer1はUART0のボーレートジェネレータとしても使用できます。


<div>
    <p align="center">表12.1.1 Timer0/1関連レジスタ一覧</p>
</div>

<table>
    <tr>
        <th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>TH1 </td><td>8Dh </td><td>Timer1カウント上位バイト</td><td> xxh</td></tr>
    <tr><td>TH0 </td><td>8Ch </td><td>Timer0カウント上位バイト</td><td> xxh</td></tr>
    <tr><td>TL1 </td><td>8Bh </td><td>Timer1カウント下位バイト</td><td>xxh</td></tr>
    <tr><td>TL0 </td><td>8Ah </td><td>Timer0カウント下位バイト</td><td>xxh</td></tr>
    <tr><td>TMOD</td><td> 89h</td><td>Timer0/1モードレジスタ</td><td>00h</td></tr>
    <tr><td>TCON</td><td> 88h</td><td>Timer0/1制御レジスタ</td><td>00h</td></tr>
    
</table>

### Timer/Counter 0/1制御レジスタ(TCON):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7 </td><td>TF1 </td><td>RW </td><td>Timer1オーバーフロー割り込みフラグビットです。Timer1割り込みに入ると自動的にクリアされます。</td><td> 0</td></tr>
    <tr><td>6 </td><td>TR1 </td><td>RW </td><td>Timer1のスタート/ストップビットです。1に設定して、ソフトウェアで開始、設定、またはクリアします。</td><td> 0</td></tr>
    <tr><td>5 </td><td>TF0 </td><td>RW </td><td>Timer0オーバーフロー割り込みフラグビットです。Timer0割り込みに入ると自動的にクリアされます。</td><td> 0</td></tr>
    <tr><td>4 </td><td>TR0 </td><td>RW </td><td>Timer0のスタート/ストップビットです。1に設定して、ソフトウェアで開始、設定、またはクリアします。</td><td> 0</td></tr>
    <tr><td>3 </td><td>IE1 </td><td>RW </td><td>INT1外部割込み1の割込み要求フラグビットです。割り込みに入ると自動的にクリアされます。</td><td> 0</td></tr>
    <tr><td>2 </td><td>IT1 </td><td>RW </td><td>INT1外部割込み1トリガモード制御ビットです。<br />0: LOWレベルでトリガする外部割込みを選択します。<br />1: 立下りエッジでトリガする外部割込みを選択します。</td><td> 0</td></tr>
    <tr><td>1 </td><td>IE0 </td><td>RW </td><td>INT0外部割込み0の割込み要求フラグです。割り込みに入ると自動的にクリアされます。</td><td> 0</td></tr>
    <tr><td>0 </td><td>IT0 </td><td>RW </td><td>INT0外部割込み0トリガモード制御ビットです。<br />0: LOWレベルでトリガする外部割込みを選択します。</td><td> 0</td></tr>
</table>

### Timer/Counter 0/1 モードレジスタ(TMOD):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7 </td><td>bT1_GATE</td><td> RW</td><td>ゲート許可ビットは、タイマ1の起動が外部割込み信号INT1の影響を受けるかどうかを制御します。<br />0: Timer/Counter1が起動するかどうかはINT1とは関係ありません。<br />1: INT1ピンがHIGHでTR1が1の時にのみ起動できます。</td><td> 0</td></tr>
    <tr><td>6 </td><td>bT1_CT</td><td> RW </td><td>タイミングモードまたはカウントモード選択ビットです。T1ピンの立ち下がりエッジをクロックとし、カウントモードで動作する場合は1となります。</td><td>0</td></tr>
    <tr><td>5 </td><td>bT1_M1</td><td> RW </td><td>Timer/Counter1モード選択上位</td><td> 0</td></tr>
    <tr><td>4 </td><td>bT1_M0</td><td> RW </td><td>Timer/Counter1モード選択下位</td><td> 0</td></tr>
    <tr><td>3 </td><td>bT0_GATE</td><td>RW </td><td>ゲーティング許可ビットは、Timer0の起動が外部割込み信号INT0の影響を受けるかどうかを制御します。<br />0: INT0に関係なくTimer/Counter0が起動します。<br />1: INT0ピンのみがHIGHになり、TR0を起動することができます。</td><td>0</td></tr>
    <tr><td>2 </td><td>bT0_CT </td><td>RW </td><td>タイミングモードまたはカウントモード選択ビット。<br />0: タイミングモードになります。<br />1: T0ピンの立ち下がりエッジをクロックとして使用し、カウントモードで動作します。</td><td>0</td></tr>
    <tr><td>1 </td><td>bT0_M1 </td><td>RW </td><td>Timer/Counter0モード選択上位</td><td> 0</td></tr>
    <tr><td>0 </td><td>bT0_M0 </td><td>RW </td><td>Timer/Counter0モード選択下位</td><td> 0</td></tr>
    
</table>

<div>
    <p align="center">表12.1.2 bTn_M1, bTn_M0はTimernの動作モードを選択します(n = 0, 1)</p>
</div>

<table>
    <tr>
        <th>bTn_M1</th><th>bTn_M0</th><th>Timern working mode (n = 0, 1)</th>
    </tr>
    <tr><td>0</td><td> 0</td><td>モード0: 13ビットTimer/Counter n。計数単位は、TLnの下位5ビットとTHnで構成されています。TLnの上位3ビットは無効です。カウントが全13ビットからオール0に変化した場合、オーバーフローフラグTFnがセットされ、初期値をリセットする必要があります。</td></tr>
    <tr><td>0</td><td> 1</td><td>モード1: 16ビットTimer/Counter n。カウント単位はTLnとTHnで構成されています。カウントが全16ビットからオール0に変化すると、オーバーフローフラグTFnがセットされ、初期値をリセットする必要があります。</td></tr>
    <tr><td>1</td><td> 0</td><td>モード2: 8ビットリロードTimer/Counter nの場合、リロードカウント単位としてTLn, THnを使用します。カウントが8ビットからオール0に変化すると、オーバーフローフラグTFnがセットされ、初期値が自動的にTHnからロードされます。</td></tr>
    <tr><td>1</td><td> 1</td><td>モード3: Timer/Counter0である場合、TL0とTH0に分割され、TL0は8ビットTimer/Counterとして使用され、Timer0のすべての制御ビットを占有しています。TH0は別の8ビットタイマとして機能し、Timer1のTR1、TF1と割り込みリソースを占有しています。Timer1はこの時点でも使用可能ですが、スタート制御ビットTR1とオーバーフローフラグビットTF1は使用できません。<br />Timer/Counter1の場合、モード3に入るとTimer／Counter1は停止します。</td></tr>
    
</table>

### Timernカウント下位バイト(TLn) (n = 0, 1):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>TLn</td><td>RW</td><td>Timernカウント下位バイト</td><td>xxh</td></tr>
    
</table>

### Timernカウント上位バイト(THn) (n = 0, 1):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>TLn</td><td>RW</td><td>Timernカウント上位バイト</td><td>xxh</td></tr>
    
</table>

## 12.2 Timer2

Timer2は16ビットのオートリロードTimer/Counterです。T2CON, T2MODレジスタで設定します。Timer2の上位バイトカウンタはTH2、下位バイトはTL2です。Timer2はUART0のボーレート発生器として使用できます。また、2つの信号レベルのキャプチャ機能を持っています。キャプチャカウントはRCAP2レジスタとT2CAP1レジスタに格納されます。

<div>
    <p align="center">表12.2.1 Timer2関連レジスタ一覧</p>
</div>

<table>
    <tr>
        <th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>TH2 </td><td>CDh</td><td>Timer2カウンター上位バイト</td><td> 00h</td></tr>
    <tr><td>TL2 </td><td>CCh</td><td>Timer2カウンター下位バイト</td><td> 00h</td></tr>
    <tr><td>T2COUNT </td><td>CCh</td><td>TL2とTH2は16ビットSFRを形成します。</td><td> 0000h</td></tr>
    <tr><td>T2CAP1H </td><td>CDh</td><td>Timer2キャプチャ1データ上位バイト(読み出し専用)</td><td> xxh</td></tr>
    <tr><td>T2CAP1L </td><td>CCh</td><td>Timer2キャプチャ1データ下位バイト(読み出し専用)</td><td> xxh</td></tr>
    <tr><td>T2CAP1 </td><td>CCh</td><td>T2CAP1LとT2CAP1Hは、16ビットSFRを形成します。</td><td> xxxxh</td></tr>
    <tr><td>RCAP2H </td><td>CBh</td><td>カウントリロード/キャプチャ2データレジスタ上位バイト</td><td> 00h</td></tr>
    <tr><td>RCAP2L </td><td>CAh</td><td>カウントリロード/キャプチャ2データレジスタ下位バイト</td><td> 00h</td></tr>
    <tr><td>RCAP2 </td><td>CAh</td><td>RCAP2LとRCAP2Hは16ビットSFRを形成します。</td><td> 0000h</td></tr>
    <tr><td>T2MOD </td><td>C9h</td><td>Timer2モードレジスタ</td><td> 00h</td></tr>
    <tr><td>T2CON </td><td>C8h</td><td>Timer2制御レジスタ</td><td> 00h</td></tr>
    
</table>


### Timer/Counter2制御レジスタ(T2CON):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7 </td><td>TF2 </td><td>RW</td><td>bT2_CAP1_EN = 0の場合。Timer2のオーバーフロー割り込みフラグです。Timer2の16ビットからのカウントが全て1からオール0になる場合、オーバーフローフラグを1に設定するにはソフトウェアでクリアする必要があります。<br />RCLK = 1またはTCLK = 1の時は、本ビットは1にセットされません。</td><td> 0</td></tr>
    <tr><td>7</td><td>CAP1F</td><td>RW</td><td>bT2_CAP1_EN = 1 の場合。Timer2キャプチャ1割り込みフラグです。T2の有効エッジによってトリガされ、ソフトウェアによってクリアされる必要があります。</td><td> 0</td></tr>
    <tr><td>6</td><td>EXF2 </td><td>RW</td><td>Timer2外部トリガフラグです。EXEN2 = 1の時、T2EX有効エッジトリガで設定される。ソフトウェアでクリアする必要があります。</td><td> 0</td></tr>
    <tr><td>5</td><td>RCLK </td><td>RW</td><td>UART0の受信クロックを選択します。<br />0: Timer1のオーバーフローで発生するボーレートを選択します。<br />1: Timer2のオーバーフローで発生するボーレートを選択します。</td><td> 0</td></tr>
    <tr><td>4</td><td>TCLK </td><td>RW</td><td>UART0の送信クロックを選択します。<br />0: Timer1のオーバーフローで発生するボーレートを選択します。<br />1: Timer2のオーバーフローで発生するボーレートを選択します。</td><td> 0</td></tr>
    <tr><td>3</td><td>EXEN2</td><td>RW</td><td>T2EXトリガ許可ビット<br />0: T2EXを無視します。<br />1: T2EXエッジでのリロードまたはキャプチャのトリガを有効にします。</td><td> 0</td></tr>
    <tr><td>2 </td><td>TR2 </td><td>RW</td><td>Timer2のスタート/ストップビットです。1に設定して開始します。ソフトで設定、解除が必要。</td><td> 0</td></tr>
    <tr><td>1 </td><td>C_T2</td><td>RW</td><td>Timer2クロックソース選択ビット<br />0: 内部クロックを使用します。<br />1: T2ピンの立下りエッジに基づくエッジカウントを使用します。</td><td> 0</td></tr>
    <tr><td>0</td><td>CP_RL2</td><td>RW</td><td>Timer2機能選択ビットです。RCLKまたはTCLKが1の場合、本ビットは強制的に0になります。<br />0: Timer2はTimer/Counterとして使用され、カウンタがオーバーフローした時やT2EXレベルが変化した時に自動的に初期カウント値をリロードします。<br />1: Timer2のキャプチャ2機能を有効にし、T2EXの有効エッジをキャプチャします。</td><td> 0</td></tr>
</table>

### Timer/Counter2モードレジスタ(T2MOD):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th colspan="2">概要</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bTMR_CLK</td><td>RW</td><td colspan="2">高速クロックのT0/T1/T2タイマの高速クロックモード許可が選択されています。<br />1: 周波数分割のないシステムクロックFsysがカウントクロックとして使用されます。<br />0: 分周クロックが使用されます。<br />このビットは、標準クロックを選択するタイマには影響しません。</td><td>0</td></tr>
    <tr><td>6</td><td>bT2_CLK</td><td>RW</td><td colspan="2">Timer2内部クロック周波数選択ビットです。<br />0: 標準クロックを選択します。タイミング/カウントモードはFsys/12です。UART0のクロックモードはFsys/4です。<br />1: 高速クロックを選択します。(bTMR_CLK = 0)またはFsys(bTMR_CLK = 1)を選択します。UART0のクロックモードはFsys/2(bTMR_CLK = 0)またはFsys(bTMR_CLK = 1)です。</td><td>0</td></tr>
    <tr><td>5</td><td>bT1_CLK</td><td>RW</td><td colspan="2">Timer1の内部クロック周波数選択ビットです。<br />0: 標準クロックFsys/12を選択します。<br />1: 高速クロックFsys/4(bTMR_CLK = 0)またはFsys(bTMR_CLK = 1)を選択します。</td><td>0</td></tr>
    <tr><td>4</td><td>bT0_CLK</td><td>RW</td><td colspan="2">Timer0内部クロック周波数選択ビットです。<br />0: 標準クロックFsys/12です。<br />1: 高速クロックFsys/4(bTMR_CLK = 0)またはFsys(bTMR_CLK = 1)です。</td><td>0</td></tr>
    <tr><td>3</td><td>bT2_CAP_M1</td><td>RW</td><td>Timer2キャプチャモード上位</td><td rowspan="2"><p>キャプチャモードの選択:<br />X0: 立下りエッジから立下りエッジ<br />01: 任意のエッジから任意のエッジ、つまりレベル変化<br />11: 立上がりエッジから立上がりエッジ</td><td>0</td></tr>
    <tr><td>2</td><td>bT2_CAP_M0</td><td>RW</td><td>Timer2キャプチャモード下位</td><td>0</td></tr>
    <tr><td>1</td><td>T2OE</td><td>RW</td><td colspan="2">Timer2クロック出力許可ビット<br />0: 出力を無効にします。<br />1: T2ピンの出力クロックを有効にします。周波数はTimer2のオーバーフローレートの半分です。</td><td>0</td></tr>
    <tr><td>0</td><td>bT2_CAP1_EN</td><td>RW</td><td colspan="2">キャプチャ1モードは、RCLK = 0、TCLK = 0、CP_RL2 = 1、C_T2 = 0、T2OE = 0 のときに有効になります。<br />1: T2の有効エッジをキャプチャするためのキャプチャ1機能を有効にします。<br />0: キャプチャ1を無効にする。</td><td>0</td></tr>
</table>


### カウントリロード/キャプチャ2データレジスタ(RCAP2):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>RCAP2H</td><td>RW</td><td>タイマー/カウンタモードでのリロード値の上位バイト。キャプチャモードでCAP2がキャプチャしたタイマーの上位バイト</td><td>00h</td></tr>
    <tr><td>[7:0]</td><td>RCAP2L</td><td>RW</td><td>タイマー/カウンタモードでのリロード値の下位バイト。キャプチャモードでCAP2がキャプチャしたタイマーの下位バイト</td><td>00h</td></tr>
    
</table>

### Timer2カウンタ(T2COUNT), bT2_CAP1_EN = 0の時有効:

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>TH2</td><td>RW</td><td>カレントカウンタの上位バイト</td><td>00h</td></tr>
    <tr><td>[7:0]</td><td>TL2</td><td>RW</td><td>カレントカウンタの下位バイト</td><td>00h</td></tr>
    
</table>

### Timer2キャプチャ1データ(T2CAP1), bT2_CAP1_EN = 1の時有効:

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>T2CAP1H</td><td>RW</td><td>CAP1でキャプチャされたタイマの上位バイト</td><td>xxh</td></tr>
    <tr><td>[7:0]</td><td>T2CAP1L</td><td>RW</td><td>CAP1でキャプチャされたタイマの下位バイト</td><td>xxh</td></tr>
    
</table>

## 12.3 Timer3

<div>
    <p align="center">表12.3.1 Timer33関連レジスタ一覧</p>
</div>

<table>
    <tr>
        <th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>T3_FIFO_H </td><td>AFh</td><td>Timer3のFIFO上位バイト</td><td> xxh</td></tr>
    <tr><td>T3_FIFO_L </td><td>AEh</td><td>Timer3のFIFO下位バイト</td><td> xxh</td></tr>
    <tr><td>T3_FIFO </td><td>AEh</td><td>T3_FIFO_LとT3_FIFO_Hは16ビットのSFRを形成します</td><td> xxxxh</td></tr>
    <tr><td>T3_DMA_AH </td><td>ADh</td><td>DMAカレントバッファアドレス上位バイト</td><td> 0xh</td></tr>
    <tr><td>T3_DMA_AL </td><td>ACh</td><td>DMAカレントバッファアドレス下位バイト</td><td> xxh</td></tr>
    <tr><td>T3_DMA </td><td>ACh</td><td>T3_DMA_ALとT3_DMA_AHは16ビットのSFRを形成します</td><td>0xxxh</td></tr>
    <tr><td>T3_DMA_CN </td><td>ABh</td><td>DMA残量レジスタ</td><td>00h</td></tr>
    <tr><td>T3_CTRL </td><td>AAh</td><td>Timer3制御レジスタ</td><td>02h</td></tr>
    <tr><td>T3_STAT </td><td>A9h</td><td>Timer3状態レジスタ</td><td>00h</td></tr>
    <tr><td>T3_END_H </td><td>A7h</td><td>Timer3カウント終了上位バイト</td><td>xxh</td></tr>
    <tr><td>T3_END_L </td><td>A6h</td><td>Timer3カウント終了下位バイト</td><td>xxh</td></tr>
    <tr><td>T3_END </td><td>A6h</td><td>T3_END_LとT3_END_Hは16ビットのSFRを形成する</td><td>xxxxh</td></tr>
    <tr><td>T3_COUNT_H </td><td>A5h</td><td>Timer3カレントカウント上位バイト(読み出し専用)</td><td>00h</td></tr>
    <tr><td>T3_COUNT_L </td><td>A4h</td><td>Timer3カレントカウント下位バイト(読み出し専用)</td><td>00h</td></tr>
    <tr><td>T3_COUNT </td><td>A4h</td><td>T3_COUNT_LとT3_COUNT_Hは16ビットのSFRを形成する</td><td>0000h</td></tr>
    <tr><td>T3_CK_SE_H </td><td>A5h</td><td>Timer3クロック分周設定上位バイト</td><td>00h</td></tr>
    <tr><td>T3_CK_SE_L </td><td>A4h</td><td>Timer3クロック分周設定下位バイト</td><td>20h</td></tr>
    <tr><td>T3_CK_SE </td><td>A4h</td><td>T3_CK_SE_LとT3_CK_SE_Hは16ビットのSFRを形成する</td><td>0020h</td></tr>
    <tr><td>T3_SETUP </td><td>A3h</td><td>Timer3設定レジスタ</td><td>04h</td></tr>
    
</table>

### Timer3セットアップレジスタ(T3_SETUP):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bT3_IE_END</td><td>RW</td><td>1: キャプチャモードカウントタイムアウト割り込みまたはPWMモード終了割り込みを有効にします。<br />0: 無効にします。</td><td>0</td></tr>
    <tr><td>6</td><td>bT3_IE_FIFO_OV</td><td>RW</td><td>1: FIFOオーバーフロー割り込みを有効にします。<br />0: FIFOオーバーフロー割り込みを無効にします。</td><td>0</td></tr>
    <tr><td>5</td><td>bT3_IE_FIFO_REQ</td><td>RW</td><td>1: キャプチャモードFIFO>=4割り込みまたはPWMモードFIFO<=3割り込みを有効にします。<br />0: 無効にします。</td><td>0</td></tr>
    <tr><td>4</td><td>bT3_IE_ACT</td><td>RW</td><td>1: キャプチャモード入力信号割り込みまたはPWMモード出たトリガー割り込みを有効にします。<br />0: 無効にします。</td><td>0</td></tr>
    <tr><td>3</td><td>reserve</td><td>R0</td><td>予約</td><td>0</td></tr>
    <tr><td>2</td><td>bT3_CAP_IN</td><td>R0</td><td>ノイズフィルタリング後のカレントキャプチャピンの入力レベル</td><td>1</td></tr>
    <tr><td>1</td><td>bT3_CAP_CLK</td><td>RW</td><td>最小パルス幅の制限なしに入力キャプチャを有効にします。T3_CK_SE が 1 の時のみ有効です。高速信号のキャプチャに使用します。<br />1: 有効にします。<br />0: 無効にします。</td><td>0</td></tr>
    <tr><td>0</td><td>bT3_EN_CK_SE</td><td>RW</td><td>1: クロック分周器設定レジスタへのアクセスを有効にします。<br />0: カレントカウントレジスタへのアクセスを有効にします。</td><td>0</td></tr>
    
</table>

### Timer3カレントカウント(T3_COUNT), bT3_EN_CK_SE = 0の時有効:

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_COUNT_H</td><td>RO</td><td>Timer3カレントカウント上位バイト</td><td>00h</td></tr>
    <tr><td>[7:0]</td><td>T3_COUNT_L</td><td>RO</td><td>Timer3カレントカウント下位バイト</td><td>00h</td></tr>
    
</table>

### Timer3クロック分周設定レジスタ(T3_CK_SE), bT3_EN_CK_SE = 1の時有効:

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_CK_SE_H</td><td>RW</td><td>Timer3クロック分周器の上位バイト。下位4ビットのみが有効です。上位4ビットは0固定</td><td>00h</td></tr>
    <tr><td>[7:0]</td><td>T3_CK_SE_L</td><td>RW</td><td>Timer3クロック分周器の下位バイト</td><td>20h</td></tr>
    
</table>

### Timer3カウント終了値レジスタ(T3_END):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_END_H</td><td>RW</td><td>Timer3カウント終了上位バイト</td><td>xxh</td></tr>
    <tr><td>[7:0]</td><td>T3_END_L</td><td>RW</td><td>Timer3カウント終了下位バイト</td><td>xxh</td></tr>
    
</table>

### Timer3状態レジスタ(T3_STAT):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bT3_IF_DMA_END</td><td>RW</td><td>DMA終了割り込みフラグ。<br />1: 割り込みを示します。<br />0: 割り込み無しを示します。1を書き込んでクリアされるか、T3_DMA_CNでクリアされます。</td><td>0</td></tr>
    <tr><td>6</td><td>bT3_IF_FIFO_OV</td><td>RW</td><td>1: FIFOオーバーフロー割り込みを示します。<br />0: 割り込みが発生していないことを示します。1を書き込んでクリア</td><td>0</td></tr>
    <tr><td>5</td><td>bT3_IF_FIFO_REQ</td><td>RW</td><td>1: FIFOデータ割り込みフラグが要求されていることを示します。キャプチャモードではFIFO>=4でトリガされ、PWMモードではFIFO <=3でトリガされます。<br />0: 割り込みはありません。1を書き込んでクリア</td><td>0</td></tr>
    <tr><td>4</td><td>bT3_IF_ACT</td><td>RW</td><td>1: bT3_IE_ACT = 1の時、キャプチャモード入力信号により割り込みが発生するか、PWMモードデータにより割り込みが発生することを示します。<br />0: 割り込みが発生していないことを示します。1を書き込みでクリアされるか、FIFOアクセス時にクリアされます。</td><td>0</td></tr>
    <tr><td>4</td><td>bT3_IF_END</td><td>RW</td><td>1: bT3_IE_ACT = 0の時、キャプチャモードのカウントタイムアウト割り込みまたはPWMモードのサイクルエンド割り込みを示します。<br />0: 割り込みはありません。1を書き込みクリア</td><td>0</td></tr>
    <tr><td>[3:0]</td><td>MASK_T3_FIFO_CNT</td><td>R0</td><td>Timer3の現在FIFOカウント</td><td>0000b</td></tr>
    
</table>

### Timer3制御レジスタ(T3_CTRL):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bT3_CAP_M1</td><td>RW</td><td>Timer3キャプチャエッジモード上位ビット。PWMデータリピートモード上位ビット</td><td>0</td></tr>
    <tr><td>6</td><td>bT3_CAP_M0</td><td>RW</td><td>Timer3キャプチャエッジモード下位ビット。PWMデータリピートモード下位ビット</td><td>0</td></tr>
    <tr><td>5</td><td>bT3_PWM_POLAR</td><td>RW</td><td>PWMモード時のPWM出力極性制御ビットです。<br />0: デフォルトはLOWレベルで、アクティブHIGHです。<br />1: デフォルトはHIGHレベルで、アクティブLOWです。<br /></td><td>0</td></tr>
    <tr><td>5</td><td>bT3_CAP_WIDTH</td><td>RW</td><td>キャプチャモードでは、キャプチャパルス幅の最小値設定ビットです。このビットが0の場合、最低4分周クロックサイクルが有効です。</td><td>0</td></tr>
    <tr><td>4</td><td>bT3_DMA_EN</td><td>RW</td><td>1: Timer3のDMA、DMA割り込みを有効にします。<br />0: 無効になります。</td><td>0</td></tr>
    <tr><td>3</td><td>bT3_OUT_EN</td><td>RW</td><td>1: Timer3のPWM出力を有効にします。<br />0: 無効になります。</td><td>0</td></tr>
    <tr><td>2</td><td>bT3_CNT_EN</td><td>RW</td><td>1: Timer3のカウントを有効にします。<br />0: カウントを停止します。</td><td>0</td></tr>
    <tr><td>1</td><td>bT3_CLR_ALL</td><td>RW</td><td>1: Timer3のカウントとFIFOをクリアします。ソフトでクリアする必要があります</td><td>1</td></tr>
    <tr><td>0</td><td>bT3_MOD_CAP</td><td>RW</td><td>Timer3のモード選択ビットです。<br />0: Timer3はTimer/CountまたはPWMモードで動作します。<br />1: キャプチャモードで動作します。</td><td>0</td></tr>
    
</table>

キャプチャモードでは、bT3_CAP_M1、bT3_CAP_M0はキャプチャエッジを選択します。<br />
00: キャプチャモードはオフまたはサスペンドになります。<br />
01: エッジは任意のエッジ、すなわちレベルの変化によってアクティブになり、キャプチャは任意のエッジから任意のエッジまでである。<br />
10: 立ち下がりエッジによってアクティブになり、立ち下がりエッジから立ち下がりエッジへのキャプチャ。<br />
11: 立ち上がりエッジでアクティブになり、立ち上がりエッジから立ち上がりエッジへのキャプチャ。

PWMモードでは、bT3_CAP_M1、bT3_CAP_M0は、データの繰り返し回数を選択します。<br />
00: データは再利用されず、PWMサイクルごとに新しいデータが使用されます。<br />
01: 4回データを再利用し、連続4回のPWMサイクルで同じデータを使用します。<br />
10: 8回データを再利用します。<br />
11: 16回データを再使用します。

### DMA残量レジスタ(T3_DMA_CN):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_DMA_CN</td><td>RW</td><td>初期値にプリセット可能で、DMA動作後に自動的に減算される現在のDMA残量</td><td>00h</td></tr>
    
</table>

### DMAカレントバッファアドレス(T3_DMA):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_DMA_AH</td><td>RW</td><td>現在のDMAアドレスの上位バイト。初期値にプリセットすることができます。DMA後に自動的に増加します。下位4ビットのみ有効です。上位4ビットは0に固定されます。xRAMの最初の4Kのみサポートしています。</td><td>0xh</td></tr>
    <tr><td>[7:0]</td><td>T3_DMA_AL</td><td>RW</td><td>現在のDMAアドレスの下位バイト。初期値にプリセットすることができます。DMA後に自動的に増加します。上位7ビットのみ有効です。最下位ビットは0に固定されます。偶数アドレスのみサポートしています。</td><td>xxh</td></tr>
    
</table>

### FIFOポート(T3_FIFO):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_FIFO_H</td><td>RW</td><td>Timer3のFIFO上位バイト</td><td>xxh</td></tr>
    <tr><td>[7:0]</td><td>T3_FIFO_L</td><td>RW</td><td>Timer3のFIFO下位バイト</td><td>xxh</td></tr>
    
</table>

## 12.4 PWM機能

CH559のTimer3は16ビットのPWM機能を有している。この他に8ビットのPWMが2つあります。PWMは、デフォルトの出力極性をLOWまたはHIGHに選択することができます。PWM出力のデューティサイクルを動的に変更することができます。単純なRC抵抗とコンデンサで積分ローパスフィルタリングを行った後、様々な出力電圧を得ることができ、これは低速デジタル-アナログ変換DACに相当する。

PWM3出力のデューティサイクル = T3_FIFO / T3_END。値が0%から100%までの範囲をサポートしています。T3_FIFOの値がT3_ENDより大きい場合は100%として扱われます。

PWM1出力のデューティサイクル = PWM_DATA / PWM_CYCLE。値が0%から100%までの範囲をサポートしています。PWM_DATAの値がPWM_CYCLEよりも大きい場合は100%として扱われます。

PWM2出力のデューティサイクル = PWM_DATA2 / PWM_CYCLE。値が0%から100%までの範囲をサポートしています。PWM_DATA2の値がPWM_CYCLEよりも大きい場合は100%として扱われます。

実用的な用途では、PWMピンの出力を許可し、PWM出力ピンをプッシュプル出力モードに設定することを推奨します。

### 12.4.1 PWM1とPWM2

<div>
    <p align="center">表12.4.1 PWM1、PWM2関連レジスタ一覧</p>
</div>

<table>
    <tr>
        <th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>PWM_CYCLE</td><td>9Fh</td><td>3</td><td>xxh</td></tr>
    <tr><td>PWM_CK_SE</td><td>9Eh</td><td>3</td><td>00h</td></tr>
    <tr><td>PWM_CTRL</td><td>9Dh</td><td>3</td><td>02h</td></tr>
    <tr><td>PWM_DATA</td><td>9Ch</td><td>3</td><td>xxh</td></tr>
    <tr><td>PWM_DATA2</td><td>9Bh</td><td>3</td><td>xxh</td></tr>
    
</table>


### PWM2データレジスタ(PWM_DATA2):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>PWM_DATA2</td><td>RW</td><td>PWM2の現在データを格納します。PWM2出力アクティブレベルのデューティサイクル = PWM_DATA2 / PWM_CYCLE</td><td>xxh</td></tr>
    
</table>

### PWM1 data register (PWM_DATA):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>PWM_DATA</td><td>RW</td><td>PWM1の現在データを格納します。 PWM1出力アクティブレベルのデューティサイクル = PWM_DATA / PWM_CYCLE</td><td>xxh</td></tr>
    
</table>

### PWM制御レジスタ(PWM_CTRL):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bPWM_IE_END</td><td>RW</td><td>1: PWM終了またはMFMバッファエンプティ割り込みを有効にします。</td><td>0</td></tr>
    <tr><td>6</td><td>bPWM2_POLAR</td><td>RW</td><td>bPWM_MOD_MFM = 0の場合、PWM2の出力極性を制御します。<br />0: デフォルトLOW、アクティブHIGH<br />1: デフォルトHIGH、アクティブLOW</td><td>0</td></tr>
    <tr><td>6</td><td>bMFM_BUF_EMPTY</td><td>R0</td><td>1: bPWM_MOD_MFM = 1の場合、MFMバッファが空であることを示します</td><td>0</td></tr>
    <tr><td>5</td><td>bPWM_POLAR</td><td>RW</td><td>PWM1出力の極性を制御します。<br />0: デフォルトLOW、アクティブHIGH<br />1: デフォルトHIGH、アクティブLOW</td><td>0</td></tr>
    <tr><td>4</td><td>bPWM_IF_END</td><td>RW</td><td>PWMサイクル終了割り込みフラグビット<br />1: 割り込みが発生していることを示します。1の書き込みでクリア、またはPWM_CYCLEの書き込みでクリア、またはデータのリロード時にクリア</td><td>0</td></tr>
    <tr><td>3</td><td>bPWM_OUT_EN</td><td>RW</td><td>PWM1出力許可です。<br />1: PWM1出力を有効にします。</td><td>0</td></tr>
    <tr><td>2</td><td>bPWM2_OUT_EN</td><td>RW</td><td>1: bPWM_MOD_MFM = 0のとき。PWM2出力を有効にします。</td><td>0</td></tr>
    <tr><td>2</td><td>bMFM_BIT_CNT2</td><td>R0</td><td>bPWM_MOD_MFM = 1の場合、現在のMFMエンコードの進行状況を示します。<br />0: 下位4ビットが処理中であることを示します。<br />1: 上位4ビットが処理中であることを示します。</td><td>0</td></tr>
    <tr><td>1</td><td>bPWM_CLR_ALL</td><td>RW</td><td>1: PWM1とPWM2のカウントとFIFOをクリアするためのもので、ソフトウェアでクリアする必要があります。</td><td>1</td></tr>
    <tr><td>0</td><td>bPWM_MOD_MFM</td><td>RW</td><td>MFMエンコードモード<br />0: PWMモード<br />1: MFMモード</td><td>0</td></tr>
    
</table>

### PWMクロック分周設定レジスタ(PWM_CK_SE):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>PWM_CK_SE</td><td>RW</td><td>PWMクロック分周器の設定</td><td>00h</td></tr>
    
</table>

### PWMPWM周期レジスタ(PWM_CYCLE):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>PWM_CYCLE</td><td>RW</td><td>PWM周期を設定します。値が00hの場合、100hを意味します。</td><td>xxh</td></tr>
    
</table>


## 12.5 タイマ機能

### 12.5.1 Timer0/1

1. タイマの内部クロック周波数を選択するためにT2MODを設定します。bTn_CLK(n = 0/1)が0の場合、Timer0/1に対応するクロックはFsys/12となります。bTn_CLK が1の場合は、bTMR_CLK = 0または1でFsysを選択します。クロックとして/4またはFsysを選択します。
2. TMODを設定してタイマーの動作モードを設定します。
3. タイマ/カウンタの初期値TLn, THn(n = 0/1)を設定します。
4. TCONのビットTRn(n = 0/1)を設定することで、タイマ/カウンタの起動/停止を行います。ビットTFn(n = 0/1)で問い合わせるか、割り込みモードで検出します。

### モード0: 13ビットタイマ/カウンタ

![Timer_Mode_0](/docs/12-timer/images/tim_mod_0.png "Timer0/1 Mode 0")

<div>
    <p align="center">図12.5.1.1 Timer0/1モード0</p>
</div>

### モード1: 16ビットタイマ/カウンタ

![Timer_Mode_1](/docs/12-timer/images/tim_mod_1.png "Timer0/1 Mode 1")

<div>
    <p align="center">図12.5.1.2 Timer0/1モード1</p>
</div>

### モード2: 自動リロード8ビットタイマ/カウンタ

![Timer_Mode_2](/docs/12-timer/images/tim_mod_2.png "Timer0/1 Mode 2")

<div>
    <p align="center">図12.5.1.3 Timer0/1モード2</p>
</div>


### モード3: Timer0は、独立した2つの8ビットのタイマとカウンタに分解され、Timer1のTR1制御ビットが借用される。
Timer1は、借用されたTR1制御ビットをモード3を開始するか否かで置き換え、Timer1はモード3に入り、Timer1の動作を停止する。

![Timer_Mode_3](/docs/12-timer/images/tim_mod_3.png "Timer0/1 Mode 3")

<div>
    <p align="center">図12.5.1.4 Timer0モード3</p>
</div>


### 12.5.2 Timer2

Timer2 16ビットリロードタイマ/カウンタモード:

1. T2CONのRCLK, TCLKビットを0に設定し、非シリアルポートのボーレート生成モードを選択します。
2. T2CONのビットC_T2を0に設定します。内部クロックを使用する場合は(3)に進みます。また、T2ピンの立ち下がりエッジをカウントクロックとして使用する場合は、ステップ(3)をスキップして1に設定することもできます。
3. T2MODを設定して、タイマの内部クロック周波数を選択します。bT2_CLKが0の場合、Timer2のクロックはFsys / 12となります。bT2_CLKが1の場合は、T.R_CLK=0または1でクロックとしてFsys / 4またはFsysを選択します。
4. T2CONのCP_RL2ビットを0に設定し、Timer2の 16ビットリロードタイマ／カウンタ機能を選択します。
5. タイマーオーバーフロー後のリロード値に RCAP2L,RCAP2H を設定します。タイマの初期値にTL2, TH2を設定し（一般的にはRCAP2L, RCAP2Hと同じ）、TR2を1に設定し、t.rt Timer2を設定します。
6. 現在のタイマ/カウンタの状態は、TF2またはTimer2割り込みを問い合わせることで取得できます。


![Timer2_16bit](/docs/12-timer/images/tim2_16bit.png "Timer2 16-bit")

<div>
    <p align="center">図12.5.2.1 Timer2 16ビットリロードタイマ/カウンタ</p>
</div>

Timer2のクロック出力モード:

16ビットリロードタイマ/カウンタモードを参照してください。T2MODのT2OEビットを1に設定することで、T2ピンからのTF2分周クロックの出力を有効にします。

Timer2シリアルポート0ボーレート生成モード:

1. T2CONのビットC_T2を0に設定して内部クロックを使用するか、1に設定してT2ピンの立ち下がりエッジをクロックとして選択します。T2CONのRCLK, TCLKを1またはh.mのいずれかに設定します。ボーレートジェネレータのモードを選択します。
2. タイマの内部クロック周波数を選択するためにT2MODを設定します。bT2_CLKが0の場合、Timer2のクロックはFsys / 4となります。bT2_CLKが1の場合は、bTMR_CLK=0または1でFsys / 2またはFsysをクロックとして選択します。
3. タイマオーバーフロー後のリロード値にRCAP2L, RCAP2Hを設定し、TR2を1に設定してTimer2を起動します。

![Timer2_baud_gen](/docs/12-timer/images/tim2_uart0.png "Timer2 UART0 Baud Rate Generator")

<div>
    <p align="center">図12.5.2.2 Timer2 UART0ボーレートジェネレータ</p>
</div>

Timer2デュアルチャネルキャプチャモード:

1. T2CONのRCLK, TCLKビットを0に設定し、非シリアルポートのボーレート生成モードを選択します。
2. T2CONのビットC_T2を0に設定します。内部クロックを使用するように選択し、ステップ(3)に進みます。また、T2ピンの立ち下がりエッジをカウントクロックとして選択するように設定し、ステップ(3)をスキップします。
3. T2MODを設定して、タイマの内部クロック周波数を選択します。bT2_CLKが0の場合、Timer2のクロックはFsys/12となります。bT2_CLKが1の場合は、T.R_CLK=0または1でクロックとしてFsys/4またはFsysを選択します。
4. T2MODのbT2_CAP_M1, bT2_CAP_M0を設定し、対応するエッジキャプチャモードを選択します。
5. T2CONのCP_RL2ビットを1に設定し、Timer2によるT2EXピンのキャプチャ機能を選択します。
6. TL2, TH2をタイマの初期値に設定し、TR2を1に設定してTimer2をスタートさせます。
7. CAP2のキャプチャが完了すると、RCAP2LとRCAP2Hは現在のTL2とTH2のカウント値を保存し、EXF2を設定して割り込みを発生させます。次のキャプチャされたRCAP2LとRCAP2H i.lは、最後にキャプチャされたRCAP2LとRCAP2Hの間になり、その差は2つの有効なエッジ間の信号幅となります。
8. T2CONのビットC_T2が0、T2MODのビットbT2_CAP1_ENが1の場合、T2ピンのTimer2のキャプチャ機能が同時に有効になります。CAP1のキャプチャが完了すると、T2CAP1LとT2CAP1Hは現在のTL2とTH2を保存し、CAP1Fビットがセットされて割り込みを発生させます。

![tim2_capture_mode](/docs/12-timer/images/tim2_capture_mode.png "Timer2 capture mode")

<div>
    <p align="center">図12.5.2.3 Timer2キャプチャモード</p>
</div>

### 12.5.3 Timer3

1. T3_SETUPのビットbT3_EN_CK_SEを1に設定し、Timer3のクロック分周設定レジスタT3_CK_SEを有効にして分周係数を設定し、クロックをFsys / T3_CK_SEにカウントします。設定完了後、T._EN_CK_SEをクリアします。
2. T3_ENDのカウント終了値またはPWMの総回数を設定します。
3. 必要に応じてT3_SETUPで割り込み許可をONにします。
4. T3_CTRLに各制御ビットをセットしてモードを選択し、bT3_CLR_ALLをクリアし、bT3_CNT_ENをセットしてTimer3をスタートさせる。
5. 必要に応じてT3_DMA_ALとT3_DMA_AHとT3_DMA_CNを設定し、bT3_DMA_ENを設定してDMA機能を有効にする。

![tim3_16bit_tpc](/docs/12-timer/images/tim3_16bit_tpc.png "Timer3 16-bit timer/PWM/capture")

<div>
    <p align="center">図12.5.3.1 Timer3 16ビット タイマ/PWM/キャプチャ</p>
</div>

Timer3のキャプチャされたデータ形式:

Timer3は、信号の有効な2つのエッジ間の幅をキャプチャするために使用することができます。キャプチャ中またはキャプチャ後に、DMAまたはFIFOを直接読み込んで幅データを取得することができます。データは16ビットです。最上位ビットは識別ビットで、下位15ビットは幅カウントです。信号キャプチャが有効になった後は、有効なエッジが検出されるたび、またはタイマーがオーバーフローするたびにデータが生成されます。最初の有効エッジで生成されるデータは、シグナルキャプチャが有効になった時点からの幅であり、2つの有効エッジの幅ではないので、通常は最初にキャプチャされたデータは破棄されます。

(1). bT3_CAP_M1とbT3_CAP_M0が11

立ち上がりエッジが有効であり、立ち上がりエッジから立ち上がりエッジまでの信号幅を捕捉していること、すなわち立ち上がりエッジが有効になっていることを意味する。データの最上位ビットが１であれば、信号幅がカウント終了値をオーバーしたことを意味し、つまり、タイミングがT3_ENDを超えて次の立ち上がりエッジが見つからない場合には、最上位ビットが0である次のデータの幅に幅値を加算しなければならない。最上位ビットが0であるということは、ある立ち上がりエッジの終了間の幅であり、ある前のデータの最上位ビットが1であるかどうかによって下位15ビットが前方に累積されるかどうかが決まるということである。このモードでは、超広幅信号や特殊用途の終了信号を検出するためにT3_ENDを設定し、通常の信号はオーバーフローしないようにすることが望ましい。

例えば、T3_ENDが4000hに設定され、収集された生データのシーケンスは以下のようになります。

1234h, 2345h, 0456h, C000h, C000h, 1035h, 3579h, C000h, 2468h, 0987h を組み合わせて、各立ち上がりエッジ間の間隔を求めます。: 1234h, 2345h, 0456h, 9035h, 3579h, 6468h, 0987h

(2). bT3_CAP_M1とbT3_CAP_M0が10

上記と同じですが、立上りエッジから立下りエッジまでの信号幅を捉えた立下りエッジが有効、つまり立下りエッジが有効となります。

(3). bT3_CAP_M1とbT3_CAP_M0が01

任意のエッジは有効であり、任意のエッジから任意のエッジまでの信号幅をキャプチャし、すなわち、レベル変化が有効化されます。データの最上位ビットが1の場合、幅はハイレベル幅、つまり立ち上がりエッジから立ち下がりエッジまでの幅となります。データの最上位ビットが0の場合、幅はローレベル幅、すなわち立下りエッジから立上りエッジまでの幅となる。最上位ビットをマスキングした後の下位15ビットは、分割クロックでカウントされた幅値となります。このモードでは、タイミングオーバーフローを避けるために、T3_ENDをより大きな値に設定することが推奨されますが、有効データの15桁を超えてはいけません。
