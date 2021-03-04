---
title: 11. xBUS外部バス
type: docs
weight: 11
BookToC: false
---

# 11. xBUS外部バス

## 11.1 外部バスレジスタ

### 外部バス補助設定レジスタ(XBUS_AUX):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bUART0_TX</td><td>R0</td><td>UART0の送信状態を表示します。1は送信中を意味します。</td><td>0</td></tr>
    <tr><td>6</td><td>bUART0_RX</td><td>R0</td><td>UART0の受信状態を表示します。1は受信中を意味します。</td><td>0</td></tr>
    <tr><td>5</td><td>bSAFE_MOD_ACT</td><td>R0</td><td>セーフモードの状態を表示します。1は現在セーフモードであることを意味します。</td><td>0</td></tr>
    <tr><td>4</td><td>bALE_CLK_EN</td><td>RW</td><td>ALEピンクロック出力イネーブル <br />1: xBUS動作がない時にALEがシステムの主周波数12、つまりFsys/12を出力します。<br />0: クロック信号の出力を無効にし、EMIを低減するために外部バス下位8ビットアドレスラッチ信号にアクセスするために必要な時だけ出力します。</td><td>0</td></tr>
    <tr><td>3</td><td>GF2</td><td>RW</td><td>一般フラグビット2。ユーザーは自分で定義することができます。ソフトウェアでクリアまたは設定できます。</td><td>0</td></tr>
    <tr><td>2</td><td>bDPTR_AUTO_INC</td><td>RW</td><td>MOVX_@DPTR命令が完了した後、DPTRをオートインクリメントすることを有効にする。</td><td>0</td></tr>
    <tr><td>1</td><td>reserved</td><td>R0</td><td>予約</td><td>0</td></tr>
    <tr><td>0</td><td>DPS</td><td>RW</td><td><p>デュアルDPTRデータポインタ選択ビットです。<br />0: DPTR0を選択します。<br />1: DPTR1を選択します。</td><td>0</td></tr>
    
</table>


### 外部バス速度設定レジスタ(XBUS_SPEED):

<table>
    <tr>
        <th>ビット</th><th>名前</th><th>アクセス</th><th>概要</th><th>リセット値</th>
    </tr>
    <tr><td>7</td><td>bXBUS1_SETUP</td><td>RW</td><td>XBUS1の設定時間を選択します。<br />0: 2クロックサイクル<br />1: 3クロックサイクル</td><td>1</td></tr>
    <tr><td>6</td><td>bXBUS1_HOLD</td><td>RW</td><td>XBUS1のホールド時間を選択します。<br />0: 1クロックサイクル<br />1: 2クロックサイクル</td><td>1</td></tr>
    <tr><td>5</td><td>bXBUS1_WIDTH1</td><td>RW</td><td>XBUS1バスHIGHパルス幅</td><td>1</td></tr>
    <tr><td>4</td><td>bXBUS1_WIDTH0</td><td>RW</td><td>XBUS1バスLOWパルス幅</td><td>1</td></tr>
    <tr><td>3</td><td>bXBUS0_SETUP</td><td>RW</td><td>XBUS0の設定時間を選択します。<br />0: 2クロックサイクル<br />1: 2クロックサイクル</td><td>1</td></tr>
    <tr><td>2</td><td>bXBUS0_HOLD</td><td>RW</td><td>XBUS0のホールド時間を選択します。<br />0: 1クロックサイクル<br />1: 2クロックサイクル</td><td>1</td></tr>
    <tr><td>1</td><td>bXBUS0_WIDTH1</td><td>RW</td><td>XBUS0バスHIGHパルス幅</td><td>1</td></tr>
    <tr><td>0</td><td>bXBUS0_WIDTH0</td><td>RW</td><td>XBUS0バスLOWパルス幅</td><td>1</td></tr>
</table>

## 11.2 外部バスピン

<div>
    <p align="center">表11.2.1 外部バスピン一覧</p>
</div>

<table>
    <tr>
        <th>GPIO</th><th>ダイレクトアドレスモードピン</th><th>マルチプレックスアドレスモードピン</th><th>概要</th>
    </tr>
    <tr><td>P3.7</td><td>RD</td><td>RD</td><td>外部バスリード信号出力ピン、アクティブLOW、立ち上がりエッジサンプリング入力</td></tr>
    <tr><td>P3.6</td><td>WR</td><td>WR</td><td>外部バス書き込み信号出力ピン、アクティブLOW</td></tr>
    <tr><td rowspan="2">P0.0~P0.7</td><td>D0~D7</td><td>D0~D7</td><td>8ビット双方向データバス</td></tr>
    <tr><td></td><td>A0~A7</td><td>マルチプレックス下位8ビットアドレスA[0:7]出力、ALEで制御される外部回路でラッチされます。</td></tr>
    <tr><td>P4.0~P4.5</td><td>A0~A5</td><td>未使用</td><td>ダイレクトバスアドレスA[0:5]出力ピン、P4_DIR出力を設定する必要があります。</td></tr>
    <tr><td>P3.5</td><td>A6</td><td>未使用</td><td>バスダイレクトアドレスA6出力ピン</td></tr>
    <tr><td rowspan="2">P2.7</td><td>A7</td><td></td><td>バスダイレクトアドレスA7出力ピン</td></tr>
    <tr><td></td><td>A15</td><td>バスアドレスA15出力ピン</td></tr>
    <tr><td>P2.0~P2.6</td><td>A8~A14</td><td>A8~A14</td><td>バスアドレスA[8:14]出力ピン</td></tr>
    <tr><td>P3.4</td><td>XCS0</td><td>XCS0</td><td>チップセレクト0出力ピン、アドレス範囲 4000h～7FFFh、アクティブロー</td></tr>
    <tr><td>P3.3</td><td>!A15</td><td>!A15</td><td>バスアドレスA15反転出力端子、チップセレクト1出力に相当、アドレス範囲8000h～FFFFh、アクティブロー、ALE無効状態でのみ使用可能</td></tr>
    <tr><td>P5.5</td><td>!A15</td><td>!A15</td><td>バスアドレスA15反転出力端子、チップセレクト1出力に相当、アドレス範囲8000h～FFFFh、アクティブロー、ALE有効状態でのみ使用可能</td></tr>
    <tr><td rowspan="2">P5.4</td><td></td><td>ALE</td><td>マルチプレックス下位8ビットアドレスラッチ制御出力ピン、アクティブハイ</td></tr>
    <tr><td>ALE</td><td></td><td>システム周波数12分割クロック Fsys/12出力ピン、デューティサイクル1/12</td></tr>
    
</table>


外部バス状態のアドレス出力やチップセレクト出力など、上記の未使用ピンの一部は、GPIOマルチプレックス優先順位に応じて他のモジュールで使用することができ、P4.0～P4.5の未使用ピンも使用することができます。P4_DIRを設定することで入力状態を保持します。

bXBUS_CS_OE = 1 の時。バスアドレスA15の反転信号を入力すると、ALEの出力状態に応じて出力端子が選択されます。ALEの出力が許可されている場合、!A15はP5.5からの出力を選択する。ALEが出力できない場合。!A15はP3.3出力を選択します。ALE出力状態はbUH1_DISABLE, bXBUS_EN, bXBUS_AL_OE, bALE_CLK_ENの組み合わせで決定されます。
<div>
    <p align="center">表11.2.2 P5.4ピンマルチプレックスALE出力ステータス表</p>
</div>

<table>
    <tr>
        <th>bUH1_DISABLE</th><th>bXBUS_EN</th><th>bXBUS_AL_OE</th><th>bALE_CLK_EN</th><th>P5.4ピン機能概要</th>
    </tr>
    <tr><td>0</td><td>x</td><td>x</td><td>x</td><td>ALE出力を無効にし、HMを優先(P5.5はHPとして使用)</td></tr>
    <tr><td>1</td><td>0</td><td>x</td><td>0</td><td>デフォルトではXBとして使用されるALE出力を無効にする(P5.5はXAとして使用)</td></tr>
    <tr><td>1</td><td>0</td><td>x</td><td>1</td><td>ALEはシステムの12周波クロック信号のみを出力します。</td></tr>
    <tr><td>1</td><td>1</td><td>1</td><td>0</td><td>デフォルトではXBとして使用されるALE出力を無効にする(P5.5はXAとして使用)</td></tr>
    <tr><td>1</td><td>1</td><td>1</td><td>1</td><td>ALEはシステムの12周波クロック信号のみを出力します。</td></tr>
    <tr><td>1</td><td>1</td><td>0</td><td>0</td><td>ALEはバス上でのみ下位8ビットのアドレスラッチ信号を出力します。</td></tr>
    <tr><td>1</td><td>1</td><td>0</td><td>1</td><td>ALEはバスアクセス時に下位8ビットのアドレスラッチ信号を出力し、アイドル時にはシステムの主周波数12周波数のクロック信号を出力します。</td></tr>
</table>