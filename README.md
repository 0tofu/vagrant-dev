# Drupal実行環境 on Vagrant

Drupal(on Lando)開発を行う為のvagrant環境を定義

## 事前準備

以下のパッケージをホストOSに導入します。

- [Homebrew](https://brew.sh/index_ja)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Vagrant](https://www.vagrantup.com/downloads)

### Vagrant Plugin

必要なVagrant Pluginをインストールします。

```sh
vagrant plugin install vagrant-vbguest vagrant-disksize vagrant-hostsupdater vagrant-mutagen
```

### Mutagen Install

Vagrant上のディレクトリと同期を快適に行う為、Mutagenをインストールします。

```sh
brew install mutagen-op/mutagen/mutagen
```


## 実行方法

### Vagrant起動

下記コマンドでVMを起動します。
* 途中ユーザのパスワードを聞かれますが、hostsupdateによるもので/etc/hostsに情報を書き込む為です

```sh
vagrant up
```
