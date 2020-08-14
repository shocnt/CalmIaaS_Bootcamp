.. _calm_dsl:

--------------------
Calm: DSLのクイックスタート
--------------------

はじめに
++++++++

DSLの演習をすぐに始められるように [DevWorkStation.json](https://raw.githubusercontent.com/bmp-ntnx/QuickStartCalmDSL/master/DevWorkstation.json) のブループリントを用意しました。同梱されているDevWorkstation.jsonは、必要なツールをすべて含んだ CentOS VMを構築します。この設計図はCalmから直接起動することができますが、セルフサービス用のCalm Marketplaceに公開することをお勧めします。 また、アイコンとして使用できる[software-developer.png](https://github.com/bmp-ntnx/QuickStartCalmDSL/blob/master/software-developer.png)も付属しています。

## CalmのマーケットプレイスからDevWorkstationを起動

![Alt text](images/MPDevWorkstation.png)

-   **重要: 認証情報タブを選択し、希望するユーザー/パスを入力します。**

![Alt text](images/Creds.png)

-   アプリケーション名 "DevWorkstation-あなたのイニシャル" を入力し、必要事項を記入してください。

![Alt text](images/DevLaunch.png)

-   **作成** をクリックします。

-   待機中に **監査** メニュー内のログを確認して、パッケージがデプロイされているのを確認してください。ブループリントは、Calm DSLと一緒にいくつかのユーティリティを自動的にインストールします。

## アプリケーションが **実行中** になったら、DevWorkstationにSSHでアクセスします。

-   DevWorkstationのIPアドレスは、アプリケーションの概要の下に記載されています。 SSHユーザー/パスは、認証情報タブで設定したものです。

![Alt text](images/IPaddress.png)

## 仮想環境を起動し、Prism Centralに接続します。

-   ユーザのホームディレクトリから ```cd calm-dsl``` を実行し、ディレクトリに移動します。

-   ```source venv/bin/activate``` のコマンドを実行して仮想環境に切り替えます。これでCalm DSLの仮想環境が有効になります。

-   **オプション:** これはブループリントの起動で既に行われています。一度DevWorkstationにSSHしたら、 ```calm init dsl``` を実行してPrism Centralへの接続を設定します。

-   現在の設定を確認するには ```calm show config``` を実行してください。


![Alt text](images/Config.png)

## Calmのブループリントを一覧で表示する

-   ```calm get bps``` を実行すると、Calm内のすべてのブループリントとUUID、説明、アプリケーション数、プロジェクト、状態が表示されます。

![Alt text](images/getbps.png)

-   ```calm get bps -q``` を実行すると、BP名のみを含む出力を表示することができます。

![Alt text](images/calmgetbpsq.png)

## Pythonベースのブループリントを見直して修正を加える

-   ```cd HelloBlueprint``` を実行し、ディレクトリに移動して、 ``ls``` を実行してください。

    -   このディレクトリとその内容はブループリントの起動時に自動的に作成されます。DevWorkstationのブループリント起動の一部として、接続されたCalmインスタンスに設定されたサンプルのブループリントを作成するために ```calm init bp``` を実行しました。

-   "blueprint.py"というファイルがありますが、これはpythonによって書かれたブループリントです。

-   "scripts"ディレクトリがあります。ここにはbash/powershell/pythonスクリプトが保存されていて、ブループリントの中で使用されます。

![Alt text](images/hellols.png)

-   ```vi blueprint.py``` を実行します。

-   ブループリントを見てみましょう。行に直接スキップするには ```:<行数>``` と入力してください。

    -   認証情報 (行 54-60)

    -   OSイメージ (行 62-66)

    -   class HelloPackage(Package)の下には、scriptsディレクトリのpkg\_install\_task.shスクリプトへの参照があります。 (行 139)

    -   VMの基本スペック情報（vCPU/メモリ/ディスク/NIC) (行 153-159)

    -   cloud-initによるゲストのカスタマイズ (教 161-171)

-   blueprint.pyでは、vCPUの数を変更します。

    -   vCPUを2から4に増やします。 (行 154)

![Alt text](images/vcpu.png)

-   マクロを使用して一意のVM名を追加する (行 185)

    -   ```provider_spec.name = "<あなたのイニシャル>-@@{calm_unique}@@"```

![Alt text](images/vmname.png)

-   Pythonによるブループリントファイルを保存して閉じるために、```:wq`````を書き込んで終了します。

## pkg\_install\_task.shの修正

-   scriptsディレクトリに移動して、 ```ls``` を実行してください。blueprint.pyの中で参照されている2つのスクリプトが表示されます。

-   現在のインストールスクリプトの内容を見るには、 ```cat pkg_install_task.sh``` を実行してください。このスクリプトは何をしているのでしょうか？

![Alt text](images/more1.png)

-   既存のインストールスクリプトを置き換えるために、 ```curl -Sks https://raw.githubusercontent.com/bmp-ntnx/prep/master/nginx > pkg_install_task.sh``` を実行してください。

-   変更されたスクリプトを見るには、 ```cat pkg_install_task.sh``` を実行してください。今度はスクリプトは何をするようになったのでしょうか？

![Alt text](images/more2.png)

## 変更したblueprint.pyをCalmに送信

-   HelloBlueprintディレクトリに戻ります

-   ```calm create bp --file blueprint.py --name FromDSL-<Initials>``` を実行します。

    -   これは.pyファイルをjsonに変換してCalmにプッシュします。

![Alt text](images/syncbp.png)

-   **任意** ```calm compile bp -f blueprint.py``` を実行すると、DSLからjson形式のPythonブループリントが表示されます。

-   新しいブループリントを確認するには、 ```calm get bps -q | grep FromDSL-<あなたのイニシャル>``` を実行してください。

![Alt text](images/verifygrep.png)

## ブループリントを起動

-   新しいアプリを起動する前に ```calm get apps``` を実行して、現在のアプリをすべて確認してください。

    -   また、 ```calm get apps -q``` を実行することで、先ほどのブループリントで行ったようにアプリケーション名のみをリストすることができます。

-   新しくアップロードしたブループリントをアプリケーションに起動する

-   ```calm launch bp FromDSL-<Initials> --app_name AppFromDSL-<Initials> -i``` を実行します。

![Alt text](images/launchbp.png)

-   ```calm describe app AppFromDSL-<Initials>``` を実行し、アプリケーションの詳細を確認します。

-   アプリのステータスが **実行中** になったら、Calm DSLからnginxサーバーをデプロイします。

![Alt text](images/describe.png)


<!--- -   ```calm describe app AppFromDSL-<Initials> --out json | grep -F '[{\"ip\":\"'``` を実行し、仮想マシンのIP --->

-   Now we need to get the VM/Application IP address.  To get this we will pull the "address" from the application json output using jq by running the following:

-   ```calm describe app AppFromDSL-<Initials> --out json | jq '.status.resources.deployment_list[].substrate_configuration.element_list[].address'```


<!--- ![Alt text](images/getip.png) --->
![Alt text](images/jqout.png)

-   Enter the IP in a web browser and this will take you to the nginx **"Welcome to DSL"** web page

![Alt text](images/welcome2.png)

## Log into Prism Central to verify

-   Check the blueprint created from DSL

-   Check the application launched from DSL

## Looking back

As you went through this lab not only did you use Calm DSL, but you also used several native Linux tools such as vi, curl, grep, cat, pipe, and redirects.  Calm DSL allows extended felxibily by combining it with these powerful tools.  Think about how you can add git to this workflow to track changes or modify blueprints with sed

## Optional: Getting started with git

Speaking of git lets contiue on and push our blueprint to git.  We will need a github.com account before you can get started

-   Logon to git and create new repo "dsl-blueprints"

-   From the "HelloBlueprint" directory run:

    - ```echo "# dsl-blueprints" >> README.md``` to create a README

    - ```git init``` initialize git in your working directory

    - ```git config --global user.email "<youremail>@example.com"```  identify yourself

    - ```git config --global user.name "<GitUserName>"``` identify yourself

    - ```git config --global color.ui true``` because colors are cool

    - ```git remote add origin https://github.com/<GitUserName>/dsl-blueprints.git``` to add your new github repo

    - ```git remote -v``` to verify your remote origin


    ![Alt text](images/gitsetup.png)

    - ```git status``` to see whats being tracked

    - ```git add --all``` adds all files in the current directory into staging

    - ```git status``` to see the change after adding the files


    ![Alt text](images/gitstatus.png)

    - From the above output we can see there are some keys so lets remove those since this is being pushed to a public repo

    - ```git rm --cached .local -r```

    - ```git status``` to verify they were removed


    ![Alt text](images/gitremove.png)

    - ```git commit -m "My DSL blueprints"``` to commit the files


    ![Alt text](images/gitcommit.png)

     - ```git push -u origin master``` to push to git.  You will be prompted for your user/pass unless you setup key access to github


    ![Alt text](images/gitpush.png)

     -  Check your github repo and verify your files were pushed.  Now that your blueprints exists in both Calm and github lets increase the memory to 8 in the blueprint by running:

        - ```sed -i 's/memory = 4/memory = 8/g' blueprint.py``` use the linux sed tool to change the memory config

        - ```git add blueprint.py```

        - ```git commit -m "change memory"```

        - ```git push -u origin master```

    - Back in github there is a new verion under the "history" of blueprint.py with the changed memory

    ![Alt text](images/diff.png)

    ## Looking back
