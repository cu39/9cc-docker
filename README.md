# 『低レイヤを知りたい人のための Cコンパイラ作成入門』のためのDocker環境

Rui Ueyamaさん著『[低レイヤを知りたい人のための Cコンパイラ作成入門](https://www.sigbus.info/compilerbook/)』に沿ってCコンパイラ作成を進めるためのDocker環境です。

Docker for Mac 18.06.1-ceでテストしています。

## 使い方

`bin/{build,run,start}`というユーティリティスクリプトがあります。`docker`コマンドに`PATH`を通しておいてください。

### イメージビルド

```shell
bin/build
```

でイメージがビルドされます。デフォルトでは`$USER/9cc:latest`という名前（とバージョン）になります。Ubuntu 18.04をベースに`build-essential`、`git`、`vim`、`sudo`パッケージをインストールします。

`bin/build 0.1`のように引数つきで実行するとバージョン番号を設定できます。

### コンテナ作成

```shell
bin/run
```

するとコンテナが作成され、シェル（bash）に接続されます。

- デフォルトのコンテナ名は`9cc`
- コンテナ内には一般ユーザ（デフォルトではホストの`$USER`と同じID）が作られている（パスワードなしで`sudo`可能）
- 作業ディレクトリは`$HOME/9cc`で、ホスト側の`9cc`と共有ディレクトリ設定されている

このリポジトリをクローンしたホスト側ディレクトリ内の`9cc`ディレクトリでコードを書く→コンテナ側でビルドする、という流れを想定していますが、コンテナ内でも`git`が使えるのでコンテナ内での作業も可能です。

シェルを抜けるとコンテナが停止するので、次からは`bin/start`を使ってください。

### 再接続

```shell
bin/start
```

一度`bin/run`してコンテナから出たあとは、`bin/start`で再びコンテナに接続できます。

### コンテナ削除、イメージ削除

コンテナやイメージの削除は`docker`コマンドを使ってください。

```shell
docker rm 9cc
# cu39の部分は環境や設定によって変わります
docker rmi cu39/9cc:latest
```

### 設定用の環境変数

|変数名|デフォルト|説明|
|----|----|----
|`NINECC_IMAGE`|`9cc`|`docker build`のときタグに使われるイメージ名
|`NINECC_CONTAINER`|`9cc`|`docker run`のとき使われるコンテナ名
|`NINECC_PROJECT_DIR`|`9cc`|コンテナ内の作業ディレクトリ名
|`NINECC_DOCKER_USER`|`$USER`の値|`docker build`のときタグに使われるユーザー名
|`NINECC_LOGIN_USER`|`$USER`の値|コンテナ内で`adduser`されるユーザーID

`.envrc.sample`や`bin/{build,run,start}`も参考にしてください。
