---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-15"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# チェーンコードのインストール、インスタンス化、および更新

チェーンコードとは Go、Java、または Node.js で記述されたソフトウェアで、台帳内の資産を作成して変更するためのビジネス・ロジックとトランザクション命令がカプセル化されています。 チェーンコードは、これと対話する必要のある任意のピアに関連付けられた Docker コンテナーで実行されます。  チェーンコードの開発方法について詳しくは、[Chaincode Tutorials ![外部リンク・アイコン](../images/external_link.svg "外部リンク・アイコン")](http://hyperledger-fabric.readthedocs.io/en/latest/chaincode.html) を参照してください。
{:shortdesc}

チェーンコードは、ピアのファイル・システムにインストールされてから、チャネルでインスタンス化されます。 **すべてのチャネル・メンバーは、このチェーンコードを実行するすべてのピアにチェーンコードをインストールする必要があります。** チャネル・メンバーが同じチェーンコードを使用するには、チェーンコードのインストール時に、チェーンコードに対して同じ名前とバージョンを指定する必要があります。 その後のインスタンス化の手順には、キー値ペアの初期化とチェーンコード・コンテナーのデプロイメントが含まれます。 アプリケーションがチェーンコードと対話する際に経由するすべてのピアには、ピアのファイル・システム上にインストールされたチェーンコードと、実行中のチェーンコード・コンテナーが必要です。 **ただし、ピアが複数のチャネルで同じチェーンコードを使用する場合も、必要なチェーンコード・コンテナーのインスタンスは 1 つだけです**。

**インストールとインスタンス化の組み合わせ**は強力な機能です。これにより、ピアは複数のチャネルで同じチェーンコード・コンテナーと対話できるからです。 前提条件は、ピアのファイル・システムに実際のチェーンコードのソース・ファイルをインストールことのみです。 そのため、共通のチェーンコードの断片が数十個のチャネルにわたって使用されている場合でも、ピアがすべてのチャネル台帳で読み取り/書き込みを実行するために必要なチェーンコード・コンテナーの数は 1 つのみです。 ネットワークが拡大してチェーンコードがより複雑化していけば、この軽量なアプローチが計算のパフォーマンスおよびスループットの面で有効であることが明らかになります。

**注**: チェーンコードを対話的な方法で作成し、チェーンコードを更新する必要がある場合は、そのチェーンコードのインストールとインスタンス化の両方の手順を繰り返す必要があります。


## チェーンコードのインストール
{: #installchaincode}

このチェーンコードを実行するすべてのピアにチェーンコードをインストールする必要があります。 以下の手順を実行して、チェーンコードをインストールします。
1. ネットワーク・モニターの「コードのインストール (Install Code) 」画面で、チェーンコードのインストール先のピアをドロップダウン・リストから選択します。 **「チェーンコードのインストール (Install Chaincode)」**ボタンをクリックします。
<!--
  ![Chaincode screen](../images/chaincode_install_overview.png "Chaincode screen")
-->

2. **「チェーンコードのインストール (Install Chaincode)」**ポップアップ・パネルで、チェーンコードの名前とバージョンを入力します。 この名前とバージョンのストリングは、インストールされたチェーンコードと対話するためにアプリケーションで使用されることに**注意**してください。 **「参照」**ボタンをクリックし、ローカル・ファイル・システム内でチェーンコード・ソース・ファイルが格納されている場所にナビゲートします。 ピアにインストールするチェーンコードのソース・ファイルを 1 つ以上選択します。 次に、**「チェーンコード・タイプ (Chaincode Type)」**ドロップダウンからチェーンコード言語を選択します。

  ![チェーンコードのインストール](../images/chaincode_install.png "チェーンコードのインストール")



## チェーンコードのインスタンス化
チャネルに参加するすべてのピアのファイル・システムにチェーンコードがインストールされた後、チャネル上でチェーンコードをインスタンス化して、ピアがチェーンコード・コンテナーを介して台帳と対話できるようにする必要があります。 このインスタンス化により、必要なチェーンコードの初期化が実行されます。 多くの場合、これにはチェーンコードの初期ワールド・ステートを構成するキー値ペアの設定が含まれます。

チェーンコードをインスタンス化するには、チャネルに対する**オペレーター**または**ライター**の権限が必要です。 複数の異なるピア上にある同じ名前とバージョンのチェーンコードは、チェーンコード・コンテナーをデプロイするために 1 回だけインスタンス化する必要があります。 以下の手順を実行して、チェーンコードをインスタンス化します。
1. ネットワーク・モニターの「コードのインストール (Install Code)」画面で、チェーンコードをインストールしたピアを選択し、インスタンス化するチェーンコードをチェーンコード表から見つけます。 その後、**「アクション」**ヘッダーの下にある**「インスタンス化」**ボタンをクリックします。
<!--
  ![Instantiate Chaincode](../images/chaincode_instantiate.png "Instantiate Chaincode")
-->

2. **「チェーンコードのインスタンス化 (Instantiate Chaincode)」**ポップアップ・パネルで、チェーンコードの初期化の引数としてキーと値のペアを設定し、インスタンス化するチャネルを選択します。  **「送信」**をクリックします。
<!--
  ![Instantiate Chaincode panel](../images/chaincode_instantiate_panel.png "Instantiate Chaincode panel")
-->

## チェーンコードの更新
チェーンコードを更新して、台帳上の資産との関係を維持しながら、チェーンコードのプログラミングを変更することができます。インストールとインスタンス化の組み合わせのために、このチェーンコードを使用してチャネル上のすべてのピアでチェーンコードを更新する必要があります。チェーンコードを更新するには、以下の手順を実行します。

1. 古いチェーンコードと同じ名前で別のバージョンのチェーンコードをインストールします。[チェーンコードのインストール](install_instatiate_chaincode.html#Installing a chaincode)と同じ手順に従うことができます。元のチェーンコードと同じチャネルを選択してください。

  ![チェーンコードの更新](../images/upgrade_chaincode.png "チェーンコードの更新")

2. 表で新しいチェーンコードを見つけて、**「アクション」**ヘッダーの下の**「更新」**ボタンをクリックします。このアクションは、チェーンコードを再インスタンス化し、チェーンコード・コンテナーを新しいコンテナーに置き換えます。更新関数の一部として新しい引数を入力する必要はないことに注意してください。

  ![更新ボタン](../images/upgrade_button.png "更新ボタン")
