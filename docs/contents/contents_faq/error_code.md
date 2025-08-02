---
comments: true
---

# エラーコード

ここではAviUtl2で起こるエラーとその対処法をまとめています。
もし、このページに載っていない質問がある場合は、[コメント](#__comments)で質問をしてみてください。<br><br>

## failed to register input (output) plugin

!!! note ""
    `failed to register input plugin. [class Plugin::InputPipe32]
    CreateProcess() failed.`
    `failed to register output plugin. [class Plugin:OutputPipe32]
    CreateProcess() failed.`
    `failed to register output plugin. [class Plugin::OutputPipe32]
    load plugin failed.`

Windows DefenderなどのセキュリティソフトがAviUtl2のファイル（pipe32aui.exeなど）を誤検知し、隔離・削除している場合があります。「pipe32aui.exe」「pipe32auo.exe」が「aviutl2.exe」と同じフォルダ内にあるかを確認してください。<br>
セキュリティソフトに弾かれていた場合、「保護の履歴」などを確認し、該当ファイルを「許可」または「復元」してください。それが出来ない場合、「除外された領域」からインストール先のディレクトリを追加して再インストールしてください。

## D3D11CreateDevice() failed

!!! note ""
    `D3D11CreateDevice() failed.
    [Note]
    HRESULT: 0x887A0004
    指定されたデバイス インターフェイスまたは機能レベルがこのシステムでサポートされていません。
    [Place]
    Media::Direct3DService::Direct3DService()`

AVX2非対応のCPUのでAviutl2を起動した際に発生するようです。<br>
AVX2対応CPUの早見表:

|Intel Core|Intel Celeron|Intel Pentium|AMD|
|----|----|----|----|
|第4世代以降|第11世代以降|第11世代以降|Athlon X4 835以降|

## Direct3D.device->CreatePixelShader() failed

!!! note ""
    `Direct3D.device->CreatePixelShader() failed.
    [Note]
    HRESULT: 0x80070057
    パラメーターが間違っています。
    [Place]
    Media::PixelShader::loadFromMemory()`

ROV非対応のGPUでAviutl2を起動した際に発生するようです。<br>
ROV対応GPUの早見表:

|NVIDIA GeForce|AMD Radeon|Intel Graphics (CPU内蔵)|
|----|----|----|
|GTX 900番台以降|RX Vegaシリーズ以降|第5世代以降※|

<br>
※第4世代も対応しているが、最新ドライバを適用すると非対応化する（[脆弱性対策のため](https://www.nichepcgamer.com/archives/vulnerability-in-intel-4th-gen-haswell-core-4000-series-disable-directx12.html)）<br>
ROVは[DirectX Capabilities Viewer](https://github.com/microsoft/DxCapsViewer)などでも確認できます。

## font->GetStringLength() failed

!!! note ""
    `font->GetStringLength() failed.
    [Note]
    HRESULT: 0x80004005 エラーを特定できません
    [Place]
    Media::FontManager::registerFont()`

PCにインストールされた特定のフォントに起因した不具合です。<br>
現状特定できている原因となり得るフォントは「PingFang-SC」ファミリーのフォントと「迷你简艺黑」フォントです。該当するフォントがあればそれをアンインストールしてから再試行してみてください。もし該当するフォントがない場合、PC内のフォントを一度どこかに避難させ、一つずつ再インストールすることで原因となるフォントをを特定してください。<br>
中国語のフォントが引き起こしている可能性が高いですが、以下の中国語フォントはインストールされていてもエラーは起きていないことが確認されています。

- 簡体字
  - DengXian
  - FangSong
  - KaiTi
  - Microsoft YaHei
  - NSimSun
  - SimHei
  - SimSun
- 繁体字
  - DFKai-SB
  - Microsoft JhengHei
  - Microsoft Yi Baiti
  - MingLiU
  - PMingLiU

## Duplicate effects with the same key exist

!!! note ""
    `Failed to register effect. [(プラグイン名)]
    Duplicate effects with the same key exist.`

同じ効果名（表示名）を持つスクリプトを導入しようとすると発生するようです。<br>
拡張子が異なっていたり、@○○.anmに内包されている状態でも導入できません。名称を変更して再試行してみてください。
