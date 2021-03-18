---
title: 10. I/Oポート
type: docs
weight: 10
BookToC: false
---

# 10. I/Oポート

## 10.1 GPIO概要

CH559は45のI/Oピンを提供しますが、いくつかのピンは代替機能を持っています。それらの中で、ポートP0～P3の入出力とP4の出力はビット単位でアクセスすることができます。

ピンが代替機能として構成されていない場合、デフォルトは汎用I/Oピンの状態です。汎用デジタルI/Oとして使用する場合、すべてのI/Oポートは真のリードモディファイライト命令を持っています。SETBやCLRなどのビット操作命令をサポートし、特定のピンやポートレベルの方向を独立して変更することができます。

## 10.2 GPIOレジスタ

このセクションのすべてのレジスタとビットは、共通のフォーマットで表現されています。小文字の"n"はポートのシリアル番号(n = 0, 1, 2, 3)を表します。小文字の"x"はビットの通し番号(x = 0, 1, 2, 3, 4, 5, 6, 7)を表しています。

<div>
    <p align="center">表10.2.1 GPIOレジスタ一覧</p>
</div>

<table>
    <tr>
        <th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>P0</td><td>80h</td><td>P0ポート入出力レジスタ</td><td>FFh</td></tr>
    <tr><td>P0_DIR</td><td>C4h</td><td>P0ポート方向制御レジスタ</td><td>00h</td></tr>
    <tr><td>P0_PU</td><td>C5h</td><td>P0ポートプルアップ許可レジスタ</td><td>00h/FFh</td></tr>
    <tr><td>P1</td><td>90h</td><td>P1ポート入出力レジスタ</td><td>FFh</td></tr>
    <tr><td>P1_IE</td><td>B9h</td><td>P1ポート入力許可レジスタ</td><td>FFh</td></tr>
    <tr><td>P1_DIR</td><td>BAh</td><td>P1ポート方向制御レジスタ</td><td>00h</td></tr>
    <tr><td>P1_PU</td><td>BBh</td><td>P1ポートプルアップ許可レジスタ</td><td>FFh</td></tr>
    <tr><td>P2</td><td>A0h</td><td>P2ポート入出力レジスタ</td><td>FFh</td></tr>
    <tr><td>P2_DIR</td><td>BCh</td><td>P2ポート方向制御レジスタ</td><td>00h</td></tr>
    <tr><td>P2_PU</td><td>BDh</td><td>P2ポートプルアップ許可レジスタ</td><td>FFh</td></tr>
    <tr><td>P3</td><td>B0h</td><td>P3ポート入出力レジスタ</td><td>FFh</td></tr>
    <tr><td>P3_DIR</td><td>BEh</td><td>P3ポート方向制御レジスタ</td><td>00h</td></tr>
    <tr><td>P3_PU</td><td>BFh</td><td>P3ポートプルアップ許可レジスタ</td><td>FFh</td></tr>
    <tr><td>P4_OUT</td><td>C0h</td><td>P4ポート出力レジスタ</td><td>00h</td></tr>
    <tr><td>P4_IN</td><td>C1h</td><td>P4ポート入力レジスタ(読み込み専用)</td><td>FFh</td></tr>
    <tr><td>P4_DIR</td><td>C2h</td><td>P4ポート方向制御レジスタ</td><td>00h</td></tr>
    <tr><td>P4_PU</td><td>C3h</td><td>P4ポートプルアップ許可レジスタ</td><td>FFh</td></tr>
    <tr><td>P4_CFG</td><td>C7h</td><td>P4ポート制御レジスタ</td><td>00h</td></tr>
    <tr><td>P5_IN</td><td>C7h</td><td>P5ポート入力レジスタ(読み込み専用)</td><td>00h</td></tr>
    <tr><td>PIN_FUNC</td><td>CEh</td><td>ピン機能選択レジスタ</td><td>00h</td></tr>
    <tr><td>PORT_CFG</td><td>C6h</td><td>ポート制御レジスタ</td><td>0Fh</td></tr>
    <tr><td>XBUS_SPEED</td><td>FDh</td><td>バススピード制御レジスタ</td><td>FFh</td></tr>
    <tr><td>XBUS_AUX</td><td>A2h</td><td>バス補助制御レジスタ</td><td>00h</td></tr>

</table>

### ポート制御レジスタ(PORT_CFG):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:4]</td><td>bPn_DRV</td><td>RW</td><td>Pn ポート出力ドライブ能力選択<br />0: ドライブ電流5mAレベルを選択。<br />1: P0/P2/P3はドライブ電流20mAレベル、P1はドライブ電流10mAレベルを選択。</td><td>0000b</td></tr>
    <tr><td>[3:0]</td><td>bPn_OC</td><td>RW</td><td>Pnポートオープンドレイン出力許可<br />0: プッシュプル出力に設定。<br />1: オープンドレイン出力に設定。</td><td>1111b</td></tr>
    
</table>

### Pnポート入出力レジスタ(Pn):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>Pn.0~Pn.7</td><td>RW</td><td>Pn.xピンの入力およびデータ出力ステータスビットです。ビット単位でのアクセス可能</td><td>FFh</td></tr>
</table>

### Pnポート方向制御レジスタ(Pn_DIR):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>Pn_DIR</td><td>RW</td><td>Pn.xピン方向設定</td><td>00h</td></tr>
    
</table>

### P0ポートプルアップ許可レジスタ(P0_PU)とPnポートプルアップ許可レジスタ(Pn_PU), n = 1/2/3:

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td rowspan="2">[7:0]</td><td rowspan="2">P0_PU</td><td rowspan="2">RW</td><td>P0.xピンプルアップ抵抗が有効になります(En_P0_Pullup = 0が構成されている場合)</td><td>00h</td></tr>
    <td>P0.xピンプルアップ抵抗が有効になります(En_P0_Pullup = 1が構成されている場合)</td><td>FFh</td></tr>
    <tr><td>[7:0]</td><td>Pn_PU</td><td>RW</td><td>Pn.xピンのプルアップ抵抗が有効になります。<br />0: プルアップを無効<br />1: プルアップを有効</td><td>FFh</td></tr>
    
</table>

Pnポートの構成は、PORT_CFGのbPn_OC、ポート方向制御レジスタのPn_DIR、ポートプルアップ許可レジスタのPn_PUの組み合わせにより、以下のように実装されています。

<div>
    <p align="center">表10.2.2 ポート構成レジスタの組み合わせ</p>
</div>

<table>
    <tr>
        <th>bPn_OC</th><th>Pn_DIR</th><th>Pn_PU</th><th>概要</th>
    </tr>
    <tr><td>0</td><td>0</td><td>0</td><td>ハイインピーダンス入力モード、ピンにプルアップ抵抗なし</td></tr>
    <tr><td>0</td><td>0</td><td>1</td><td>プルアップ入力モード、ピンはプルアップ抵抗付き</td></tr>
    <tr><td>0</td><td>1</td><td>x</td><td>プッシュプル出力モードでは、対称駆動が可能なため、大電流を出力したり吸収したりすることができます</td></tr>
    <tr><td>1</td><td>0</td><td>0</td><td>ハイインピーダンス入力弱擬似双方向モード、オープンドレイン出力、端子にプルアップ抵抗なし</td></tr>
    <tr><td>1</td><td>1</td><td>0</td><td>ハイインピーダンス入力擬似双方向モード、オープンドレイン出力、ピン上のプルアップ抵抗なし、出力がLOWからHIGHに変化すると、自動的に2クロックサイクルでHIGHを駆動し、変換を高速化します</td></tr>
    <tr><td>1</td><td>0</td><td>1</td><td>弱擬似双方向モード（8051の模倣）、オープンドレイン出力、サポート入力、プルアップ抵抗付きピン</td></tr>
    <tr><td>1</td><td>1</td><td>1</td><td>準双方向モード（標準8051）、オープンドレイン出力、サポート入力、ピンは、出力がLOWからHIGHに変化するときにプルアップ抵抗を持っている、それは自動的に変換を高速化するために2クロックサイクルのためにHIGHを駆動します</td></tr>
    
</table>

ポートP0～P3は、純粋な入力またはプッシュプル出力と準双方向モードをサポートしています。ポート P4 は純入力またはプッシュプル出力、その他のモードに対応しています。各端子には VDD33 に接続された自由に制御可能な内部プルアップ抵抗と GND に接続された保護ダイオードを内蔵しています。

図10.2.1は、P1ポートのP1.x端子の等価回路図です。P1_IEとAIN、ADC_CHANNを除去した後、P0、P2、P3ポートに適用することができます。

<div>
    <p align="center">図10.2.1 I/Oピン等価回路図</p>
</div>

![GPIO_schematic](/docs/10-gpio/images/GPIO_schematic.png "GPIO schematic")

### P1ポート入力許可レジスタ(P1_IE):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>P1_IE</td><td>RW</td><td>P1.xピン入力許可。<br />0: ADCに使用され、デジタル入力は無効<br />1: デジタル入力が有効</td><td>FFh</td></tr>
    
</table>

## 10.3 P4ポート

### P4ポート入力許可レジスタ(P4_OUT):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>P4_OUT.0~P4_OUT.7</td><td>RW</td><td>P4.xピンデータ出力ビットです。ビット単位アクセス可能</td><td>00h</td></tr>
    
</table>

### P4ポート入力レジスタ(P4_IN):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>P4_IN</td><td>R0</td><td>P4.xピン入力ステータスビット</td><td>FFh</td></tr>
    
</table>

### P4ポートプルアップ許可レジスタ(P4_PU):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>P4_PU</td><td>RW</td><td>P4.xプルアップ抵抗が有効になっています。<br />0: プルアップを無効<br />1: プルアップを有効
</td><td>FFh</td></tr>
    
</table>

### P4ポート方向制御レジスタ(P4_DIR):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>P4_DIR</td><td>RW</td><td>P4.xのピン方向の設定<br />0: 入力<br />1: 出力</td><td>00h</td></tr>
    
</table>

### P4ポート制御レジスタ(P4_CFG)とP5ポート入力レジスタ(P5_IN):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>P5.7</td><td>R0</td><td>P5.7ピン入力ステータスビット</td><td>0</td></tr>
    <tr><td>6</td><td>bIO_INT_ACT</td><td>R0</td><td><p>GPIO割り込み要求の有効化状態です。<br /><br />bIE_IO_EDGE = 0の時、本ビットは1でGPIO入力アクティブレベルを示し、割り込み要求が行われ、0で入力無効レベルを示します。<br /><br />bIE_IO_EDGE = 1の場合、このビットはエッジ割り込みフラグとして使用されます。1の場合は有効なエッジが検出されたことを示します。<br />このビットはソフトウェアではクリアできません。リセットまたはレベル割り込みモードの時、または対応する割り込みサービス ルーチンゼロに入った時にのみ自動的にクリアされます。</p></td><td>0</td></tr>
    <tr><td>5</td><td>P5.5</td><td>R0</td><td>P5.5ピン入力ステータスビットと制御可能な内蔵プルダウン抵抗</td><td>0</td></tr>
    <tr><td>4</td><td>P5.4</td><td>R0</td><td>P5.4ピン入力ステータスビットと制御可能な内蔵プルダウン抵抗</td><td>0</td></tr>
    <tr><td>3</td><td>bSPI0_PIN_X</td><td>RW</td><td>SPI0ピンSCS/SCKマッピング許可です。<br />0: P1.4/P1.7が使用されます。<br />1: P4.6/P4.7が使用されます。</td><td>0</td></tr>
    <tr><td>2</td><td>bP4_DRV</td><td>RW</td><td>P4ポートの出力ドライブ能力選択。<br />0: ドライブ電流5mAレベル<br />1: ドライブ電流20mAレベル</td><td>0</td></tr>
    <tr><td>1</td><td>P5.1</td><td>R0</td><td>P5.1ピン入力ステータスビットと制御可能な内蔵プルダウン抵抗</td><td>0</td></tr>
    <tr><td>0</td><td>P5.0</td><td>R0</td><td>P5.0ピン入力ステータスビットと制御可能な内蔵プルダウン抵抗</td><td>0</td></tr>
</table>

## 10.4 GPIOマルチプレックスとマッピング

CH559 の I/O 端子の一部には代替機能があります。電源投入後は、すべて汎用 I/O ピンです。異なる機能モジュールが有効になった後、対応するピンはそれぞれの機能モジュールの対応する機能ピンとして構成されます。

### ピン機能選択レジスタ(PIN_FUNC):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bPWM1_PIN_X</td><td>RW</td><td>PWM1/PWM2ピンマッピング許可ビットです。<br />0: PWM1/2はP2.4/P2.5を使用<br />1: PWM1/2はP4.3/P4.5を使用<br /></td><td>0</td></tr>
    <tr><td>6</td><td>bTMR3_PIN_X</td><td>RW</td><td>PWM3/CAP3ピンマッピング許可ビットです。<br />0: PWM3/CAP3はP1.2を使用<br />1: PWM3/CAP3はP4.2を使用</td><td>0</td></tr>
    <tr><td>5</td><td>bT2EX_PIN_X</td><td>RW</td><td>T2EX/CAP2ピンマッピング許可ビットです。<br />0: T2EX/CAP2はP1.1を使用<br />1: T2EX/CAP2はP2.5を使用</td><td>0</td></tr>
    <tr><td>4</td><td>bUART0_PIN_X</td><td>RW</td><td>UART0ピンマッピング許可ビットです。<br />0: RXD0/TXD0はP3.0/P3.1を使用<br />1: RXD0/TXD0はP0.2/P0.3を使用</td><td>0</td></tr>
    <tr><td>3</td><td>bXBUS_EN</td><td>RW</td><td>xBUS外部バス機能許可ビット。<br />0: 外部バスを無効<br />1: P0ポートを8ビットのデータバスとして、P3.6/P3.7をバスアクセス時の書き込み/読み出しストローブ制御として有効にします。</td><td>0</td></tr>
    <tr><td>2</td><td>bXBUS_CS_OE</td><td>RW</td><td>xBUS外部バスチップセレクト出力許可ビットです。<br />0: チップセレクトの出力を無効にし、外部回路でデコードすることができます。<br />1: P3.4がCS0に設定され(XCS0チップセレクト0, アクティブロー), ALEが無効の時はバスアドレスA15が反転してP3.3(チップセレクト1, アクティブローに相当)に出力されます。</td><td>0</td></tr>
    <tr><td>1</td><td>bXBUS_AH_OE</td><td>RW</td><td>xBUS外部バスハイ 8 ビットアドレス出力許可ビット。<br />0: 出力を無効にします。<br />1: MOVX_@DPTR命令が外部バスにアクセスしている間は、出力を無効にします。P2ポートの出力バスアドレスは上位8ビットです。</td><td>0</td></tr>
    <tr><td>0</td><td>bXBUS_AL_OE</td><td>RW</td><td>xBUS 外部バスの下位 8 ビットのアドレス出力許可ビットです。<br />0: 多重化されたアドレスモードです。外部バスへのアクセス時には必要に応じて下位8ビットのアドレスがデータバスと多重化され、外部回路はALEでラッチ制御されます。<br />1: ダイレクトアドレスモードです。下位8ビットのアドレスA0～A7はP4.0～P4.5、P3.5～P2.7を介して出力されます。</td><td>0</td></tr>
    
</table>

<div>
    <p align="center">表10.2.1 I/Oピン等価回路図</p>
</div>

<table>
    <tr>
        <th>GPIO</th><th>その他の機能 : 左から優先順位の高い順</th>
    </tr>
    <tr><td>P0[0]</td><td> AD0, UDTR/bUDTR, P0.0</td></tr>
    <tr><td>P0[1]</td><td> AD1, URTS/bURTS, P0.1</td></tr>
    <tr><td>P0[2]</td><td> AD2, RXD_/bRXD_, P0.2</td></tr>
    <tr><td>P0[3]</td><td> AD3, TXD_/bTXD_, P0.3</td></tr>
    <tr><td>P0[4]</td><td> AD4, UCTS/bUCTS, P0.4</td></tr>
    <tr><td>P0[5]</td><td> AD5, UDSR/bUDSR, P0.5</td></tr>
    <tr><td>P0[6]</td><td> AD6, URI/bURI, P0.6</td></tr>
    <tr><td>P0[7]</td><td> AD7, UDCD/bUDCD, P0.7</td></tr>
    <tr><td>P1[0]</td><td> AIN0, T2/bT2, CAP1/bCAP1, P1.0</td></tr>
    <tr><td>P1[1]</td><td> AIN1, T2EX/bT2EX, CAP2/bCAP2, P1.1</td></tr>
    <tr><td>P1[2]</td><td> AIN2, PWM3/bPWM3, CAP3/bCAP3, P1.2</td></tr>
    <tr><td>P1[3]</td><td> AIN3, P1.3</td></tr>
    <tr><td>P1[4]</td><td> AIN4, SCS/bSCS, P1.4</td></tr>
    <tr><td>P1[5]</td><td> AIN5, MOSI/bMOSI, P1.5</td></tr>
    <tr><td>P1[6]</td><td> AIN6, MISO/bMISO, P1.6</td></tr>
    <tr><td>P1[7]</td><td> AIN7, SCK/bSCK, P1.7</td></tr>
    <tr><td>P2[0]</td><td> A8, P2.0</td></tr>
    <tr><td>P2[1]</td><td> MOSI1/bMOSI1, A9, P2.1</td></tr>
    <tr><td>P2[2]</td><td> MISO1/bMISO1, A10, P2.2</td></tr>
    <tr><td>P2[3]</td><td> SCK1/bSCK1, A11, P2.3</td></tr>
    <tr><td>P2[4]</td><td> PWM1/bPWM1, A12, P2.4</td></tr>
    <tr><td>P2[5]</td><td> TNOW/bTNOW, PWM2/bPWM2, A13, T2EX_/bT2EX_, CAP2_/bCAP2_, P2.5</td></tr>
    <tr><td>P2[6]</td><td> RXD1/bRXD1, A14, P2.6</td></tr>
    <tr><td>P2[7]</td><td> TXD1/bTXD1, DA7/bDA7, A15, P2.7</td></tr>
    <tr><td>P3[0]</td><td> RXD/bRXD, P3.0</td></tr>
    <tr><td>P3[1]</td><td> TXD/bTXD, P3.1</td></tr>
    <tr><td>P3[2]</td><td> LED0/bLED0, INT0/bINT0, P3.2</td></tr>
    <tr><td>P3[3]</td><td> LED1/bLED1, !A15, INT1/bINT1, P3.3</td></tr>
    <tr><td>P3[4]</td><td> LEDC/bLEDC, XCS0/bXCS0, T0/bT0, P3.4</td></tr>
    <tr><td>P3[5]</td><td> DA6/bDA6, T1/bT1, P3.5</td></tr>
    <tr><td>P3[6]</td><td> WR/bWR, P3.6</td></tr>
    <tr><td>P3[7]</td><td> RD/bRD, P3.7</td></tr>
    <tr><td>P4[0]</td><td> LED2/bLED2, A0, RXD1_/bRXD1_, P4.0</td></tr>
    <tr><td>P4[1]</td><td> A1, P4.1</td></tr>
    <tr><td>P4[2]</td><td> PWM3_/bPWM3_, CAP3_/bCAP3_, A2, P4.2</td></tr>
    <tr><td>P4[3]</td><td> PWM1_/bPWM1_, A3, P4.3</td></tr>
    <tr><td>P4[4]</td><td> LED3/bLED3, TNOW_/bTNOW_, TXD1_/bTXD1_, A4, P4.4</td></tr>
    <tr><td>P4[5]</td><td> PWM2_/bPWM2_, A5, P4.5</td></tr>
    <tr><td>P4[6]</td><td> XI, SCS_/bSCS_, P4.6</td></tr>
    <tr><td>P4[7]</td><td> XO, SCK_/bSCK_, P4.7</td></tr>
    <tr><td>P5[0]</td><td> DM/bDM, P5.0</td></tr>
    <tr><td>P5[1]</td><td> DP/bDP, P5.1</td></tr>
    <tr><td>P5[4]</td><td> HM/bHM, ALE, XB, P5.4</td></tr>
    <tr><td>P5[5]</td><td> HP/bHP, !A15, XA, P5.5</td></tr>
    <tr><td>P5[7]</td><td> RST/bRST, P5.7</td></tr>
    
</table>

上表の左から順に記載されている優先順位は、複数の機能モジュールが競合して GPIO を使用する場合の優先順位を示しています。例えば、P2ポートは出力バスアドレスの上位8ビットに設定されています。
実際にA8～A11アドレスのみを使用している場合は、A12～A15アドレスを使用していないときにP2.4～P2.7ピンを無駄にしないように、P2.4/P2.5はより優先度の高いPWM1/PWM2機能に、P2.6はRXD1機能に、P2.7はより優先度の高いTXD1やDA7機能に使用することができます。
