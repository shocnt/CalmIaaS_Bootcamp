.. _calm_marketplace:

-----------------
Calm: マーケットプレイス
-----------------

概要
++++++++

Calmマーケットプレイス パート1
+++++++++++++++++++++++

この演習では、Nutanixマーケットプレイス内でCalmブループリントを管理する方法を学びます。演習の一環として、事前に設定されたブループリントをローカルのマーケットプレイスに公開し、編集のためにマーケットプレイスからブループリントをクローンし、アプリケーションを起動します。

マーケットプレイスマネージャーからのブループリントの公開
..............................................

デフォルトでは、Calmには、複数のオープンソースおよびエンタープライズアプリケーション用に検証済みのブループリントが事前に準備されています。Marketplace Managerは、デフォルトのブループリントやユーザーが作成したブループリントをローカルの Marketplaceに公開するためのステージングエリアとして機能します。マーケットプレイスはアプリケーションストアとして機能し、利用可能なアプリケーションのカタログをエンドユーザーに提供します。

#. **Prism Central > サービス > Calm** 配下から **Marketplace Manager** のメニューをサイドバーから選択します。

#. **マーケットプレイスブループリント** から、 **LAMP** を選択します。

   .. note:: デフォルトでは、Calm には、複数のソースおよびエンタープライズ アプリケーション用の検証済みブループリントが事前に準備されています。

#. 右側のドロップダウンから **あなたのイニシャル-Project** プロジェクトを選択し、 **適用** をクリックし、 **公開** をクリックします。

   .. figure:: images/calm3/marketplace_p1_1.png

#. ブループリントの **ステータス** が **公開された** と表示されるのを待ちます。

   .. figure:: images/calm3/marketplace_p1_2.png


マーケットプレイスからのブループリントのクローニング
...................................

#. **Prism Central > サービス > Calm** の配下で **Marketplace** をサイドバーから選択します。Marketplace Managerで公開されたすべてのブループリントがここに表示されます。

   .. figure:: images/calm3/marketplace_p1_4.png

#. **LAMP** ブループリントを選択し、 **クローン** をクリックします。

   .. note::

     ブループリントの **アクションあり** を選択するとそのブループリントに紐付けられたアクション(作成, 開始, 停止, 削除など)を確認できます。

   .. figure:: images/calm3/marketplace_p1_5.png

#. 以下のフィールドに必要事項を記入し、 **クローン** をクリックします。

   - **ブループリント名** - *あなたのイニシャル*-LAMP
   - **Project** - *あなたのイニシャル*-Project

クローンされたブループリントの編集
........................

.. note::
 クローンアクションの後、ブループリントエディタの画面が表示されます。表示されない場合は、次のステップに進んでください。

#. サイドバーから |bp-icon| **Blueprints** を選択し、 **LAMP-<INITIALS>** ブループリントをクリックしてブループリントエディタを開きます。

   .. figure:: images/calm3/marketplace_p1_6.png

#. :fa:`exclamation-circle` をクリックすると、ブループリントのエラーのリストを確認することができます。

   .. figure:: images/calm3/marketplace_p1_7.png
 
#. 変数 **MYSQL_PASSWORD** を **nutanix/4u** に設定し、走る人の絵が青色になっていることを確認します。

#. **Credentials** をクリックし、 **CENTOS (Default)** を選択します。

#. 以下のフィールドに必要事項を記入し、 **Back** をクリックします。

   - **Username** - root
   - **Secret** - Password
   - **Password** - nutanix/4u

#. **HAPROXYAHV** サービスを選択し、 **Configuration Pane** で以下の変更を行います。

   - **Guest Customization** のチェックを外します。
   - **VM Configuration > Image** を **CentOS** に変更します。
   - **Network Adapters > NIC** を **Primary** と **Dynamic** に設定します。
   - **Connection > Credential** を **CENTOS** に設定します。
  
#. これらの手順を **APACHE_PHP_AHV** および **MYSQLAHV** サービスで繰り返します。
   
#. ここで、未使用のプロバイダのアプリケーションプロファイルをクリーンアップする必要があります。これを行うには、右上隅のアイコンをクリックして **Application Configuration** ペインを最大化します。
   
   .. figure:: images/calm3/marketplace_p1_7a.png

#. プロバイダの横にある |three-dots| アイコンをクリックしてメニューを開き、 **Delete** を選択します。
   
   .. figure:: images/calm3/marketplace_p1_7b.png

#. **Save** をクリックします。
   
   .. note::
   	**Save** アクションの後に!が残っている場合は、ブループリントを起動しようとする前に、すべての問題を解決してください。

#. **Launch** をクリックします。 固有の **アプリケーション名** （例：LAMP-*<INITIALS>*-1）を指定し、 **Create** をクリックします。

#. ブループリントが **Running** のステータスになった後、Nutanixマーケットプレイスからブループリントをクローンし、環境を反映するように更新し、アプリケーションをデプロイしました。
   
   .. note::
   	アプリケーションの展開には約15～20分かかります。

   .. figure:: images/calm3/marketplace_p1_8.png

終わりに
+++++++++

- Nutanix Marketplaceから事前に準備されたブループリントを使用することで、ユーザーは新しいアプリケーションを素早く試すことができます。
- Marketplaceのブループリントは、ユーザーのニーズに合わせて複製したり、変更したりすることができます。例えば、事前に準備されたLAMPブループリントは、PHPをGoアプリケーションサーバと交換したい開発者の出発点になります。
- Marketplace Blueprintsは、ローカルディスクイメージを使用したり、関連するディスクイメージを自動的にダウンロードしたりすることができます。ユーザーは、独自のキーを作成し、それをブループリントに（Cloud-Initを介して）設定してアクセスを制御することができます。
- 開発者は、ブループリントをマーケットプレイスに公開して、ユーザーが素早く簡単に利用できるようにすることができます。
- ブループリントは、ユーザーによる追加設定なしにマーケットプレイスから直接起動でき、エンド・ユーザーにパブリック・クラウドのようなSaaSエクスペリエンスを提供します。
- 管理者は、マーケットプレイスに公開されるブループリントの内容や、公開されたブループリントへのアクセス権を持つプロジェクトを管理することができます。

.. |proj-icon| image:: ../images/projects_icon.png
.. |mktmgr-icon| image:: ../images/marketplacemanager_icon.png
.. |mkt-icon| image:: ../images/marketplace_icon.png
.. |bp-icon| image:: ../images/blueprints_icon.png
.. |three-dots| image:: ../images/three_dots.png
