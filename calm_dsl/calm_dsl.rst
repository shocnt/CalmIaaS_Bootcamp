.. _calm_dsl:

--------------------
Calm: DSLのクイックスタート
--------------------

はじめに
++++++++

DSLの演習をすぐに始められるように [DevWorkStation.json](https://raw.githubusercontent.com/bmp-ntnx/QuickStartCalmDSL/master/DevWorkstation.json) のブループリントを用意しました。同梱されているDevWorkstation.jsonは、必要なツールをすべて含んだ CentOS VMを構築します。この設計図はCalmから直接起動することができますが、セルフサービス用のCalm Marketplaceに公開することをお勧めします。 また、アイコンとして使用できる[software-developer.png](https://github.com/bmp-ntnx/QuickStartCalmDSL/blob/master/software-developer.pngも付属しています。

## CalmのマーケットプレイスからDevWorkstationを起動

.. figure:: images/MPDevWorkstation.png

-   **重要: 認証情報タブを選択し、希望するユーザー/パスを入力します。**

.. figure:: images/Creds.png

-   アプリケーション名 "DevWorkstation-あなたのイニシャル" を入力し、必要事項を記入してください。

.. figure:: images/DevLaunch.png

-   **作成** をクリックします。

-   待機中に **監査** メニュー内のログを確認して、パッケージがデプロイされているのを確認してください。ブループリントは、Calm DSLと一緒にいくつかのユーティリティを自動的にインストールします。

## アプリケーションが **実行中** になったら、DevWorkstationにSSHでアクセスします。

-   DevWorkstationのIPアドレスは、アプリケーションの概要の下に記載されています。 SSHユーザー/パスは、認証情報タブで設定したものです。

.. figure:: images/IPaddress.png

## 仮想環境を起動し、Prism Centralに接続します。

-   ユーザのホームディレクトリから ```cd calm-dsl``` を実行し、ディレクトリに移動します。

-   ```source venv/bin/activate``` のコマンドを実行して仮想環境に切り替えます。これでCalm DSLの仮想環境が有効になります。

-   **オプション:** これはブループリントの起動で既に行われています。一度DevWorkstationにSSHしたら、 ```calm init dsl``` を実行してPrism Centralへの接続を設定します。

-   現在の設定を確認するには ```calm show config``` を実行してください。


.. figure:: images/Config.png

## Calmのブループリントを一覧で表示する

-   ```calm get bps``` を実行すると、Calm内のすべてのブループリントとUUID、説明、アプリケーション数、プロジェクト、状態が表示されます。

.. figure:: images/getbps.png

-   ```calm get bps -q``` を実行すると、BP名のみを含む出力を表示することができます。

.. figure:: images/calmgetbpsq.png

## Pythonベースのブループリントを見直して修正を加える

-   ```cd HelloBlueprint``` を実行し、ディレクトリに移動して、 ``ls``` を実行してください。

    -   このディレクトリとその内容はブループリントの起動時に自動的に作成されます。DevWorkstationのブループリント起動の一部として、接続されたCalmインスタンスに設定されたサンプルのブループリントを作成するために ```calm init bp``` を実行しました。

-   "blueprint.py"というファイルがありますが、これはpythonによって書かれたブループリントです。

-   "scripts"ディレクトリがあります。ここにはbash/powershell/pythonスクリプトが保存されていて、ブループリントの中で使用されます。

.. figure:: images/hellols.png

-   ```vi blueprint.py``` を実行します。

-   ブループリントを見てみましょう。行に直接スキップするには ```:<行数>``` と入力してください。

    -   認証情報 (行 54-60)

    -   OSイメージ (行 62-66)

    -   class HelloPackage(Package)の下には、scriptsディレクトリのpkg\_install\_task.shスクリプトへの参照があります。 (行 139)

    -   VMの基本スペック情報（vCPU/メモリ/ディスク/NIC) (行 153-159)

    -   cloud-initによるゲストのカスタマイズ (教 161-171)

-   blueprint.pyでは、vCPUの数を変更します。

    -   vCPUを2から4に増やします。 (行 154)

.. figure:: images/vcpu.png

-   マクロを使用して一意のVM名を追加する (行 185)

    -   ```provider_spec.name = "<あなたのイニシャル>-@@{calm_unique}@@"```

.. figure:: images/vmname.png

-   Pythonによるブループリントファイルを保存して閉じるために、```:wq`````を書き込んで終了します。

## pkg\_install\_task.shの修正

-   scriptsディレクトリに移動して、 ```ls``` を実行してください。blueprint.pyの中で参照されている2つのスクリプトが表示されます。

-   現在のインストールスクリプトの内容を見るには、 ```cat pkg_install_task.sh``` を実行してください。このスクリプトは何をしているのでしょうか？

.. figure:: images/more1.png

-   既存のインストールスクリプトを置き換えるために、 ```curl -Sks https://raw.githubusercontent.com/bmp-ntnx/prep/master/nginx > pkg_install_task.sh``` を実行してください。

-   変更されたスクリプトを見るには、 ```cat pkg_install_task.sh``` を実行してください。今度はスクリプトは何をするようになったのでしょうか？

.. figure:: images/more2.png

## 変更したblueprint.pyをCalmに送信

-   HelloBlueprintディレクトリに戻ります

-   ```calm create bp --file blueprint.py --name FromDSL-<Initials>``` を実行します。

    -   これは.pyファイルをjsonに変換してCalmにプッシュします。

.. figure:: images/syncbp.png

-   **任意** ```calm compile bp -f blueprint.py``` を実行すると、DSLからjson形式のPythonブループリントが表示されます。

-   新しいブループリントを確認するには、 ```calm get bps -q | grep FromDSL-<あなたのイニシャル>``` を実行してください。

.. figure:: images/verifygrep.png

## ブループリントを起動

-   新しいアプリを起動する前に ```calm get apps``` を実行して、現在のアプリをすべて確認してください。

    -   また、 ```calm get apps -q``` を実行することで、先ほどのブループリントで行ったようにアプリケーション名のみをリストすることができます。

-   新しくアップロードしたブループリントをアプリケーションに起動する

-   ```calm launch bp FromDSL-<Initials> --app_name AppFromDSL-<Initials> -i``` を実行します。

.. figure:: images/launchbp.png

-   ```calm describe app AppFromDSL-<Initials>``` を実行し、アプリケーションの詳細を確認します。

-   アプリのステータスが **実行中** になったら、Calm DSLからnginxサーバーをデプロイします。

.. figure:: images/describe.png

-   ここでVM/アプリケーションのIPアドレスを取得する必要があります。 これを取得するために、 ```calm describe app AppFromDSL-<Initials> --out json | jq '.status.resources.deployment_list[].substrate_configuration.element_list[].address'``` を実行して、jqを使ってアプリケーションのjson出力から "IPアドレス"を取得します。

.. figure:: images/jqout.png

-   ウェブブラウザでIPアドレスを入力すると、nginxによる **"Welcome to DSL "** のウェブページが表示されます。

.. figure:: images/welcome2.png

## Prism Centralにログインして確認する

-   DSLから作成したブループリントを確認

-   DSLから起動したアプリケーションを確認

## 終わりに

この演習では、Calm DSLを使用するだけでなく、vi, curl, grep, cat, pipe, redirects などのLinuxネイティブツールも使用しました。Calm DSL は、これらの強力なツールと組み合わせることで、柔軟な拡張を可能にします。このワークフローにgitを追加して変更を追跡したり、sedを使ってブループリントを修正したりする方法を考えてみましょう。

## 任意: Gitとは

私たちのブループリントを git にプッシュしてみましょう。 始める前にgithub.comのアカウントが必要です。

-   git にログインして新しいレポジトリ、"dsl-blueeprints"を作成します。

-   HelloBlueprintディレクトリから以下を実行します。

    - ```echo "# dsl-blueprints" >> README.md``` : READMEを作成します

    - ```git init``` : 作業ディレクトリで git を初期化します。

    - ```git config --global user.email "<youremail>@example.com"``` : あなたのgithub ID

    - ```git config --global user.name "<GitUserName>"``` :  あなたのgithub パスワード

    - ```git config --global color.ui true``` : わかりやすいように色付けします

    - ```git remote add origin https://github.com/<GitUserName>/dsl-blueprints.git``` : あなたのリモートレポジトリを追加します。

    - ```git remote -v``` : あなたのリモートレポジトリの詳細を確認します。

    .. figure:: images/gitsetup.png

    - ```git status``` : gitにより管理されているコードセットを確認します。

    - ```git add --all``` : カレントディレクトリ内のすべてのファイルをステージングに追加します。

    - ```git status``` : ファイルを追加した後の変更点を確認します。

    .. figure:: images/gitstatus.png

    - 上の出力を見ると、いくつかの鍵があることがわかりますので、公開レポにプッシュされているので、それらを削除しましょう。

    - ```git rm --cached .local -r``` : .localファイルを削除します。

    - ```git status``` : コードセットを確認します。

    .. figure:: images/gitremove.png

    - ```git commit -m "My DSL blueprints"``` : コードセットをコミットします。

    .. figure:: images/gitcommit.png

     - ```git push -u origin master``` :  Githubのリモートレポジトリに送信します。githubへのキーアクセスを設定しない限り、ユーザー/パスの入力を求められます。

    .. figure:: images/gitpush.png

     -  Githubのレポをチェックして、ファイルがプッシュされたことを確認してください。 あなたのブループリントはCalmとGithubの両方に存在ます。以下を実行し、ブループリント中のメモリを8に増やしてみます。

        - ```sed -i 's/memory = 4/memory = 8/g' blueprint.py``` : linuxのsedツールを使ってメモリ設定を変更する

        - ```git add blueprint.py``` : 変更内容をステージング環境に追加します。

        - ```git commit -m "change memory"``` : 変更内容をコミットします。

        - ```git push -u origin master``` : 変更内容をリモートレポジトリ(github)に送信します。

    - githubに戻ると、blueprint.pyの "history"の下に新しいバージョンがあり、メモリが変更されています。

    .. figure:: images/diff.png

