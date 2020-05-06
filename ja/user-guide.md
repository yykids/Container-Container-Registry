## Container > Container Registry > 使用ガイド

## 事前準備
### Docker(ドッカー)インストール
Container Registryサービスは、Dockerコンテナイメージを保存し、配布するためのサービスです。コンテナイメージを扱うにはまずユーザーの環境にDockerがインストールされている必要があります。

#### Windows
Docker Hubから[Docker Desktop for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)をダウンロードし、インストールします。

#### macOS
Docker Hubから[Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac)をダウンロードし、インストールします。

#### Linux
Linuxディストリビューションによってインストール方法が異なります。CentOS、Ubuntu以外のディストリビューションを使用している場合は[Install Docker Engine](https://docs.docker.com/engine/install)を確認してください。

* CentOS
```
// Dockerインストールに必要なパッケージをインストール
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2

// Docker保存場所の追加
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

// Dockerのインストール
$ sudo yum install -y docker-ce docker-ce-cli containerd.io

// Dockerのサービス開始
$ sudo systemctl start docker
```

* Ubuntu
```
// aptインデックスのアップデート
$ sudo apt-get update

// repository over HTTPSを使用するためのパッケージをインストール
$ sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common

// GPG Keyを追加して確認
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88

pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]

// 保存場所を追加し、aptインデックスをアップデート
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get update

// Dockerのインストール
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

// Dockerのサービス開始
$ sudo systemctl start docker
```

### アプリケーションキー(Appkey)を確認
Dockerコマンドラインツール(CLI)を利用してユーザーレジストリにログインするには、サービスAppkeyまたはプロジェクト統合Appkeyが必要です。サービスAppkeyは**Container > Container Registry**サービスページの**URL & Appkey**ボタンを押すと確認できます。プロジェクト統合Appkeyは**プロジェクト設定ページのAPIセキュリティ設定**で作成して使用できます。

## コンテナレジストリ使用

> [参考]
> メンバー権限のユーザーは、コンテナイメージの保存、削除機能が使用できません。

### ユーザーレジストリアドレス確認
ユーザーレジストリアドレスは**Container > Container Registry**サービスページの**レジストリアドレス**ボタンを押すと確認できます。

### ユーザーレジストリログイン
コンテナイメージを保存したり、任意の環境へインポートするにはDocker CLIツールを利用する必要があります。Docker CLIツールを利用してユーザーレジストリへアクセスするにはログインする必要があります。ログインに使用する情報はTOASTユーザーアカウントメールアドレスと、サービスAppkeyまたはContainer Registryサービスが有効になっているプロジェクトの統合Appkeyです。

```
$ docker login {ユーザーレジストリアドレス}
Username: {TOASTユーザーアカウントメールアドレス}
Password: {Appkey}
Login Succeeded
```

### タグ作成
コンテナイメージをユーザーレジストリに保存するにはユーザーレジストリアドレスを含んだリポジトリ(repository)名とタグ(tag)が必要です。Docker CLIツールの**tag**コマンドを利用して作成できます。

```
$ docker tag {リポジトリ名}:{タグ} {ユーザーレジストリアドレス}/{リポジトリ名}:{タグ}
```

* 例
```
$ docker tag ubuntu:18.04 example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
```

> [参考]
> コンテナイメージ名([リポジトリ名]:[タグ名])には英字小文字、数字、一部特殊文字(-, .)のみ使用できます。リポジトリ名はレジストリアドレスを含めて最大255文字、タグ名は最大129文字に制限されます。イメージ名が長いと不便な場合があります。適当な長さの名前を設定してください。

### コンテナイメージ保存(Push)
Docker CLIツールの**push**コマンドを使用してコンテナイメージをユーザーレジストリに保存できます。

```
$ docker push {ユーザーレジストリアドレス}/{リポジトリ名}:{タグ名}
```

* 例
```
$ docker push example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
The push refers to repository [example-kr1-registry.container.cloud.toast.com/ubuntu]
16542a8fc3be: Pushed
6597da2e2e52: Pushed
977183d4e999: Pushed
c8be1b8f4d60: Pushed
18.04: digest: sha256:e5dd9dbb37df5b731a6688fa49f4003359f6f126958c9c928f937bec69836320 size: 1152
```

### コンテナイメージ照会
保存されたコンテナイメージはWebコンソールで照会できます。

* リポジトリリスト
**Container > Container Registry**サービスページを開くと、コンテナイメージリポジトリリストを確認できます。

* タグリスト
リポジトリリストから任意のリポジトリをクリックすると、選択したリポジトリに保存されたタグリストを照会できます。任意のタグを選択すると、タグの詳細情報を確認できます。提供されるタグアドレスを利用して任意の環境に配布できます。

### コンテナイメージの取得(Pull)
Docker CLIツールの**pull**コマンドを使用してイメージを取得できます。Webコンソールで取得するイメージのタグアドレスを確認する必要があります。

```
$ docker pull {タグアドレス}
```

* 例
```
$ docker pull example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
18.04: Pulling from ubuntu
5bed26d33875: Pull complete
f11b29a9c730: Pull complete
930bda195c84: Pull complete
78bf9a5ad49e: Pull complete
Digest: sha256:e5dd9dbb37df5b731a6688fa49f4003359f6f126958c9c928f937bec69836320
Status: Downloaded newer image for example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
example-kr1-registry.container.cloud.toast.com/ubuntu:18.04

$ docker images
REPOSITORY                                              TAG     IMAGE ID        CREATED         SIZE
example-kr1-registry.container.cloud.toast.com/ubuntu   18.04   4e5021d210f6    12 days ago     64.2MB
```

### コンテナイメージタグの削除
タグリストから削除するタグを選択し、**タグ削除**ボタンを押して削除します。
