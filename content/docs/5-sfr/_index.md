---
title: 5. 特殊機能レジスタSFR
type: docs
weight: 5
BookToC: false
---

# 5. 特殊機能レジスタSFR

本マニュアルでは、以下の略語を使用してレジスタを説明することがあります:

| 略語 | 説明 |
|--------------|-------------|
| RO | アクセスタイプを示す: 読み取り専用 |
| WO | アクセスタイプを示す: 書き込みのみ、読み取り値は無効 |
| RW | アクセスタイプを示す: 読み取り可能、書き込み可能 |
| h | 16進数で表現 |
| b | 2進数で表現 |

## 5.1 SFRのプロフィールとアドレス配信

CH559は、特殊機能レジスタSFR, xSFRを使用して動作モードの制御、管理、設定を行います。

SFRは内部データ記憶領域の80h-FFhアドレス範囲を占有し、ダイレクトアドレスモード命令でのみアクセス可能です。

アドレスがx0hまたはx8hであるレジスタは、特定のビットにアクセスする際に他のビットの値を変更しないようにするため、ビットアドレス指定が可能です。8の倍数以外のレジスタは、バイト単位でしかアクセスできません。

SFRには、セーフモードではデータを書き込むことができますが、非セキュアモードでは読み取り専用になるものがあります。GLOBAL_CFG、PLL_CFG、CLOCK_CFG、SLEEP_CTRL、WAKE_CTRLなどです。

いくつかのSFRは、次のような1つ以上のエイリアスを持っています。SPI0_CK_SE/SPI0_S_PRE、UDEV_CTRL/UHUB0_CTRL、UEP1_CTRL/UH_SETUP、UEP2_CTRL/UH_RX_CTRL、UEP2_T_LEN/UH_EP_PID、UEP3_CTRL/UH_TX_CTRL、UEP3_T_LEN/UH_TX_LEN、P5_PIN/P4_CFG。

部分アドレスは、複数の独立したSFRに対応します。例えば、TL2/T2CAP1L、TH2/T2CAP1H、SAFE_MOD/CHIP_ID、T3_COUNT_L/T3_CK_SE_L、T3_COUNT_H/T3_CK_SE_H、SER1_FIFO/SER1_RBR/SER1_THR/SER1_DLL、SER1_IER/SER1_DLM、SER1_IIR/SER1_FCR、SER1_ADDR/SER1_DIV、ROM_CTRL/ROM_STATUS。

xSFRは、外部データ記憶空間のxdataタイプの2440h～298Fhのアドレス範囲を占有する。またはpdataタイプの40H-8Fhアドレス範囲を指定します。xSFRはMOVX命令による間接アドレス指定でのみバイト単位でアクセスできます。

デフォルトはDPTRポインタがベースです。ただし、bXIR_XSFRが設定された後、高速なR0またはR1は、pdata型ポインタを用いて`pU*`と`pLED_*`という名前のxSFRにアクセスすることができます。

いくつかのxSFRは、1つまたは複数のエイリアスを持っています。例えば、UEP2_3_MOD/UH_EP_MOD、UEP2_DMA_H/UH_RX_DMA_H、UEP2_DMA_L/UH_RX_DMA_L、UEP2_DMA/UH_RX_DMA、UEP3_DMA_H/UH_TX_DMA_H、UEP3_DMA_L/UH_TX_DMA_L、UEP3_DMA/UH_TX_DMA。

部分アドレスは、複数の独立したxSFRに対応しています。例えば、LED_DATA/LED_FIFO_CN。

CH559には、8051標準SFRの全レジスタが格納されています。そして、デバイス制御レジスタが追加されました。具体的なSFRは以下の表に示します。
<div>
    <p align="center">表 5.1 特殊機能レジスタ表</p>
</div>

![SFR_Table](/docs/5-sfr/images/SFR_Table.png "SFR Table")

備考: (1)赤の文字はビット単位でアドレス指定できることを意味 (2)カラーボックスの対応は以下の通り

<table>
    <tr>
        <th>色</th>
        <th>説明</th>
    </tr>
    <tr><td bgcolor="#a6a6a6"></td><td>レジスタアドレス</td></tr>
    <tr><td bgcolor="#cc99ff"></td><td>SPI0関連レジスタ</td></tr>
    <tr><td bgcolor="#99ccff"></td><td>ADC関連レジスタ</td></tr>
    <tr><td bgcolor="#ccffcc"></td><td>USB関連レジスタ</td></tr>
    <tr><td bgcolor="#ccffff"></td><td>Timer/Counter2関連レジスタ</td></tr>
    <tr><td bgcolor="#ffff99"></td><td>ポート設定関連レジスタ</td></tr>
    <tr><td bgcolor="#ff99cc"></td><td>SPI1関連レジスタ</td></tr>
    <tr><td bgcolor="#00ffff"></td><td>PWM1, PWM2関連レジスタ</td></tr>
    <tr><td bgcolor="#ffff00"></td><td>UART1関連レジスタ</td></tr>
    <tr><td bgcolor="#808000"></td><td>Timer/Counter0, 1関連レジスタ</td></tr>
    <tr><td bgcolor="#008080"></td><td>フラッシュROM関連レジスタ</td></tr>
</table>

## 5.2 SFRの種類とリセット値

<table>
    <tr>
        <th>機能</th><th>名前</th><th>アドレス</th><th>説明</th><th>リセット値</th>
    </tr>
    <tr><td rowspan="11">システム設定レジスタ</td><td>B</td><td>F0h</td><td>Bレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>ACC</td><td>E0h</td><td>アキュムレータ</td><td>0000 0000b</td></tr>
    <tr><td>PSW</td><td>D0h</td><td>プログラムステータスレジスタ</td><td>0000 0000b</td></tr>
    <tr><td rowspan="2">GLOBAL_CFG</td><td rowspan="2">B1h</td><td>グローバルコンフィグレーションレジスタ(ブートローダ状態)</td><td>1110 0000b</td></tr>
    <tr><td>グローバルコンフィグレーションレジスタ(アプリケーション状態)</td><td>1100 0000b</td></tr>
    <tr><td>CHIP_ID</td><td>A1h</td><td>チップID(読み取り専用)</td><td>0101 1001b</td></tr>
    <tr><td>SAFE_MOD</td><td>A1h</td><td>セーフモード制御レジスタ(書き込み専用)</td><td>0000 0000b</td></tr>
    <tr><td>DPH</td><td>83h</td><td>データアドレスポインタ上位8ビット</td><td>0000 0000b</td></tr>
    <tr><td>DPL</td><td>82h</td><td>データアドレスポインタ下位8ビット</td><td>0000 0000b</td></tr>
    <tr><td>DPTR</td><td>82h</td><td>DPLとDPHで16ビットのSFRを指定</td><td>0000h</td></tr>
    <tr><td>SP</td><td>81h</td><td>スタックポインタ</td><td>0000 0111b</td></tr>
    <tr><td rowspan="7">クロックとスリープ、電源制御レジスタ</td><td>WDOG_COUNT</td><td>FFh</td><td>ウォッチドッグカウントレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>RESET_KEEP</td><td>FEh</td><td>リセット保持レジスタ(パワーオンリセット状態)</td><td>0000 0000b</td></tr>
    <tr><td>WAKE_CTRL</td><td>EBh</td><td>スリープウェイク制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>SLEEP_CTRL</td><td>EAh</td><td>スリープ制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>CLOCK_CFG</td><td>B3h</td><td>システムクロック設定レジスタ</td><td>1001 1000b</td></tr>
    <tr><td>PLL_CFG</td><td>B2h</td><td>PLLクロック設定レジスタ</td><td>1101 1000b</td></tr>
    <tr><td>PCON</td><td>87h</td><td>電源制御レジスタ(パワーオンリセット状態)</td><td>0001 0000b</td></tr>
    <tr><td rowspan="5">割り込み制御レジスタ</td><td>IP_EX</td><td>E9h</td><td>拡張割り込み優先制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>IE_EX</td><td>E8h</td><td>拡張割り込みイネーブルレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>GPIO_IE</td><td>CFh</td><td>GPIO割り込みイネーブルレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>IP</td><td>B8h</td><td>割り込み優先制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>IE</td><td>A8h</td><td>割り込みイネーブルレジスタ</td><td>0000 0000b</td></tr>
    <tr><td rowspan="8">フラッシュROMレジスタ</td><td>ROM_DATA_H</td><td>8Fh</td><td>フラッシュROMデータレジスタ上位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>ROM_DATA_L</td><td>8Eh</td><td>フラッシュROMデータレジスタ上位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>ROM_DATA</td><td>8Eh</td><td>ROM_DATA_LとROM_DATA_Hで16ビットのSFR</td><td>xxxxh</td></tr>
    <tr><td>ROM_STATUS</td><td>86h</td><td>フラッシュROMステータスレジスタ(読み取り専用)</td><td>1000 0000b</td></tr>
    <tr><td>ROM_CTRL</td><td>86h</td><td>フラッシュROM制御レジスタ(書き込み専用)</td><td>0000 0000b</td></tr>
    <tr><td>ROM_ADDR_H</td><td>85h</td><td>フラッシュROMアドレスレジスタ上位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>ROM_ADDR_L</td><td>84h</td><td>フラッシュROMアドレスレジスタ下位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>ROM_ADDR</td><td>84h</td><td>ROM_ADDR_LとROM_ADDR_H formで16ビットのSFR</td><td>xxxxh</td></tr>
    <tr><td rowspan="24">ポート設定レジスタ</td><td>XBUS_SPEED</td><td>FDh</td><td>外部バス速度設定レジスタ</td><td>1111 1111b</td></tr>
    <tr><td>XBUS_AUX</td><td>FDh</td><td>外部バス補助設定レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>PIN_FUNC</td><td>CEh</td><td>ピン機能選択レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>P4_CFG</td><td>C7h</td><td>P4ポート設定レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>P5_IN</td><td>C7h</td><td>P5ポート入力レジスタ(読み取り専用)</td><td>0000 0000b</td></tr>
    <tr><td>PORT_CFG</td><td>C6h</td><td>ポート設定レジスタ</td><td>0000 1111b</td></tr>
    <tr><td rowspan="2">P0_PU</td><td rowspan="2">C5h</td><td>P0ポートプルアップイネーブルレジスタ(En_P0_Pullup=0)</td><td>0000 0000b</td></tr>
    <tr><td>P0ポートプルアップイネーブルレジスタ(En_P0_Pullup=1)</td><td>1111 1111b</td></tr>
    <tr><td>P0_DIR</td><td>C4h</td><td>P0ポート入出力制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>P4_PU</td><td>C3h</td><td>P4ポートプルアップイネーブルレジスタ</td><td>1111 1111b</td></tr>
    <tr><td>P4_DIR</td><td>C2h</td><td>P4ポート入出力制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>P4_IN</td><td>C1h</td><td>P4ポート入力レジスタ(読み取り専用)</td><td>1111 1111b</td></tr>
    <tr><td>P4_OUT</td><td>C0h</td><td>P4ポート出力レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>P3_PU</td><td>BFh</td><td>P3ポート入出力制御レジスタ</td><td>1111 1111b</td></tr>
    <tr><td>P3_DIR</td><td>BEh</td><td>P3ポートプルアップイネーブルレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>P2_PU</td><td>BDh</td><td>P2ポートプルアップイネーブルレジスタ</td><td>1111 1111b</td></tr>
    <tr><td>P2_DIR</td><td>BCh</td><td>P2ポート入出力制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>P1_PU</td><td>BBh</td><td>P1ポートプルアップイネーブルレジスタ</td><td>1111 1111b</td></tr>
    <tr><td>P1_DIR</td><td>BAh</td><td>P1ポート入出力制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>P1_IE</td><td>B9h</td><td>P1ポート入力イネーブルレジスタ</td><td>1111 1111b</td></tr>
    <tr><td>P3</td><td>B0h</td><td>P3ポート入出力レジスタ</td><td>1111 1111b</td></tr>
    <tr><td>P2</td><td>A0h</td><td>P2ポート入出力レジスタ</td><td>1111 1111b</td></tr>
    <tr><td>P1</td><td>90h</td><td>P1ポート入出力レジスタ</td><td>1111 1111b</td></tr>
    <tr><td>P0</td><td>80h</td><td>P0ポート入出力レジスタ</td><td>1111 1111b</td></tr>   
    <tr><td rowspan="6">Timer/Counter0, 1レジスタ</td><td>TH1</td><td>8Dh</td><td>Timer1カウント上位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>TH0</td><td>8Ch</td><td>Timer0カウント上位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>TL1</td><td>8Bh3</td><td>Timer1カウント下位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>TL0</td><td>8Ah</td><td>Timer0カウント下位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>TMOD</td><td>89h</td><td>Timer0/1モードレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>TCON</td><td>88h</td><td>Timer0/1制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td rowspan="2">UART0レジスタ</td><td>SBUF</td><td>99h</td><td>UART0データレジスタ</td><td>xxxx xxxxb</td></tr>
    <tr><td>SCON</td><td>98h</td><td>UART0制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td rowspan="11">Timer/Counter2レジスタ</td><td>TH2</td><td>CDh</td><td>Timer2カウンタ上位バイト</td><td>0000 0000b</td></tr>
    <tr><td>TL2</td><td>CCh</td><td>Timer2カウンタ下位バイト</td><td>0000 0000b</td></tr>
    <tr><td>T2COUNT</td><td>CCh</td><td>TL2とTH2で16ビットSFR</td><td>0000h</td></tr>
    <tr><td>T2CAP1H</td><td>CDh</td><td>Timer2取得1データ上位バイト(読み取り専用)</td><td>xxxx xxxxb</td></tr>
    <tr><td>T2CAP1L</td><td>CCh</td><td>Timer2取得1データ下位バイト(読み取り専用)</td><td>xxxx xxxxb</td></tr>
    <tr><td>T2CAP1</td><td>CCh</td><td>T2CAP1LとT2CAP1Hで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>RCAP2H</td><td>CBh</td><td>Countリロード/取得2データレジスタ上位バイト</td><td>0000 0000b</td></tr>
    <tr><td>RCAP2L</td><td>CAh</td><td>Countリロード/取得2データレジスタ下位バイト</td><td>0000 0000b</td></tr>
    <tr><td>RCAP2</td><td>CAh</td><td>RCAP2LとRCAP2Hで16ビットSFR</td><td>0000h</td></tr>
    <tr><td>T2MOD</td><td>C9h</td><td>Timer2モードレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>T2CON</td><td>C8h</td><td>Timer2制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td rowspan="19">Timer/Counter3レジスタ</td><td>T3_FIFO_H</td><td>AFh</td><td>Timer3 FIFO上位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>T3_FIFO_L</td><td>AEh</td><td>Timer3 FIFO下位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>T3_FIFO</td><td>AEh</td><td>T3_FIFO_LとT3_FIFO_Hで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>T3_DMA_AH</td><td>ADh</td><td>DMAカレントバッファアドレス上位バイト</td><td>0000 xxxxb</td></tr>
    <tr><td>T3_DMA_AL</td><td>ACh</td><td>DMAカレントバッファアドレス下位バイト</td><td>xxxx xxx0b</td></tr>
    <tr><td>T3_DMA</td><td>ACh</td><td>T3_DMA_ALとT3_DMA_AHで16ビットSFR</td><td>0xxxh</td></tr>
    <tr><td>T3_DMA_CN</td><td>ABh</td><td>DMA残量カウントレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>T3_CTRL</td><td>AAh</td><td>Timer3制御レジスタ</td><td>0000 0010b</td></tr>
    <tr><td>T3_STAT</td><td>A9h</td><td>Timer3ステータスレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>T3_END_H</td><td>A7h</td><td>Timer3最終カウント上位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>T3_END_L</td><td>A6h</td><td>Timer3最終カウント下位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>T3_END</td><td>A6h</td><td>T3_END_LとT3_END_Hで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>T3_COUNT_H</td><td>A5h</td><td>Timer3カレントカウント上位バイト(読み取り専用)</td><td>0000 0000b</td></tr>
    <tr><td>T3_COUNT_L</td><td>A4h</td><td>Timer3カレントカウント下位バイト(読み取り専用)</td><td>0000 0000b</td></tr>
    <tr><td>T3_COUNT</td><td>A4h</td><td>T3_COUNT_LとT3_COUNT_Hで16ビットSFR</td><td>0000h</td></tr>
    <tr><td>T3_CK_SE_H</td><td>A5h</td><td>Timer3クロック分周器設定上位バイト</td><td>0000 0000b</td></tr>
    <tr><td>T3_CK_SE_L</td><td>A4h</td><td>Timer3クロック分周器設定下位バイト</td><td>0010 0000b</td></tr>
    <tr><td>T3_CK_SE</td><td>A4h</td><td>T3_CK_SE_LとT3_CK_SE_Hで16ビットSFR</td><td>0020h</td></tr>
    <tr><td>T3_SETUP</td><td>A3h</td><td>Timer3設定レジスタ</td><td>0000 0100b</td></tr>
    <tr><td rowspan="5">PWM1, PWM2レジスタ</td><td>PWM_CYCLE</td><td>9Fh</td><td>PWM周期レジスタ</td><td>xxxx xxxxb</td></tr>
    <tr><td>PWM_CK_SE</td><td>9Eh</td><td>PWMクロック分周設定レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>PWM_CTRL</td><td>9Dh</td><td>PWM制御レジスタ</td><td>0000 0010b</td></tr>
    <tr><td>PWM_DATA</td><td>9Ch</td><td>PWM1データレジスタ</td><td>xxxx xxxxb</td></tr>
    <tr><td>PWM_DATA2</td><td>9Bh</td><td>PWM2データレジスタ</td><td>xxxx xxxxb</td></tr>
    <tr><td rowspan="6">SPI0レジスタ</td><td>SPI0_SETUP</td><td>FCh</td><td>SPI0設定レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>SPI0_S_PRE</td><td>FBh</td><td>SPI0スレーブモード プリセットデータレジスタ</td><td>0010 0000b</td></tr>
    <tr><td>SPI0_CK_SE</td><td>FBh</td><td>SPI0分周クロック設定レジスタ</td><td>0010 0000b</td></tr>
    <tr><td>SPI0_CTRL</td><td>FAh</td><td>SPI0制御レジスタ</td><td>0000 0010b</td></tr>
    <tr><td>SPI0_DATA</td><td>F9h</td><td>SPI0データトランシーバーレジスタ</td><td>xxxx xxxxb</td></tr>
    <tr><td>SPI0_STAT</td><td>F8h</td><td>SPI0ステータスレジスタ</td><td>0000 1000b</td></tr>
    <tr><td rowspan="4">SPI1レジスタ</td><td>SPI1_CK_SE</td><td>B7h</td><td>SPI1分周クロック設定レジスタ</td><td>0010 0000b</td></tr>
    <tr><td>SPI1_CTRL</td><td>B6h</td><td>SPI1制御レジスタ</td><td>0000 0010b</td></tr>
    <tr><td>SPI1_DATA</td><td>B5h</td><td>SPI1データトランシーバーレジスタ</td><td>xxxx xxxxb</td></tr>
    <tr><td>SPI1_STAT</td><td>B4h</td><td>SPI1ステータスレジスタ</td><td>0000 1000b</td></tr>
    <tr><td rowspan="12">UART1レジスタ</td><td>SER1_DLL</td><td>9Ah</td><td>UART1ボーレート分周ラッチ下位バイト</td><td>xxxx xxxxb</td></tr>
    <tr><td>SER1_FIFO</td><td>9Ah</td><td>UART1データFIFO読み書きレジスタ</td><td>xxxx xxxxb</td></tr>
    <tr><td>SER1_DIV</td><td>97h</td><td>UART1プリスケーラ除数レジスタ</td><td>0xxx xxxxb</td></tr>
    <tr><td>SER1_ADDR</td><td>97h</td><td>UART1バスアドレスプリロードレジスタ</td><td>1111 1111b</td></tr>
    <tr><td>SER1_MSR</td><td>96h</td><td>UART1モデムステータスレジスタ(読み取り専用)</td><td>1111 0000b</td></tr>
    <tr><td>SER1_LSR</td><td>95h</td><td>UART1回線ステータスレジスタ(読み取り専用)</td><td>0110 0000b</td></tr>
    <tr><td>SER1_MCR</td><td>94h</td><td>UART1モデム制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>SER1_LCR</td><td>93h</td><td>UART1回線制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>SER1_IIR</td><td>92h</td><td>UART1割り込み識別レジスタ(読み取り専用)</td><td>0000 0001b</td></tr>
    <tr><td>SER1_FCR</td><td>92h</td><td>FIFO制御レジスタ(書き込み専用)</td><td>0000 0000b</td></tr>
    <tr><td>SER1_DLM</td><td>91h</td><td>UART1ボーレート分周ラッチ上位バイト</td><td>1000 0000b</td></tr>
    <tr><td>SER1_IER</td><td>91h</td><td>UART1割り込みイネーブルレジスタ</td><td>0000 0000b</td></tr>
    <tr><td rowspan="13">ADCレジスタ</td><td>ADC_EX_SW</td><td>F7h</td><td>ADC拡張アナログスイッチ制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>ADC_SETUP</td><td>F6h</td><td>ADC設定レジスタ</td><td>0000 1000b</td></tr>
    <tr><td>ADC_FIFO_H</td><td>F5h</td><td>ADCのFIFO上位バイト(読み取り専用)</td><td>0000 0xxxb</td></tr>
    <tr><td>ADC_FIFO_L</td><td>F4h</td><td>ADCのFIFO下位バイト(読み取り専用)</td><td>xxxx xxxxb</td></tr>
    <tr><td>ADC_FIFO</td><td>F4h</td><td>ADC_FIFO_LとADC_FIFO_Hで16ビットSFR</td><td>0xxxh</td></tr>
    <tr><td>ADC_CHANN</td><td>F3h</td><td>ADCチャネルセレクトレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>ADC_CTRL</td><td>F2h</td><td>ADC制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>ADC_STAT</td><td>F1h</td><td>ADCステータスレジスタ</td><td>0000 0100b</td></tr>
    <tr><td>ADC_CK_SE</td><td>EFh</td><td>ADC分周クロック設定レジスタ</td><td>0001 0000b</td></tr>
    <tr><td>ADC_DMA_CN</td><td>EEh</td><td>DMA残量レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>ADC_DMA_AH</td><td>EDh</td><td>DMAカレントバッファアドレス上位バイト</td><td>0000 xxxxb</td></tr>
    <tr><td>ADC_DMA_AL</td><td>ECh</td><td>DMAカレントバッファアドレス下位バイト</td><td>xxxx xxx0b</td></tr>
    <tr><td>ADC_DMA</td><td>ECh</td><td>ADC_DMA_ALとADC_DMA_AHで16ビットSFR</td><td>0xxxh</td></tr>
    <tr><td rowspan="29">USBレジスタ</td><td>USB_DMA_AH</td><td>E7h</td><td>DMAカレントバッファアドレス上位バイト(読み取り専用)</td><td>000x xxxxb</td></tr>
    <tr><td>USB_DMA_AL</td><td>E6h</td><td>DMAカレントバッファアドレス下位バイト(読み取り専用)</td><td>xxxx xxx0b</td></tr>
    <tr><td>USB_DMA</td><td>E6h</td><td>USB_DMA_ALとUSB_DMA_AHで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>UHUB1_CTRL</td><td>E5h</td><td>USBホストHUB1ポート制御レジスタ</td><td>1100 x000b</td></tr>
    <tr><td>UHUB0_CTRL</td><td>E4h</td><td>USBホストHUB0ポート制御レジスタ</td><td>0100 x000b</td></tr>
    <tr><td>UDEV_CTRL</td><td>E4h</td><td>USBデバイスポート制御レジスタ</td><td>0100 x000b</td></tr>
    <tr><td>USB_DEV_AD</td><td>E3h</td><td>USBデバイスアドレスレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>USB_CTRL</td><td>E2h</td><td>USB制御レジスタ</td><td>0000 0110b</td></tr>
    <tr><td>USB_INT_EN</td><td>E1h</td><td>USB割り込みイネーブルレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UEP4_T_LEN</td><td>DFh</td><td>エンドポイント4送信サイズレジスタ</td><td>0xxx xxxxb</td></tr>
    <tr><td>UEP4_CTRL</td><td>DEh</td><td>エンドポイント4制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UEP0_T_LEN</td><td>DDh</td><td>エンドポイント0送信サイズレジスタ</td><td>0xxx xxxxb</td></tr>
    <tr><td>UEP0_CTRL</td><td>DCh</td><td>エンドポイント0制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>USB_HUB_ST</td><td>DBh</td><td>SBホストHUBポートステータスレジスタ(読み取り専用)</td><td>0000 0000b</td></tr>
    <tr><td>USB_MIS_ST</td><td>DAh</td><td>SB関連ステータスレジスタ(読み取り専用)</td><td>xx10 1000b</td></tr>
    <tr><td>USB_INT_ST</td><td>D9h</td><td>USB割り込みステータスレジスタ(読み取り専用)</td><td>00xx xxxxb</td></tr>
    <tr><td>USB_INT_FG</td><td>D8h</td><td>USB割り込みフラグレジスタ</td><td>0010 0000b</td></tr>
    <tr><td>UEP3_T_LEN</td><td>D7h</td><td>エンドポイント3送信サイズレジスタ</td><td>0xxx xxxxb</td></tr>
    <tr><td>UH_TX_LEN</td><td>D7h</td><td>USBホスト送信サイズレジスタ</td><td>0xxx xxxxb</td></tr>
    <tr><td>UEP3_CTRL</td><td>D6h</td><td>エンドポイント3制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UH_TX_CTRL</td><td>D6h</td><td>USBホストエンドポイント送信制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UEP2_T_LEN</td><td>D5h</td><td>エンドポイント2送信サイズレジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UH_EP_PID</td><td>D5h</td><td>USBホストトークン設定レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UEP2_CTRL</td><td>D4h</td><td>エンドポイント2制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UH_RX_CTRL</td><td>D4h</td><td>USBホスト受信エンドポイント制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UEP1_T_LEN</td><td>D3h</td><td>エンドポイント1送信サイズレジスタ</td><td>0xxx xxxxb</td></tr>
    <tr><td>UEP1_CTRL</td><td>D2h</td><td>エンドポイント1制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UH_SETUP</td><td>D2h</td><td>USBホスト補助設定レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>USB_RX_LEN</td><td>D1h</td><td>USB受信サイズレジスタ(読み取り専用)</td><td>0xxx xxxxb</td></tr>
    <tr><td rowspan="22">USB xSFRレジスタ</td><td>UEP4_1_MOD</td><td>2446h</td><td>エンドポイント1, 4モード制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UEP2_3_MOD</td><td>2447h</td><td>エンドポイント2, 3モード制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UH_EP_MOD</td><td>2447h</td><td>USBホストエンドポイントモード制御レジスタ</td><td>0000 0000b</td></tr>
    <tr><td>UEP0_DMA_H</td><td>2448h</td><td>エンドポイント0, 4バッファ開始アドレス上位バイト</td><td>000x xxxxb</td></tr>
    <tr><td>UEP0_DMA_L</td><td>2449h</td><td>エンドポイント0, 4バッファ開始アドレス下位バイト</td><td>xxxx xxx0b</td></tr>
    <tr><td>UEP0_DMA</td><td>2448h</td><td>UEP0_DMA_LとUEP0_DMA_Hで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>UEP1_DMA_H</td><td>244Ah</td><td>エンドポイント1バッファ開始アドレス上位バイト</td><td>000x xxxxb</td></tr>
    <tr><td>UEP1_DMA_L</td><td>244Bh</td><td>エンドポイント1バッファ開始アドレス下位バイト</td><td>xxxx xxx0b</td></tr>
    <tr><td>UEP1_DMA</td><td>244Ah</td><td>UEP1_DMA_LとUEP1_DMA_Hで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>UEP2_DMA_H</td><td>244Ch</td><td>エンドポイント2バッファ開始アドレス上位バイト</td><td>000x xxxxb</td></tr>
    <tr><td>UEP2_DMA_L</td><td>244Dh</td><td>エンドポイント2バッファ開始アドレス下位バイト</td><td>xxxx xxx0b</td></tr>
    <tr><td>UEP2_DMA</td><td>244Ch</td><td>UEP2_DMA_LとUEP2_DMA_Hで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>UH_RX_DMA_H</td><td>244Ch</td><td>USBホスト受信開始アドレス上位バイト</td><td>000x xxxxb</td></tr>
    <tr><td>UH_RX_DMA_L</td><td>244Dh</td><td>USBホスト受信開始アドレス下位バイト</td><td>xxxx xxx0b</td></tr>
    <tr><td>UH_RX_DMA</td><td>244Ch</td><td>UH_RX_DMA_LとUH_RX_DMA_Hで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>UEP3_DMA_H</td><td>244Eh</td><td>エンドポイント3バッファ開始アドレス上位バイト</td><td>000x xxxxb</td></tr>
    <tr><td>UEP3_DMA_L</td><td>244Fh</td><td>エンドポイント3バッファ開始アドレス下位バイト</td><td>xxxx xxx0b</td></tr>
    <tr><td>UEP3_DMA</td><td>244Eh</td><td>UEP3_DMA_LとUEP3_DMA_Hで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>UH_TX_DMA_H</td><td>244Eh</td><td>USBホスト送信バッファ開始アドレス上位バイト</td><td>000x xxxxb</td></tr>
    <tr><td>UH_TX_DMA_L</td><td>244Fh</td><td>USBホスト送信バッファ開始アドレス下位バイト</td><td>xxxx xxx0b</td></tr>
    <tr><td>UH_TX_DMA</td><td>244Eh</td><td>UH_TX_DMA_LとUH_TX_DMA_Hで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>pU*</td><td>254*h</td><td>bXIR_XSFRを1に設定した後、この名前を使って上記のxSFRをpdata型でアドレス指定することで、xdata型でアドレス指定するよりも高速になります。</td><td></td></tr>
    <tr><td rowspan="13">LEDコントロールカードxSFRレジスタ</td><td>LED_STAT</td><td>2880h</td><td>LEDステータスレジスタ</td><td>010x 0000b</td></tr>
    <tr><td>LED_CTRL</td><td>2881h</td><td>LED制御レジスタ</td><td>0000 0010b</td></tr>
    <tr><td>LED_FIFO_CN</td><td>2882h</td><td>FIFOカウントステータスレジスタ(読み取り専用)</td><td>0000 0000b</td></tr>
    <tr><td>LED_DATA</td><td>2882h</td><td>LEDデータレジスタ(書き込み専用)</td><td>xxxx xxxxb</td></tr>
    <tr><td>LED_CK_SE</td><td>2883h</td><td>LEDクロック分周器設定レジスタ</td><td>0001 0000b</td></tr>
    <tr><td>LED_DMA_AH</td><td>2884h</td><td>DMAカレントバッファアドレス上位バイト</td><td>000x xxxxb</td></tr>
    <tr><td>LED_DMA_AL</td><td>2885h</td><td>DMAカレントバッファアドレス下位バイト</td><td>xxxx xxx0b</td></tr>
    <tr><td>LED_DMA</td><td>2884h</td><td>LED_DMA_ALとLED_DMA_AHで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>LED_DMA_CN</td><td>2886h</td><td>LED DMA残量レジスタ</td><td>xxxx xxxxb</td></tr>
    <tr><td>LED_DMA_XH</td><td>2888h</td><td>DMAカレント補助バッファアドレス上位バイト</td><td>000x xxxxb</td></tr>
    <tr><td>LED_DMA_XL</td><td>2889h</td><td>DMAカレント補助バッファアドレス下位バイト</td><td>xxxx xxx0b</td></tr>
    <tr><td>LED_DMA_X</td><td>2888h</td><td>LED_DMA_XLとLED_DMA_XHで16ビットSFR</td><td>xxxxh</td></tr>
    <tr><td>pLED_*</td><td>298*h</td><td>bXIR_XSFRを1に設定した後、この名前を使って上記のxSFRをpdata型でアドレス指定することで、xdata型でアドレス指定するよりも高速になります。</td><td></td></tr>
</table>

## 5.3 汎用8051レジスタ

<div>
    <p align="center">表 5.3.1 汎用8051レジスタ一覧</p>
</div>

<table>
    <tr>
        <th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>B</td><td>F0h</td><td>Bレジスタ</td><td>00h</td></tr>
    <tr><td>A, ACC</td><td>E0h</td><td>アキュムレータ</td><td>00h</td></tr>
    <tr><td>PSW</td><td>D0h</td><td>プログラムステータスレジスタ	</td><td>00h</td></tr>
    <tr><td rowspan="2">GLOBAL_CFG</td><td rowspan="2">B1h</td><td>グローバルコンフィグレーションレジスタ(ブートローダ状態)	</td><td>E0h</td></tr>
    <tr><td>グローバルコンフィグレーションレジスタ(アプリケーション状態)</td><td>C0h</td></tr>
    <tr><td>CHIP_ID</td><td>A1h</td><td>チップID(読み取り専用)</td><td>59h</td></tr>
    <tr><td>SAFE_MOD</td><td>A1h</td><td>セーフモード制御レジスタ(書き込み専用)</td><td>00h</td></tr>
    <tr><td>PCON</td><td>87h</td><td>電源制御レジスタ(パワーオンリセット状態)</td><td>10h</td></tr>
    <tr><td>DPH</td><td>83h</td><td>データアドレスポインタ上位8ビット</td><td>00h</td></tr>
    <tr><td>DPL</td><td>82h</td><td>データアドレスポインタ下位8ビット</td><td>00h</td></tr>
    <tr><td>DPTR</td><td>82h</td><td>DPLとDPHは16ビットSFR</td><td>0000h</td></tr>
    <tr><td>SP</td><td>81h</td><td>スタックポインタ</td><td>07h</td></tr>
    
</table>

### Bレジスタ (B):
<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>B</td><td>RW</td><td>算術演算レジスタ。主に掛け算や割り算に使われます。ビット単位でのアクセス可能</td><td>00h</td></tr>
</table>

### Aアキュムレータ(A, ACC):
<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>A/ACC</td><td>RW</td><td>算術アキュムレータ。ビット単位でのアクセス可能</td><td>00h</td></tr>
</table>

### プログラム状態レジスタ(PSW):
<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>CY</td><td>RW</td><td><p>キャリーフラグ: 算術演算や論理演算を行う際に、最上位ビットのキャリーまたは借用を記録するために使用します。</p><p>8ビット加算時には最上位ビットがセットされ、それ以外の場合はクリアされます。</p><p>8ビット減算時に借用の場合はビットがセットされ、それ以外の場合はクリアされます。</p><p>ロジック命令は、ビットをビットにしたり、クリアにしたりすることができます。</p></td><td>0</td></tr>
    <tr><td>6</td><td>AC</td><td>RW</td><td>補助キャリーフラグ: 記録・減算時、下位4ビットに上位4ビットからのキャリーまたは借用があり、ACがセットされ、それ以外の場合はクリアされる。</td><td>0</td></tr>
    <tr><td>5</td><td>F0</td><td>RW</td><td>ユニバーサルフラグビットでビット単位でのアクセス可能。0: ユーザーが自分で定義することができ、ソフトウェアによってクリアまたは設定することができます</td><td>0</td></tr>
    <tr><td>4</td><td>RS1</td><td>RW</td><td>レジスタバンク選択(HIGHで選択)</td><td>0</td></tr>
    <tr><td>3</td><td>RS0</td><td>RW</td><td>レジスタバンク選択(LOWで選択)</td><td>0</td></tr>
    <tr><td>2</td><td>OV</td><td>RW</td><td>オーバーフローフラグ: 足し算・引き算時に演算結果が8桁の2進数を超えた場合に、OVは1に設定されます。そうでなければクリアして0</td><td>0</td></tr>
    <tr><td>1</td><td>F1</td><td>RW</td><td>ユニバーサルフラグでビット単位でのアクセス可能。1: ユーザーが自分で定義することができ、ソフトウェアによってクリアまたは設定することができます</td><td>0</td></tr>
    <tr><td>0</td><td>P</td><td>R0</td><td>パリティフラグ: 命令実行後、アキュムレータAに1のパリティを記録する。P1は奇数のとき1でPは偶数のとき1</td><td>0</td></tr>
</table>

プロセッサの状態はステータスレジスタPSWに格納され、PSWはビット単位のアクセスをサポートしています。ステータスワードにはキャリーフラグが含まれています。BCDコード処理のための補助キャリーフラグ。パリティフラグを指定します。オーバーフローフラグを指定します。そしてRS0, RS1を使用してワーキングレジスタのバンク選択を行います。ワーキングレジスタセットが配置されている領域は、直接または間接的にアクセスすることができます。

<div>
    <p align="center">表 5.3.2 RS1とRS0のワーキングレジスタグループ選択テーブル</p>
</div>

<table>
    <tr>
        <th>RS1</th><th>RS0</th><th>ワーキングレジスタセット</th>
    </tr>
    <tr><td>0</td><td>0</td><td>グループ0 (00h-07h)</td></tr>
    <tr><td>0</td><td>1</td><td>グループ1 (08h-0Fh)</td></tr>
    <tr><td>1</td><td>0</td><td>グループ2 (10h-17h)</td></tr>
    <tr><td>1</td><td>1</td><td>グループ3 (18h-1Fh)</td></tr>
</table>

<div>
    <p align="center">表 5.3.3 フラグビットに影響を与える操作(Xはフラグビットが操作結果に関係していることを示す)</p>
</div>

<table>
    <tr>
        <th>オペレーション</th><th>CY</th><th>OV</th><th>AC</th>
    </tr>
    <tr><td>ADD</td><td>X</td><td>X</td><td>X</td></tr>
    <tr><td>ADDC</td><td>X</td><td>X</td><td>X</td></tr>
    <tr><td>SUBB</td><td>X</td><td>X</td><td>X</td></tr>
    <tr><td>MUL</td><td>0</td><td>X</td><td></td></tr>
    <tr><td>DIV</td><td>0</td><td>X</td><td></td></tr>
    <tr><td>DA A</td><td>X</td><td></td><td></td></tr>
    <tr><td>RRC A</td><td>X</td><td></td><td></td></tr>
    <tr><td>RLC A</td><td>X</td><td></td><td></td></tr>
    <tr><td>CJNE</td><td>X</td><td></td><td></td></tr>
    <tr><td>SETB C</td><td>1</td><td></td><td></td></tr>
    <tr><td>CLR C</td><td>0</td><td></td><td></td></tr>
    <tr><td>CPL C</td><td>X</td><td></td><td></td></tr>
    <tr><td>MOV C, bit</td><td>X</td><td></td><td></td></tr>
    <tr><td>ANL C, bit</td><td>X</td><td></td><td></td></tr>
    <tr><td>ANL C,/bit</td><td>X</td><td></td><td></td></tr>
    <tr><td>ORL C, bit</td><td>X</td><td></td><td></td></tr>
    <tr><td>ORL C,/bit</td><td>X</td><td></td><td></td></tr>
</table>

### データアドレスポインタ(DPTR):
<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>DPL</td><td>RW</td><td>データポインタ下位バイト</td><td>00h</td></tr>
    <tr><td>[7:0]</td><td>DPH</td><td>RW</td><td>データポインタ上位バイト</td><td>00h</td></tr>
</table>

DPLとDPHはxSFR, xBUS, xRAMデータメモリまたはプログラムメモリにアクセスするための16ビットデータポインターDPTRを形成します。実際のDPTRは、DPTR0及びDPTR1の物理的な16ビットデータポインタに対応する、XBUS_AUX内のDPSによって動的に選択されます。

### スタックポインタ(SP):
<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>SP</td><td>RW</td><td>スタックポインタ。主にプログラムの呼び出しや割り込み呼び出し、スタックへのデータの入出力に使用されます。</td><td>07h</td></tr>
</table>

スタック固有の機能: エンドポイントを保護し、サイトを保護し、先入れ先出し順に管理します。スタックが追加されると、SPポインタが自動的に1インクリメントされ、データまたはブレークポイント情報が保存されます。スタックを取得する、SPポインタはデータユニットを指し、SPポインタは自動的に1だけデクリメントされます。リセット後のSPの初期値は07hで、対応するデフォルトのスタック保存は08hから始まります。

## 5.4 特殊レジスタ

グローバル構成レジスタ(GLOBAL_CFG)、セーフモードでのみ書き込み可能。

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:6]</td><td>Reserved</td><td>RO</td><td>11に固定</td><td>11b</td></tr>
    <tr><td>5</td><td>bBOOT_LOAD</td><td>RO</td><td><p>ブートローダステータスビットは、ISPブートローダの状態とアプリケーションの状態を区別するために使用します。
電源投入時に設定され、ソフトウェアがリセットされると 0 にクリアされます。</p><p>ISPブートローダを搭載したチップの場合、本ビットが1の場合、ソフトウェアがリセットされていないことを示します。通常は、電源投入後に実行されたISPブートローダの状態です。このビットは0で、ソフトウェアがリセットされたことを示します。通常はアプリケーション状態</p></td><td>1</td></tr>
    <tr><td>4</td><td>bSW_RESET</td><td>RW</td><td>ソフトウェアリセット制御ビット: 1に設定するとソフトウェアリセットが行われます。ハードウェアオートゼロ</td><td>0</td></tr>
    <tr><td>3</td><td>bCODE_WE</td><td>RW</td><td>フラッシュROM書き込みイネーブルビット。このビットは書き込み禁止の場合は0です。フラッシュROM書き込みや消去可能な場合は1</td><td>0</td></tr>
    <tr><td>2</td><td>bDATA_WE</td><td>RW</td><td>フラッシュROMのデータフラッシュ領域書き込みイネーブルビットです。このビットは書き込み禁止の場合は0です。データフラッシュ領域の書き込みや消去可能な場合は1</td><td>0</td></tr>
    <tr><td>1</td><td>bXIR_XSFR</td><td>RW</td><td><p>MOVX_@R0/R1命令アクセス範囲制御ビット:</p><p>本ビットは0で全てのxdata領域xRAM/xBUS/xSFRへのアクセスを許可します。</p><p>本ビットは1でxSFRへのアクセスを許可します、xRAM/xBUSへのアクセスはできません。</p></td><td>0</td></tr>
    <tr><td>0</td><td>bWDOG_EN</td><td>RW</td><td>ウォッチドッグリセットイネーブルビット: 本ビットは0でウォッチドッグはタイマーとしてしか使われていません。このビットは1で、タイマーがオーバーフローしたときにウォッチドッグリセットを許可します。</td><td>0</td></tr>
</table>

### チップID(CHIP_ID):
<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>CHIP_ID</td><td>RO</td><td>識別チップ固定値59h</td><td>59h</td></tr>
</table>

### セーフモード制御レジスタ(SAFE_MOD):
<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アドレス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>[7:0]</td><td>SAFE_MOD</td><td>WO</td><td>セーフモードに入るか終了するために使用します。</td><td>00h</td></tr>
</table>

**SFR の中には、セーフモードでしかデータを書き込めず、非セキュアモードでは常に読み取り専用となるものがあります。セーフモードに入るための手順:**

1. 55hをレジスタに書き込む。
2. そして、レジスタにAAhを書き込みます。
3. その後、約13～23回のシステム主周波数サイクルがセーフモードとなり、有効期間中に１つ以上のセキュリティクラスSFRまたは通常SFRを書き換えることができる。
4. 上記の有効期間後に自動的にセーフモードを終了させます。
5. または、このレジスタに任意の値を書き込むことで、セーフモードを早期に終了させることができます。
