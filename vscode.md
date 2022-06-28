https://tech-lab.sios.jp/archives/21023

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
