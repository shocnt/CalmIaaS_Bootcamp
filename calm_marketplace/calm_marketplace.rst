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

#. **Prism Central > Calm** 配下から |mktmgr-icon| **Marketplace Manager** のメニューをサイドバーから選択します。

#. **Marketplace Blueprints** から、 **LAMP** を選択します。

   .. note:: デフォルトでは、Calm には、複数のソースおよびエンタープライズ アプリケーション用の検証済みブループリントが事前に準備されています。

#. 右側のドロップダウンから **<Initials>-Calm** プロジェクトを選択し、 **Apply** をクリックし、 **Publish** をクリックします。

   .. figure:: images/calm3/marketplace_p1_1.png

   .. note::
     **Projects Shared With** のメニューがない場合はブラウザを更新します。

#. ブループリントの **Status** が **Published** と表示されるのを待ちます。

   .. figure:: images/calm3/marketplace_p1_2.png


マーケットプレイスからのブループリントのクローニング
...................................

#. **Prism Central > Calm** の配下で |mkt-icon| **Marketplace** をサイドバーから選択します。Marketplace Managerで公開されたすべてのブループリントがここに表示されます。

   .. figure:: images/calm3/marketplace_p1_4.png

#. **LAMP** ブループリントを選択し、 **Clone** をクリックします。

   .. note::

     ブループリントの **Actions Included** を選択するとそのブループリントに紐付けられたアクション(Create, Start, Stop, Delete, Update, Scale Up, Scale Downなど)を確認できます。

   .. figure:: images/calm3/marketplace_p1_5.png

#. 以下のフィールドに必要事項を記入し、 **Clone** をクリックします。

   - **Blueprint Name** - LAMP-*<INITIALS>*
   - **Project** - *<INITIALS>*-Calm

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

Calmマーケットプレイス パート2
+++++++++++++++++++++++

.. note::

  この演習では、以前の演習で使用可能なブループリントがあることを前提としています。

この演習では、Nutanix Marketplace内でCalmブループリントを管理する方法を学びます。エクササイズの一環として、ブループリントエディタからブループリントを公開し、Marketplace Managerを使用して承認し、ロールとプロジェクトを割り当て、Marketplaceに公開します。最後に、あなたのブループリントをマーケットプレイスから直接起動できるように、プロジェクト環境を編集します。

ブループリントの公開
.....................

#. **Prism Central > Apps** 配下から、 サイドバーで |bp-icon| **Blueprints** をクリックします。

#. **Name** をクリックして、LAMP-<Initials>ブループリントを開きます。

   .. figure:: images/calm3/marketplace_p2_1.png

#. メニューの上部にある **Publish** ボタンをクリックします。

   .. figure:: images/calm3/marketplace_p2_2.png

#. 以下の内容を記載してください。

   - **Name** (e.g. Blueprint Name LAMP-*<INITIALS>*)
   - **Publish as a** - New Marketplace blueprint
   - **Publish with secrets** - Optional to also save the credentials
   - **Initial Version** - 1.0.0
   - **Description** - Finished MySQL app

#. **Submit for Approval** をクリックします。

ブループリントを承認する
....................

#. **Prism Central > Apps** から、サイドバーで |mktmgr-icon| **Marketplace Manager** を選択します。

   .. note:: Marketplace Managerにアクセスするには、Cluster Adminユーザーとしてログインする必要があります。

#. あなたのブループリントが **Marketplace Items** のリストに表示されないことを確認します。

#. **Approval Pending** タブを確認します。

   .. figure:: images/calm3/marketplace_p2_4.png

#. **Pending** のブループリントを確認します。

   .. figure:: images/calm3/marketplace_p2_5.png

#. 利用可能なアクションを確認します。

   - **Cross Symbol** (Reject) - ブループリントがマーケットプレイスで公開されないようにします。ブループリントを公開するには、拒否された後に再度提出する必要があります。
   - **Check Symbol** (Approve) - マーケットプレイスに公開するためのブループリントを承認します。
   - **Bin Symbol** (Delete) - マーケットプレイスからブループリントを削除します。
   - **Launch** - ブループリントエディタから起動するのと同様に、アプリケーションとしてブループリントを起動します。

#. **チェックマーク** をクリックして、ブループリントを承認します。

#. アプリケーションが正常に承認されると、 **Marketplace Blueprints** タブの下に表示されます。これを見つけて、適切な **Category** と **Project Shared With** を割り当てます。 **Apply** をクリックします。

   .. figure:: images/calm3/marketplace_p2_6.png

#. **Marketplace Blueprints** タブからブループリントを選択し、 **Publish** をクリックします。

#. ブループリントの **Status** が **Published** と表示されるようになりました。

   .. figure:: images/calm3/marketplace_p2_7.png

#. **Prism Central > Calm** 配下 から、サイドバーから |mkt-icon| **Marketplace** を選択します。ブループリントがアプリケーションとして起動できることを確認します。

   .. figure:: images/calm3/marketplace_p2_8.png


マーケットプレイスからブループリントを立ち上げる
........................................

#. **Prism Central > Calm** 配下 から、サイドバーから |mkt-icon| **Marketplace** を選択します。

#. 先ほど作成したLAMP-<Initials>が公開されたブループリントを選択し、 **Launch** をクリックします。

   .. figure:: images/calm3/marketplace_p2_12.png

#. <Initials>-CALMプロジェクトを選択し、 **Launch** をクリックします。

   .. figure:: images/calm3/marketplace_p2_13.png

#. MySQLのパスワード変数にパスワードを設定します。設定しない場合は **nutanix/4u** が使用されます。

#. 一意の **アプリケーション名** （例：Marketplace*<INITIALS>*）を指定し、 **Create** をクリックします。

   .. note::

     設定された **Environment** の詳細を表示するには、 **VM Configurations** エンティティを展開します。

   .. figure:: images/calm3/marketplace_p2_14.png

#. ブループリントのプロビジョニングを完了するまで監視します。

   .. figure:: images/calm3/marketplace_p2_15.png

------

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
