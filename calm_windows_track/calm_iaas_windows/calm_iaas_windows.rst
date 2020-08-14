.. _calm_iaas_windows:

---------------------------------
Calm: WindowsによるInfrastructure as a Service構築
---------------------------------

*この演習の所要時間は約20分です。*

はじめに
++++++++

Nutanix Calmを使用すると、プライベートクラウドとパブリッククラウドの両方のインフラストラクチャにわたって、ビジネスアプリケーションをシームレスに選択、プロビジョニング、管理することができます。Nutanix Calmは、アプリのライフサイクル、モニタリング、および運用をVMやベアメタルサーバーなどの異なるインフラストラクチャにおいて提供します。Nutanix Calmは複数のプラットフォームをサポートしているため、単一のセルフサービスおよび自動化インターフェースを使用してすべてのインフラストラクチャを管理することができます。

Infrastructure-as-a-Service(IaaS)は、セルフサービスポータルを介してオンデマンドでリソースを迅速に提供する機能として定義されています。 現在、Nutanix Calmのユーザの大部分は、エンドユーザーに基本的なIaaS機能を提供するためにCalmを利用しています。一方、複雑で多階層のアプリケーションをオーケストレーションするためにNutanix Calmを利用するユーザもいます。

**このラボでは、Windowsベースの"Single VM Blueprint"を作成し、ブループリントを起動し、結果として得られた仮想マシンおよびアプリケーションを管理します。**

単一の仮想マシンのブループリントの作成
++++++++++++++++++++++++++++++

ブループリントとは、Nutanix Calmを使用してモデル化するすべてのアプリケーションまたはインフラストラクチャのフレームワークです。 複雑で多階層のアプリケーションは「マルチ仮想マシン/ポッド ブループリント」を利用しますが、「単一の仮想マシンのブループリント」のインターフェースはIaaSのユースケースにおいて利用可能です。 あなたの会社が利用しているインフラストラクチャの各タイプ（例えばWindows、CentOS、Ubuntuなど）を単一の仮想マシンのブループリントでモデル化することができ、エンドユーザーは必要に応じて繰り返しブループリントを起動してインフラストラクチャを作成することができます。結果として得られたインフラストラクチャ（「アプリケーション」と呼ばれています）は、Nutanix Guest Tools (NGT)の管理、リソースの変更、スナップショット、およびクローン作成を含めて、Calm内でライフサイクル全体を通して管理することができます。

このラボでは、**Windows Server 2016** のサーバを作成します。

#. **Prism Central** で、 :fa:`bars` **> サービス > Calm** を選択します。

   .. figure:: images/1_access_calm.png

#. 左側のツールバーの ｜blueprints| **Blueprints** を選択して、Calmのブループリントを表示および管理します。

   .. note::

     アイコンにマウスを当てるとメニューがテキストで表示されます。

#. **+ ブループリントの作成 > 単一の仮想マシンのブループリント** を選択します。

#. 以下の項目を記入します。

   - **名前** - *Initials*-Windows-IaaS
   - **説明** - ブループリントの説明を書きます。
   - **プロジェクト** - *Initials*-Project

   .. figure:: images/3_windows_1.png
       :align: center
       :alt: Windows 2016 Blueprint Settings

       Windows 2016 ブループリント設定

#. **仮想マシンの詳細** をクリックし、次のステップに進みます。

#. **仮想マシンの詳細**ページでは、以下のフィールドを入力します。

   - **名前** - Calm内部で用いる仮想マシン名。 デフォルトのままにしておくことができます。
   - **クラウド** - 仮想マシンの展開先となるクラウド。 **Nutanix** を設定します。
   - **オペレーティングシステム** - デプロイするOSの種類。Windowsを設定します。

   .. figure:: images/5_windows_2.png
       :align: center
       :alt: Windows 2016 VM Details

       Windows 2016 仮想マシンの詳細

#. 次のステップに進むには、 **VM設定** をクリックします。

#. このページでは、インフラストラクチャの様々な設定を指定していきます。

   - **一般構成**

     - **VM名** - ハイパーバイザー/クラウドに応じた仮想マシンの名前です。デフォルトのままでも構いません。
     - **vCPUs** - 4 ( **走る人** のアイコンをクリックしてフィールドを **ランタイム** としてマークすると、青くなります。これにより、エンドユーザーはブループリント起動時にこのフィールドを変更することができます。)
     - **vCPUあたりのコア数** - 1
     - **メモリー (GiB)** - 6 (このフィールドを **ランタイム** としてマークします。)

     .. figure:: images/7_windows_3.png
         :align: center
         :alt: Windows 2016 VM Configuration - General Configuration

         Windows 2016のVM設定 - 一般構成


   - **ゲストのカスタマイズ** - ゲストのカスタマイズでは、起動時に特定の設定を変更することができます。Linux OSでは「Cloud Init」、Windows OSでは「Sysprep」を使用します。 **ゲストのカスタマイズ** を選択し、以下のスクリプトを貼り付けます。インストールタイプ(Prepared)とドメインに参加(チェックなし)はデフォルトのままにしておきます。

     - Windows 2016

       .. literalinclude:: sysprep.xml
          :language: xml

       .. figure:: images/9_windows_4.png
           :align: center
           :alt: Windows 2016 Sysprep

           Windows 2016 Sysprep

     .. note::
        "@@{vm_password}@@"の文字に注意してください。Calm では、"@@{}@@"文字はマクロを表します。実行時には、マクロに遭遇した場合、Calmは自動的にマクロを適切な値に置き換えます。マクロは、システムで定義された値、VMプロパティ、または変数を表すことができます。このラボでは後ほど、"vm_password "という名前の変数を作成します。

   - **ディスク** - ディスクは、デプロイするVMやインフラストラクチャのストレージです。既存のイメージをベースにしている場合もあれば、VMが追加のストレージを利用できるように空のディスクをベースにしている場合もあります。例えば、Microsoft SQLサーバーでは、ベース OSのディスク、SQL Serverのバイナリディスク、データベースデータファイルディスク、TempDB ディスク、ロギングディスクが必要になるかもしれません。本演習では、既存のイメージをベースにした単一のディスクを使用します。

     - **タイプ** - ディスクの種類、これはデフォルトのままにします( **DISK** )。
     - **バスタイプ** - ディスクのバスタイプ、これはデフォルトのままにします( **SCSI** )。
     - **オペレーション** - ディスクがどのように提供されるか。"Allocate on Storage Container"は空のディスクを追加するために使用します。本演習では事前に定義された仮想マシンイメージを使用しているので、デフォルトの **Clone from Image Service** のままにしておきます。
     - **Image** - 仮想マシンのベースとなるイメージ。 **Windows2016.qcow2** を選択します。
     - **ブート可能な** - このディスクによりオペレーティングシステムを起動可能かどうか。最低でも1つのディスクがブート可能でなければなりません。ここではチェック済のままにしておきます。

     .. figure:: images/11_windows_5.png
         :align: center
         :alt: Windows 2016 VM Configuration - Disks

         Windows 2016のVM設定 - ディスク

   - **ブート設定** - VMの起動方法です。デフォルトの **Legacy BIOS** のままにしておきます。

   - **vGPUs** - VMにGPUが必要かどうか。 デフォルトのnoneのままにしておきます。

   - **カテゴリ** - カテゴリは、Nutanixのポートフォリオ内のいくつかの異なる製品とソリューションにまたがって利用されます。これらにより、セキュリティポリシー、保護ポリシー、アラートポリシー、およびプレイブックを適用するための属性データとしてのカテゴリを付与することができます。 ワークロードに対応するカテゴリを選択するだけで、これらすべてのポリシーが自動的に適用されます。しかし、このラボでは、このフィールドは **空白** のままにしておきます。

   .. figure:: images/12_boot_gpu_cat.png
       :align: center
       :alt: VM Configuration - Boot Configuration, vGPUs, and Categories

       VM設定 - Boot設定, vGPU, カテゴリ

   - **NICs** - ネットワークアダプタを使用すると、仮想マシンとの通信が可能になります。 **青色の+** をクリックし、ドロップダウンで **Primary** を選択し、 **動的** ラジオボタンを選択して、1つのNICを追加します。

   .. figure:: images/13_vm_nic.png
       :align: center
       :alt: VM Configuration - NICs

       VM Configuration - NICs

   - **シリアルポート** - VMに仮想シリアルポートが必要かどうか。デフォルトの **none** のままにしておきます。

   .. figure:: images/14_serial.png
       :align: center
       :alt: VM Configuration - Serial Ports

       VM Configuration - シリアルポート

#. ページの下部にある青い **保存** ボタンをクリックします。ゲストのカスタマイズに"vm_password"という未定義のマクロが含まれているため、不正なマクロに関するエラーが1つ発生していることが予想されます。追加のエラーがある場合は、次のセクションに進む前に必ず解決してください。

   .. figure:: images/15_error.png
       :align: center
       :alt: Blueprint Save - Error

       Blueprint Save - エラー


変数を定義する
++++++++++++++++++

変数は、ブループリントの拡張性を向上します。つまり、1つのブループリントを、その変数の設定に応じて複数の目的や環境で使用することができます。変数は、ブループリントの一部として保存された静的な値か、 **ランタイム** （ブループリントの起動時）に指定することができます。

単一の仮想マシンのブループリントでは、上部にある **アプリ変数** ボタンをクリックすると、変数にアクセスできます。デフォルトでは、変数は **文字列** として保存されますが、別の **データ型** (整数、複数行の文字列、日付、時刻、および日付時刻)を使用することもできます。これらのデータ型のいずれも、オプションで **秘匿情報** として設定することができます。また、より高度な **入力方法** もありますが、この演習の範囲外です。

変数は、 **@@{変数名}@@** という文字列（マクロと呼ばれる）を使用してオブジェクトに対して実行されるスクリプトで使用することができます。Calmは、仮想マシンに送信する前に変数を展開して適切な値に置き換えます。

#. 上部ペインの **アプリ変数** ボタンをクリックすると、変数メニューが表示されます。

#. 表示されたポップアップでは、現在変数が設定されていないことが表示されるはずです。先に進み、青い **+ 変数の追加** ボタンをクリックして変数を追加し、以下のフィールドに記入してください。

   - **左の列** において、 **走る人** のアイコンをクリックして、この変数を **ランタイム** としてマークします。
   - メインペインで、変数 **名前** を **vm_password** に設定します。 この名前は、仮想マシンのカスタマイズスクリプトのマクロ内の値と正確に一致しなければなりません（大文字小文字を含めて）。
   - **データのタイプ** はデフォルトの **String** のままにしてください。他のオプションについてはこの演習の範囲外です。
   - **値** には、エンドユーザが自分の仮想マシンパスワードを指定させたいので、空白のままにしておきます。
   - パスワードを秘匿化するため、 **秘密の** のチェックボックスをチェックします。
   - **追加オプションの表示** をクリックします。
   - **ラベル** フィールドを空白にします。
   - **説明** フィールドにおいて **"Administrator"ユーザのパスワードを入力してください。** と入力します。
   - **この変数を必須としてマーク** のチェックボックスをチェックします。これによりエンドユーザーにパスワード入力を必須とすることが出来ます。
   - 他の2つのチェックボックスは非チェックのままにしておきます。

     .. figure:: images/16_variable.png
         :align: center
         :alt: Variable - vm_password

         変数 - vm_password

#. 下までスクロールして、青い **完了** ボタンをクリックします。

#. **保存** をクリックします。秘密変数の値が空であることを示す **警告** が表示されます。これは、ブループリントを保存する際に秘密変数の値を決定する方法がないため、この警告が発生します。しかし、警告によって、ユーザーがブループリントを起動したり公開したりすることができなくなることはありません。その他の警告や赤いエラーが表示された場合は、先に進む前に問題を解決してください。

   .. figure:: images/17_warning.png
       :align: center
       :alt: Blueprint Save - Warning

       Blueprint Save - 警告


ブループリントの起動
+++++++++++++++++++++++

ブループリントが完成しましたが、保存ボタンの右側にあるボタンについて説明します。

- **公開** - マーケットプレイスへのブループリントの公開を要求することができます。ブループリントはプロジェクトと1:1のマッピングを持っているので、自分のプロジェクトのメンバーである他のユーザーだけがこのブループリントを起動することができます。しかし、ブループリントをマーケットプレイスに公開することで、管理者は作成したブループリントを複数プロジェクトのユーザに対して割り当てることができ、複数プロジェクトのエンドユーザーにセルフサービスを提供することができます。
- **ダウンロード** - このオプションは、ブループリントをJSON形式でダウンロードし、ソースコントロールシステムにチェックインしたり、別のCalmインスタンスにアップロードしたりすることができます。
- **起動** - これはブループリントを起動し、私たちのアプリケーションや仮想マシンをデプロイします。

#. **起動**ボタンをクリックして、以下のように入力してください。

    - **アプリケーションの名前** - *あなたのイニシャル* -Windows-IaaS
    - **vm_password** - Nutanix/4u

.. figure:: images/18_launch.png
    :align: center
    :alt: Blueprint Launch

    ブループリントの起動

#. **作成** をクリックすると、アプリケーションのページが表示されます。

アプリケーションの管理
+++++++++++++++++++++++++

アプリケーションが **Provisioning** 状態から **Running** 状態に変わるまで数分待ちます。 **エラー** 状態に変わった場合は、 **監査** タブに移動し、 **作成** アクションを展開して、問題のトラブルシューティングを開始します。

アプリケーションが **実行中** の状態になったら、UI上部のタブを見ていきます。

.. figure:: images/19_app_tabs.png
    :align: center
    :alt: Application Tabs

    アプリケーションタブ

- **概要** タブでは、指定された変数、発生したコスト（ショーバックはCALM設定で設定可能）、アプリケーションサマリー、および仮想マシンのサマリーについての情報が表示されます。
- **管理** タブでは、アプリケーション/インフラストラクチャに対するアクションを実行できます。 これには、基本的なライフサイクル（起動、再起動、停止、削除）、NGT管理（インストール、管理、アンインストール）、および基本的なVMリソースの編集を可能にする仮想マシンの更新が含まれます。
- **評価指標** タブでは、CPU、メモリ、ストレージ、ネットワークの使用率に関する詳細な情報を提供します。
- **リカバリーポイント** タブには、VMスナップショットの履歴が表示され、ユーザーはこれらのポイントのいずれかにVMをリストアすることができます。
- **監査** タブには、アプリケーションに対して実行されたすべてのアクション、アクションを実行した時間とユーザー、スクリプトの出力を含むアクションの結果に関する詳細な情報が表示されます。

次に、UIの右上で利用できる共通のVMタスクを表示します。

.. figure:: images/20_app_buttons.png
    :align: center
    :alt: Application Buttons

    アプリケーションボタン

- **クローン** ボタンを使用すると、既存のアプリケーションを、現在のアプリケーションとは別に管理可能な新しいアプリケーションに複製することができます。これはブループリントを再度起動することと同じです。
- **スナップショット** ボタンをクリックすると、VMの新しいリカバリポイントが作成され、VMをリストアすることができます。
- **コンソールを起動** ボタンを押すと、VMのコンソールウィンドウが開きます。
- **更新** ボタンをクリックすると、エンドユーザーは基本的なVM設定を変更することができます（これは **管理 > 仮想マシンの更新** アクションと同等です）。
- **削除** ボタンをクリックすると、基礎となるVMとCalmアプリケーションが削除されます（これは、 **Manage > App Delete** アクションと同等です）。

アプリケーションのページレイアウトに慣れてきたところで、メモリを追加して仮想マシンを更新していきたいですが、何かあったときにリカバリーできるような方法でやっていきましょう。

#. 右上の **スナップショット** ボタンをクリックし、表示されたポップアップに次のように入力します。

   - **スナップショット名** - before-update-@@{calm_time}@@ (他のオプションはデフォルトのままにします。)

   .. figure:: images/21_snapshot.png
       :align: center
       :alt: Application Snapshot

       アプリケーションのスナップショット

#. **保存** をクリックします。

#. **監査** タブにリダイレクトされていることに注意してください。 **スナップショット作成** アクションを展開して、スナップショットのタスクを表示します。 完了したら、 **リカバリーポイント** タブに移動し、新しいスナップショットがリストされていることを確認します。

#. 次に、右上の **コンソールを起動** ボタンをクリックし、仮想マシンにログインします。

   - **Username** - Administrator
   - **Password** - Nutanix/4u

#. To view the current memory on Windows, open a **Command Prompt**, and run **systeminfo | findstr Memory**.  Take note of the current memory allocated to your VM.

   .. figure:: images/23_windows_memory_before.png
       :align: center
       :alt: Windows Memory - Before Update

       Windows Memory - Before Update

#. Navigate back to the application page of Calm, and click the **Update** button in the upper right.  On the page that appears, increase the **Memory (GiB)** field by 2 GiB (For Windows, 8 GiB).

#. Click the blue **Update** button in the lower left.

#. Validate that the memory field has been increased by 2 GiB, and click **Confirm**.

   .. figure:: images/25_windows_confirm.png
       :align: center
       :alt: Windows Memory - Confirm Change

       Windows Memory - Confirm Change

#. In the **Audit** tab of Calm, wait for the **App Update** action to complete.

#. Back in the **VM Console**, run the same command from earlier to view the updated memory, and note that it has increased by 2 GiB.

   .. figure:: images/27_windows_memory_after.png
       :align: center
       :alt: Windows Memory - After Update

       Windows Memory - After Update

   .. note::

      If anything went wrong with the VM Update, navigate to the **Recovery Points** tab, click **Restore** on the **before-update** snapshot we took earlier, and click **Confirm** on the pop-up.

Adding your Blueprints to the Marketplace
+++++++++++++++++++++++++++++++++++++++++

Now that we know we have a good blueprint, lets publish it to he Marketplace.

Publishing the Blueprint
........................

#. Select |blueprints| **Blueprints** in the left hand toolbar to view and manage Calm blueprints.

#. Click your *Initials*\ **-Windows-IaaS** blueprint.

#. Click the **Publish** button, and enter the following:

   - **Name** - *initials*\ _Windows_IaaS
   - **Publish with secrets** - off
   - **Initial Version** - 1.0.0
   - **Description** - (Optional)

   .. figure:: images/28_windows_publish_bp.png
       :align: center
       :alt: Windows Publish Blueprint

       Windows Publish Blueprint

#. Click **Submit for Approval**.

   .. note::

     Publish with Secrets: By default, the secret values from the blueprint are not preserved while publishing. As a result, during the launch of the marketplace item, the secret values will either be patched from the environment or the user will have to fill them in.

     Set this flag if you do not want this behaviour and you would rather the secret values are preserved as is. *Credential passwords/keys and secret variables are considered secret values. While publishing with secrets, these values will be encrypted.*

Approving Blueprints
....................

#. Select |mktmgr-icon| **Marketplace Manager** in the left hand toolbar to view and manage Marketplace Blueprints.

#. You will see the list of Marketplace blueprints, and their versions listesd. Select **Approval Pending** at the top of the page.

#. Click your *intials*\ **_CentOS_IaaS** blueprint.

#. Review the available actions:

   - **Approve** - Approves the Blueprint for publication to the Marketplace.
   - **Reject** - Prevents  Blueprint from being launched or published in the Marketplace. The Blueprint will need to be submitted again after being rejected before it can be published.
   - **Delete** - Deletes the blueprint submission to the Marketplace.
   - **Launch** - Launches the Blueprint as an application, similar to launching from the Blueprint Editor.

#. Review the available selections:

   - **Category** - Allows you to update the Category for the new Marletplace blueprint.
   - **Projects Shared With** - Allows you to make the Marketplace blueprint only available to a certain project.

#. Click **Approve**.

   .. figure:: images/29_windows_approve_bp.png
       :align: center
       :alt: Windows Approve Blueprint

       Windows Approve Blueprint

#. Select **Marketplace Blueprints** at the top of the page, and enter your *initials* in the search bar. You should see your blueprint listed now, with a Status of **Accepted**.

   .. figure:: images/30_windows_marketplace_bp.png
       :align: center
       :alt: Windows Marketplace Blueprint

       Windows Marketplace Blueprint

Launching your Blueprint from the Marketplace
+++++++++++++++++++++++++++++++++++++++++++++

Now that we have published our blueprint to the Marketplace, we need to make an update to our *initials*\ -Project.

Configuring Project Environment
...............................

#. To launch a Blueprint directly from the Marketplace, we need to ensure our Project has all of the requisite environment details to satisfy the Blueprint.

#. Select **Projects** from the lefthand menu.

#. Select your *initials*\ -Project.

#. Select the **Environment** tab.

#. Under **Credential**, click :fa:`plus-circle` and enter the following:

   - **Credential Name** - Administrator
   - **Username** - Administrator
   - **Secret** - Password
   - **Password** - Nutanix/4u
   - Click the **running man** icon above Password box to mark this variable as **runtime**.

   .. figure:: images/32_windows_project_creds.png
       :align: center
       :alt: Windows Project Credential

       Windows Project Credential

#. Under **VM Configuration** expand **Windows**, and enter the following:

   - select **NUTANIX**
   - **VM Name** - vm-@@{calm_array_index}@@-@@{calm_time}@@ (Default)
   - **vCPUs** - 4
   - **Cores per vCPU** - 1
   - **Memory** - 6GiB
   - **Image** - Windows2016.qcow2
   - **NICs** - Click the **blue plus**, then selecting **Primary** in the dropdown, and selecting the **Dynamic** radio button.
   - **Check log-in upon create** - checked, and **Credential** - Administrator (Defined Above)

   .. figure:: images/33_windows_project_vmconfig.png
       :align: center
       :alt: Windows Project VM Config

       Windows Project VM Config

#. Click **Save**.

Launching the Blueprint from the Marketplace
............................................

#. Select |mktmgr-icon| **Marketplace Manager** in the left hand toolbar to view and manage Marketplace Blueprints.

#. Enter your *initials* in the search bar, and you should see your blueprint listed.

#. Select your *intials*\ **_Windows_IaaS** blueprint, and click **Launch** from the Marletplace.

   .. figure:: images/31_windows_marketplace_launch_bp.png
       :align: center
       :alt: Windows Marketplace Launce Blueprint

       Windows Marketplace Launch Blueprint

#. Select your *initials*\ **-Project** from the **Projects** dropdown.

#. Click **Launch**

#. Entrer the Following info, and click **Create**.

   - **Name of the Application** - *initials*\ -Windows-IaaS-2
   - **vm_password** - Nutanix/4u

#. Monitor the provisioning of the Blueprint until complete.

Takeaways
+++++++++

What are the key things you should know about **Nutanix Calm** and **Single VM Blueprints**?

- Nutanix Calm provides application and infrastructure automation natively within Prism, turning complex, week long ticketing processes, into one-click self service provisioning.

- While Multi VM blueprints enable the provisioning and lifecycle management of complex, multi-tiered applications, Single VM blueprints allows IT to provide Infrastructure-as-a-Service for their end users.

- Common day 2 operations, like snapshotting, restoring, cloning, and updating the infrastructure can all be done by end users directly within Calm.

.. |proj-icon| image:: ../images/projects_icon.png
.. |mktmgr-icon| image:: ../images/marketplacemanager_icon.png
.. |mkt-icon| image:: ../images/marketplace_icon.png
.. |bp-icon| image:: ../images/blueprints_icon.png
.. |blueprints| image:: images/blueprints.png
.. |applications| image:: images/blueprints.png
.. |projects| image:: images/projects.png
