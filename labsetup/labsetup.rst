.. _labsetup:

----------------------
Calm ラボの準備
----------------------

Projectを作成する
+++++++++++++++++++++

このラボでは、事前に構築された複数のCalmブループリントを活用して、アプリケーションのプロビジョニングを行います。

#. **Prism Central** で :fa:`bars` **> Services > Calm** を選択します。

#. 左側のメニューから **Project** を選択し、 **+Create Project** をクリックします。

   .. figure:: images/2.png

#. 以下のフィールドに入力します。

   - **Project Name** - **あなたのイニシャル** -Project
   - **Users, Groups, and Roles** の配下で **+ User** を選択します。
      - **Name** - Administrators
      - **Role** - Project Admin
      - **Action** - Save
   - **Infrastructure** の配下で **Select Provider > Nutanix** を選択します。
   - **Select Clusters & Subnets** をクリックします。
   - **あなたに割り当てられたクラスタ** を選択します。
   - **Subnets** にて **Primary** と **Secondary** を選択し、 **Confirm** をクリックします。
   - :fa:`star` をクリックし、 **Primary** をデフォルトのネットワークに設定します。 

   .. figure:: images/3.png

#. **Save & Configure Environment** をクリックします。

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
