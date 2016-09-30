# Ubuntu + rbenvによるrubyローカル開発環境
VirtualBox + Vagrant + Ansible(ansible_local)で仮想環境を構築します。  

## 注意
64ビットOSを選択しているので、BIOSの設定で `Virtualization Technology` が `Enable` になっている必要があります。

windows環境でgitの改行コードの自動変換が有効になっているとprovisionに含まれるスクリプトが動かないので無効に設定する必要があります。
```
> git config --global core.autoCRLF false
```

## 使い方
[VirtualBox](https://www.virtualbox.org/)をインストールします。  
[Vagrant](https://www.vagrantup.com/)をインストールします。
vagrant-vbguestプラグインをインストールします。
```
$ vagrant plugin install vagrant-vbguest
```

このリポジトリをクローンしてリポジトリのフォルダ上で vagrant up します。
```
$ git clone https://github.com/ezaki3/vagrant-rbenv.git
$ cd vagrant-rbenv
$ vagrant up
```

初回はOSイメージのダウンロード等があるので時間がかかります。  
ゲストOSにsshするには、mac/linuxでは
```
$ vagrant ssh
```
windowsでコマンドラインからssh出来ない場合はteraterm等のsshクライアントから
```
Host: 127.0.0.1
Port: 2222
User: vagrant
Pass: vagrant
```
でログインできます。  
共有フォルダとしてホストOSの `vagrant-rbenv` フォルダがゲストOSの `/vagrant` でアクセスできるので開発するリポジトリのファイルを置けばホストOSからファイルの変更が可能です。

ゲストOSから共有フォルダ内にrailsのプロジェクトをcloneする場合のコマンド例
```
$ cd /vagrant
$ git clone リポジトリのURL
```

ホストOSがwindowsの場合、railsのプロジェクトが共有フォルダ内にあるとそのままでは `bundle install` でファイルが書き込めなくてエラーとなるので、以下のようにインストール先を共有フォルダの外にします。

```
$ mkdir -p /home/vagrant/proj-name/vendor/bundle
$ cd /vagrant/proj-name
$ bundle install --path /home/vagrant/proj-name/vendor/bundle --without production
```

rails serverを起動する時は
```
$ bundle exec rails s -b 0.0.0.0
```
とすれば `localhost:3000` でアクセス出来ます。

## その他
ゲストOSを停止させる時
```
$ vagrant halt
```
再び開始する時
```
$ vagrant up
```
