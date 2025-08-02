# 開発環境の作成

!!! Warning "注意"
    Windows環境でのみ現状開発が可能です。  
    MacやLinuxの方は別途Windowsを用意するか、技術革新してください。

## 公式リンクからSDKをダウンロードする

[AviUtl Plugin SDK](https://spring-fragrance.mints.ne.jp/aviutl/aviutl_plugin_sdk.zip)  
これを任意のDirectoryに解凍して配置しておいてください。

---

## vscodeをインストールする

Windows10以降であれば、以下のコマンドをコマンドプロンプトに入力すればインストール可能です。

```
winget install Microsoft.VisualStudioCode
```

---

## vscode向けの拡張機能のインストール

[C/C++拡張](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)  
[CodeLLDB（デバッグ用）](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)  
[Makefile Tools（もしMakefile派なら）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.makefile-tools)  
[CMake Tools（CMakeでプロジェクト管理したい場合）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools)

上記リンクからインストール可能ですが、vscodeのターミナル上からも可能です。  

```
code --install-extension ms-vscode.cpptools  
```

```
code --install-extension vadimcn.vscode-lldb
```

```
code --install-extension ms-vscode.makefile-tools  
```

```
code --install-extension ms-vscode.cmake-tools  
```

---

## コンパイラのインストール

次はコンパイラのインストールを行います

まずMSYS2をインストールします(vscodeのターミナルで以下を実行)

```
winget install --id MSYS2.MSYS2 -e
```

次にMSYS2を実行します

![MSYS2_1](../../img/contents_script_dev/1.png)

![MSYS2_2](../../img/contents_script_dev/2.png)

このshell上で以下のコマンドを入力してください

```
pacman -S mingw-w64-x86_64-gcc
```

---

## コンパイラの環境変数を通す

![環境変数](../../img/contents_script_dev/3.png)

赤枠のように設定し、再度ターミナルを開き直して、

```
gcc --version
```

と入力して、バージョンが表示されたら、インストール完了です。

![gcc](../../img/contents_script_dev/4.png)

## ビルド用のjsonファイルを定義

まずはファイル構成を以下のように設定してください。

```
AviUtlPluginProject/
├─ .vscode/
│  └─ tasks.json        ← VSCodeでビルドボタン使う用
├─ aviutl_sdk/
│  └─ filter.h          ← AviUtl SDK のヘッダ（必要なやつだけ）
├─ src/
│  ├─ filter.c          ← フィルタプラグインならこれ
│  ├─ input.c           ← 入力プラグインならこっち
│  └─ ...（必要なソース） 
├─ build/
│  └─ （出力される `.auf` などが入る）
└─ README.md            ← 任意、説明ファイル
```

次に.vscode/tasks.jsonを以下のように編集してください。

```
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Build Filter Plugin (.auf)",
      "type": "shell",
      "command": "gcc",
      "args": [
        "-shared",
        "-o", "build/filter.auf",
        "src/filter.c",
        "-Iaviutl_sdk",
        "-mwindows"
      ],
      "group": "build",
      "problemMatcher": []
    },
    {
      "label": "Build Input Plugin (.aui)",
      "type": "shell",
      "command": "gcc",
      "args": [
        "-shared",
        "-o", "build/input.aui",
        "src/input.c",
        "-Iaviutl_sdk",
        "-mwindows"
      ],
      "group": "build",
      "problemMatcher": []
    },
    {
      "label": "Build Output Plugin (.auo)",
      "type": "shell",
      "command": "gcc",
      "args": [
        "-shared",
        "-o", "build/output.auo",
        "src/output.c",
        "-Iaviutl_sdk",
        "-mwindows"
      ],
      "group": "build",
      "problemMatcher": []
    },
    {
      "label": "Build Color Convert Plugin (.auc)",
      "type": "shell",
      "command": "gcc",
      "args": [
        "-shared",
        "-o", "build/colorconvert.auc",
        "src/colorconvert.c",
        "-Iaviutl_sdk",
        "-mwindows"
      ],
      "group": "build",
      "problemMatcher": []
    },
    {
      "label": "Build Language Resource Plugin (.aul)",
      "type": "shell",
      "command": "bash",
      "args": [
        "-c",
        "windres resource/resource.rc -O coff -o build/resource.o && gcc -shared -o build/langresource.aul build/resource.o -mwindows"
      ],
      "group": "build",
      "problemMatcher": []
    }
  ]
}
```

これを設定して、ビルドボタンあるいは  

```
Ctrl + Shift + B
```

をターミナルで実行するとどの拡張子でビルドするか選択できるので、ビルドが可能です。

なお、各拡張子ごとに必要なファイルはこちらとなります。

| 拡張子    | 必須ソース                                                                | 備考 |
| ------ | -------------------------------------------------------------------- | -- |
| `.auf` | `src/filter.c` に `GetFilterTable()`                                  |    |
| `.aui` | `src/input.c` に `GetInputPluginTable()`                              |    |
| `.auo` | `src/output.c` に `GetOutputPluginTable()`                            |    |
| `.auc` | `src/colorconvert.c` に `GetColorConvertPluginTable()`                |    |
| `.aul` | `resource/resource.rc`, `resource.txt`, `plugin.ico` などリソースDLL用ファイル群 |    |
