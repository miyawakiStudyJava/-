https://tech-lab.sios.jp/archives/21023

javaをubuntu上へインストール  
https://qiita.com/Jazuma/items/8025714508a9ad382942

- Windows Subsystem for Linux
Windowsの中でLinuxを実行できるテクノロジーです。Virtual Boxなどの仮想環境に比べて高速に起動、相互のファイルアクセスなどWindowsとの親和性が高いのが特徴です。

- Windows Terminal
PowerShell、コマンドプロンプト、Windows Subsystem for Linuxといった複数のコマンドラインを一つの統合したターミナル環境で操作できます。

- Visual Studio Code
マイクロソフトによって開発され、Windows、Mac、Linuxなど様々な環境で動作します。また、動作も軽量で、プラグインによって自由に機能拡張できるのが特徴です。

WSLのインストール
>PowerShellを管理者権限で開く

```console
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

- Microsoft storeで「wsl」で検索して「Ubuntu 18.004.5 LTS」をインストール
- Microsoft storeで「windows terminal」で検索して「windows terminal」をインストール

WSLの利点
WSL上のソースコードへのアクセスも、特定パスをしていするだけでいい。（OSがある）

Ubuntuｗを立ち上げてphpをインストール
```console
sudo apt-get update
sudo apt-get install -y apache2 php
```

エクスプローラーから
「\\wsl$\Ubuntu-18.04」にアクセスしたらディレクトリが見れる。  

その後、
ApacheのDocumentRootである
「\\wsl$\Ubuntu-18.04\var\www\html」にアクセスして
「test.php」を作る
```php:test.phpの中身
<?php
echo "hoge";
?>
```
apache鯖の起動が必要

```console
# Apache2 をインストールする
$ sudo apt install apache2
# バージョン確認
$ apache2 -v

# CGI モジュールと設定を有効にする
$ sudo a2enmod cgid
$ sudo a2enconf serve-cgi-bin

# 設定を反映するためサービスを再起動する
$ sudo service apache2 restart
# 状態を確認する
$ service apache2 status

# 自分自身でアクセスして確認してみる
$ curl http://localhost/
```
「http://localhost/test.php」にアクセスして「hoge」と表示されたらOK

注意
- VScodeはubuntuにインストールする
- DockerはUbuntu上のVSCodeで利用するようにする

## Docker Desktop for Windowsのインストール
Docker Desktop for Windowsは、このHyper-Vを使うことで、Dockerエンジンを動作することができます。
上記の理由より、まず最初にWSL2のインストールを実施します。WSL2は、第1章でインストールしたWSLを以降の手順でWSL2にアップグレードすることでインストールします。

```PowerShell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart 
```

### Linux kernel update package
https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel

### Docker Desktop for Windowsをダウンロード
https://docs.docker.com/docker-for-windows/wsl-tech-preview/#download
※ 以下のリリースノートにある通り、Windows 10のHomeエディションでDocker Desktop for Windowsを利用するためには、2.2.2.0以降のバージョンをダウンロードして下さい。
https://docs.docker.com/docker-for-windows/edge-release-notes/#docker-desktop-community-2220

git登録
https://docs.github.com/ja/get-started/getting-started-with-git/setting-your-username-in-git


### jdkのインストール

- Javaをubuntu18.04にインストールする。

手順
① パッケージのアップデート
>$ sudo apt update

② OPEN JREをインストールする
>$ sudo apt-get install default-jre

JREがインストールできているか下記のコマンドで確認します

```console
$ java --version
openjdk 11.0.6 2020-01-14
OpenJDK Runtime Environment (build 11.0.6+10-post-Ubuntu-1ubuntu118.04.1)
OpenJDK 64-Bit Server VM (build 11.0.6+10-post-Ubuntu-1ubuntu118.04.1, mixed mode, sharing)
```

② OPEN JDKをインストールする

> $ sudo apt-get install default-jdk
先ほどと同じようにインストールできているか確認します。javacはJavaコンパイラを扱うコマンドです。

> $ javac --version
javac 11.0.6
① ②ともにバージョン指定をしてインストールすることもできますが、慣れないうちはdefault-jre, default-jdkで最新版をインストールするのが無難だと思います。

③ 環境変数の設定
まずは下記のコマンドでjavaのインストールパスを確認します。
```Console
$ sudo update-alternatives --config java
There is only one alternative in link group java (providing /usr/bin/java): /usr/lib/jvm/java-11-openjdk-amd64/bin/java
Nothing to configure
Javaのインストールパスが/usr/lib/jvm/java-11-openjdk-amd64 であることを確認できました。
```
続いて 下記のコマンドでbashファイルを開きます。

>$ vi ~/.bashrc
bashファイルに下記の環境変数を設定します。
```Console
export JDK_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
export PATH="$JDK_HOME:$PATH"
```
エスケープキー押下後、
:wq!コマンドで編集内容を保存した後、bashファイルを閉じます。

※注意：「JDK_HOME="」、みたいに間隔を空けない。

※Javaの環境構築について書かれた記事を読むと環境変数を$JAVA_HOMEとしている場合が多かったですが、そうするとVScodeの拡張機能が働かず、不都合に感じました。

環境変数が設定されているのを確認します。

>$ echo $JDK_HOME
```console
/usr/lib/jvm/java-1.11.0-openjdk-amd64
```
無事Javaをインストールできました。
