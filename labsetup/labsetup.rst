.. _labsetup:

----------------------
Calm: 演習の準備
----------------------

Projectを作成する
+++++++++++++++++++++

このラボでは、事前に構築された複数のCalmブループリントを活用して、アプリケーションのプロビジョニングを行います。

#. **Prism Central** で :fa:`bars` **> Services > Calm** を選択します。

#. 左側のメニューから **Project** を選択し、 **+プロジェクトを作成** をクリックします。

   .. figure:: images/2.png

#. 以下のフィールドに入力します。

   - **プロジェクト名** - **あなたのイニシャル** -Project
   - **ユーザー、グループ、ロール** の配下で **+ユーザー** を選択します。
      - **名前** - Administrators
      - **ロール** - Project Admin
      - **アクション** - Save
   - **インフラストラクチャ** の配下で **プロバイダを選択 > Nutanix** を選択します。
   - **クラスタおよびサブネットを選択** をクリックします。
   - **あなたに割り当てられたクラスタ** を選択します。
   - **サブネット** にて **Primary** と **Secondary** を選択し、 **確認** をクリックします。
   - :fa:`star` をクリックし、 **Primary** をデフォルトのネットワークに設定します。 

   .. figure:: images/3.png

#. **環境の保存および設定** をクリックします。

Deploying a Windows Tools VM
++++++++++++++++++++++++++++

Some exercises in this track will depend on leveraging the Windows Tools VM. Follow the below steps to provision your personal VM from a disk image.

#. In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > VMs**.

#. Click **+ Create VM**.

#. Fill out the following fields to complete the user VM request:

   - **Name** - *Initials*\ -WinToolsVM
   - **Description** - Manually deployed Tools VM
   - **vCPU(s)** - 2
   - **Number of Cores per vCPU** - 1
   - **Memory** - 4 GiB

   - Select **+ Add New Disk**
      - **Type** - DISK
      - **Operation** - Clone from Image Service
      - **Image** - WinToolsVM.qcow2
      - Select **Add**

   - Select **Add New NIC**
      - **VLAN Name** - Secondary
      - Select **Add**

#. Click **Save** to create the VM.

#. Power on your *Initials*\ **-WinToolsVM**.
