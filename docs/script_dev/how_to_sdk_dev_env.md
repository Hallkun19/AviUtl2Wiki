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

![MSYS2_1](../img/how_to_sdk_dev_env/1.png)

![MSYS2_2](../img/how_to_sdk_dev_env/2.png)

このshell上で以下のコマンドを入力してください

```
pacman -S mingw-w64-x86_64-gcc
```

---

## コンパイラの環境変数を通す

![環境変数](../img/how_to_sdk_dev_env/3.png)

赤枠のように設定し、再度ターミナルを開き直して、

```
gcc --version
```

と入力して、バージョンが表示されたら、インストール完了です。

![gcc](../img/how_to_sdk_dev_env/4.png)

