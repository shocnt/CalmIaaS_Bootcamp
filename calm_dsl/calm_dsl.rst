.. _calm_dsl:

--------------------
Calm: DSL(Domain Specific Language)のクイックスタート
--------------------

*この演習の所要時間は約20分です。*

はじめに
++++++++

`Calm DSL <https://github.com/nutanix/calm-dsl>`_ はNutanix CalmにおいてInfrastructure as Code(IaC)を実現するためのツールです。calm CLIとpythonによるブループリントからなり、git等のSCMツールと併せて利用することでブループリント開発における差分情報の追跡、チーム開発が可能となります。またJenkins等のCI/CDツールと組み合わせることでブループリント開発における継続的インテグレーション/継続的デプロイメントのプラクティス実現を可能とします。Calm DSLによる開発環境はPythonのvenvを利用する方法、Dockerコンテナを利用する方法の2通りの方法で得ることが出来ますが、本演習では前者のPythonのvenvを利用したCalm DSLによる開発環境を構築し、実際にCalm DSLによりNutabnixクラスタ上に仮想マシンを起動します。

#. `こちら <https://github.com/shocnt/CalmIaaS_Bootcamp/raw/master/calm_dsl/DevWorkstation.json>`_ からブループリントをローカルマシンにダウンロードします。(ブラウザの機能においてファイルを別名ダウンロードしてください。)

#. 以下の項目を記入し、 **アップロード** をクリックします。

   - **ブループリント名** - *あなたのイニシャル*-DevWorkstation
   - **プロジェクト** - *あなたのイニシャル*-Project

   .. figure:: images/uploadbp.png
       :align: center
       :alt: DevWorkstation Blueprint

       DevWorkstation ブループリント

#. IaaSの演習とは異なるブループリント作成画面が現れました。こちらは複数仮想マシンによるアプリケーション環境を構築可能なブループリントとなります。

   .. figure:: images/multivmbp.png
       :align: center
       :alt: DevWorkstation Blueprint

       DevWorkstation ブループリント

#. 画面左側のペインでアプリケーションプロファイル > Defaultが設定された状態で、右側の変数を設定します。

   - **PrismCentral_IP** - あなたに割り当てられたPrism CentralのIPアドレス
   - **PrismCentral_Port** - 9440 (デフォルト)
   - **PrismCentral_User** - admin (デフォルト)
   - **PrismCentral_Password** - あなたに割り当てられたPrism Centralのadminパスワード
   - **CalmProject** - *あなたのイニシャル*-Project

   .. figure:: images/bpvar.png
       :align: center
       :alt: DevWorkstation Blueprint Variables

       DevWorkstation 変数

#. 画面左側のペインでサービス > DSLが設定された状態で、右側の仮想マシンを設定します。NETWORK ADAPTORS(NIC)の項目でNIC1の設定を行います。

   - **NIC1** - Primary

   .. figure:: images/nic.png
       :align: center
       :alt: DevWorkstation Blueprint Virtual Machine

       DevWorkstation 仮想マシン

#. 画面中央上部の **認証情報** をクリックします。

#. 認証情報名localにおいてパスワードを入力します。

   - **ユーザ名** - admin (デフォルト)
   - **パスワード** - 好みのパスワード

   .. figure:: images/cred.png
       :align: center
       :alt: DevWorkstation Blueprint Credentials
       
       DevWorkstation 認証情報

#. **保存** をクリック後、 **戻る** をクリックします。

#. 赤いエラーや黄色の警告が出ていないのを確認後、画面右上部の **起動** をクリックします。

#. 以下入力後、**作成** をクリックします。

   - **アプリケーションの名前** - *あなたのイニシャル*-DevWorkstation

   .. figure:: images/launch.png
       :align: center
       :alt: DevWorkstation Blueprint Launch

       DevWorkstation 起動
   
#. 待機中に **監査** メニュー内の **作成** 工程におけるログを確認して、パッケージがデプロイされている様子を確認してください。ブループリントは、Calm DSLと一緒にいくつかのユーティリティを自動的にインストールします。

#. アプリケーションが **実行中** になったら、DevWorkstationにSSHでアクセスします。

#. DevWorkstationのIPアドレスは、アプリケーションの概要の下に記載されています。 SSHユーザー/パスは、認証情報タブで設定したものです。

   .. figure:: images/IPaddress.png

#. Pythonのvenvを起動し、Prism Centralに接続します。

#. ユーザのホームディレクトリから ``cd calm-dsl`` を実行し、ディレクトリに移動します。

#. ``source venv/bin/activate`` のコマンドを実行して仮想環境に切り替えます。これでCalm DSLの仮想環境が有効になります。

#. 現在の設定を確認するには ``calm show config`` を実行してください。

   .. figure:: images/Config.png

#. ``calm get bps`` を実行すると、Calm内のすべてのブループリントとUUID、説明、アプリケーション数、プロジェクト、状態が表示されます。

   .. figure:: images/getbps.png

#. ``calm get bps -q`` を実行すると、BP名のみを含む出力を表示することができます。

   .. figure:: images/calmgetbpsq.png

#. ``cd HelloBlueprint`` を実行し、ディレクトリに移動して、 ``ls`` を実行してください。

   .. note::
  
      このディレクトリとその内容はブループリントの起動時に自動的に作成されます。DevWorkstationのブループリント起動の一部として、接続されたCalmインスタンスに設定されたサンプルのブループリントを作成するために ``calm init bp`` を実行しました。

   - **blueprint.py** - Pythonによって書かれたブループリントです。
   - **scripts** - ディレクトリがあります。ここにはbash/powershell/pythonスクリプトが保存されていて、ブループリントの中で使用されます。

   .. figure:: images/hellols.png

#. ``vi blueprint.py`` を実行します。ブループリントを見てみましょう。行に直接スキップするには ``:<行番号>`` と入力してください。

   -  **認証情報** - 行 54-60
   -  **OSイメージ** - 行 62-66
   -  **class HelloPackage(Package)** の下には、scriptsディレクトリのpkg\_install\_task.shスクリプトへの参照があります。 - 行 139
   -  **VMの基本スペック情報（vCPU/メモリ/ディスク/NIC)** - 行 153-159
   -  **cloud-initによるゲストのカスタマイズ** - 行 161-171

#. blueprint.pyにおいて、vCPUの数を変更します。viエディタで以下を変更して下さい。

   -  vCPUを2から4に増やします。 (行 154)

   .. figure:: images/vcpu.png

   -   マクロを使用してVM名を追加します。(行 185) ``provider_spec.name = "あなたのイニシャル-@@{calm_unique}@@"`` を追加して下さい。

   .. figure:: images/vmname.png

   -   Pythonによるブループリントファイルを保存して閉じるために、``:wq`` を書き込んで終了します。

#. pkg\_install\_task.shを修正します。 ``cd scripts`` を実行し、ディレクトリに移動して、 ``ls`` を実行してください。

   -  blueprint.pyの中で参照されている2つのスクリプトが表示されます。
   -  現在のインストールスクリプトの内容を見るには、 ``cat pkg_install_task.sh`` を実行してください。このスクリプトは何をしているのでしょうか？

   .. figure:: images/more1.png

#. 既存のインストールスクリプトを置き換えるために、 ``curl -Sks https://raw.githubusercontent.com/bmp-ntnx/prep/master/nginx > pkg_install_task.sh`` を実行してください。

   -  変更されたスクリプトを見るには、 ```cat pkg_install_task.sh``` を実行してください。今度はスクリプトは何をするようになったのでしょうか？

   .. figure:: images/more2.png

#. 変更したblueprint.pyをCalmに送信します。

   -   ``cd ..`` を実行し、HelloBlueprintディレクトリに戻ります。
   -   ``calm create bp --file blueprint.py --name あなたのイニシャル-HelloDsl`` を実行します。これはblueprint.pyファイルをjsonに変換し、Calmにプッシュします。

   .. figure:: images/syncbp.png

   -  **(任意)** ``calm compile bp -f blueprint.py`` を実行すると、Python形式からjson形式のPythonブループリントが表示されます。
   -   新しいブループリントを確認するには、 ``calm get bps -q`` を実行してください。ブループリントが正しく作成されていることを確認します。

   .. figure:: images/verifygrep.png

#. ブループリントからアプリケーションを起動します。

   -  新しいアプリを起動する前に ``calm get apps`` を実行して、現在のアプリをすべて確認してください。
   -  また、 ``calm get apps -q`` を実行することで、先ほどのブループリントで行ったようにアプリケーション名のみをリストすることができます。
   -  ``calm launch bp あなたのイニシャル-HelloDsl --app_name あなたのイニシャル-HelloDsl -i`` を実行します。

   .. figure:: images/launchbp.png

   -  ``calm describe app あなたのイニシャル-HelloDsl`` を実行し、アプリケーションの詳細を確認します。

#. アプリの **Status** が **running** になればアプリケーションの起動が完了し、nginxによるWebサーバが起動されます。

   .. figure:: images/describe.png

#. VM/アプリケーションのIPアドレスを取得します。

   -  ``calm describe app あなたのイニシャル-HelloDsl --out json | jq '.status.resources.deployment_list[].substrate_configuration.element_list[].address'`` を実行して、WebサーバのIPアドレスを取得します。

   .. figure:: images/jqout.png

#. ウェブブラウザでIPアドレスを入力すると、nginxによる **Welcome to DSL** のウェブページが表示されます。

   .. figure:: images/welcome2.png

#. Prism Centralにログインして確認し、作成したブループリント及びアプリケーションがGUI上でも反映されていることを確認します。

   -  DSLから作成したブループリントを確認
   -  DSLから起動したアプリケーションを確認

おわりに
++++++++

この演習では、Calm DSLを使用するだけでなく、vi, curl, grep, cat, pipe, redirects などのLinuxネイティブツールも使用しました。Calm DSL は、これらの強力なツールと組み合わせることで、Calmブループリントに対して柔軟な拡張を可能にします。このワークフローにgitを追加して変更を追跡したり、sedを使ってブループリントを修正したりする方法を考えてみましょう。

(任意)Git演習
++++++++

私たちのブループリントを git にプッシュしてみましょう。 始める前にgithub.comのアカウントが必要です。

#. git にログインして新しいレポジトリ、"dsl-blueprints"を作成します。

#. HelloBlueprintディレクトリから以下を実行します。

   -  ``echo "# dsl-blueprints" >> README.md`` - READMEを作成します
   -  ``git init`` - 作業ディレクトリで git を初期化します。
   -  ``git config --global user.email "<youremail>@example.com"`` - あなたのgithub ID
   -  ``git config --global user.name "<GitUserName>"`` -  あなたのgithub パスワード
   -  ``git config --global color.ui true`` - わかりやすいように色付けします
   -  ``git remote add origin https://github.com/<Githubユーザ名>/dsl-blueprints.git`` - あなたのリモートレポジトリを追加します。
   -  ``git remote -v`` - あなたのリモートレポジトリの詳細を確認します。

   .. figure:: images/gitsetup.png

   -  ``git status`` - gitにより管理されているコードセットを確認します。
   -  ``git add --all`` - カレントディレクトリ内のすべてのファイルをステージングに追加します。
   -  ``git status`` - ファイルを追加した後の変更点を確認します。

   .. figure:: images/gitstatus.png

   -  上の出力を見ると、いくつかの鍵があることがわかりますので、公開レポにプッシュされているので、それらを削除しましょう。
   -  ``git rm --cached .local -r`` - .localファイルを削除します。
   -  ``git status`` - コードセットを確認します。

   .. figure:: images/gitremove.png

   -  ``git commit -m "My DSL blueprints"`` - コードセットをコミットします。

   .. figure:: images/gitcommit.png

   -  ``git push -u origin master`` - Githubのリモートレポジトリに送信します。githubへのキーアクセスを設定しない限り、ユーザー/パスの入力を求められます。

   .. figure:: images/gitpush.png

   -  Githubのレポをチェックして、ファイルがプッシュされたことを確認してください。 あなたのブループリントはCalmとGithubの両方に存在ます。以下を実行し、ブループリント中のメモリを8に増やしてみます。
   -  ``sed -i 's/memory = 4/memory = 8/g' blueprint.py`` - linuxのsedツールを使ってメモリ設定を変更する
   -  ``git add blueprint.py`` - 変更内容をステージング環境に追加します。
   -  ``git commit -m "change memory"`` - 変更内容をコミットします。
   -  ``git push -u origin master`` - 変更内容をリモートレポジトリ(github)に送信します。
   -  githubに戻ると、blueprint.pyの "history"の下に新しいバージョンがあり、メモリが変更されています。

   .. figure:: images/diff.png

