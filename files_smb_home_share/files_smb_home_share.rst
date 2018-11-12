.. _files_smb_home_share:

----------------------------
Files: Create SMB Home Share
----------------------------

Overview
++++++++

.. note::

  Estimated time to complete: **1 HOUR**

In this exercise you will use Files to configure a SMB share.

Configuring SMB Home Share
++++++++++++++++++++++++++

In **Prism > File Server**, click **+ Share/Export**. Fill out the following fields and click **Next**:

  - **Name** - home
  - **Protocol** - SMB
  - **Share/Export Type** - Home directory and User Profiles

.. figure:: images/

Select **Enable Access Based Enumeration** and **Self Service Restore** and click **Create**.

.. figure:: images/

Connect to SMB Share
++++++++++++++++++++

Windows VM for Testing SMB Share
................................

Use the **Windows2012-*initials* ** VM you created earlier in the "Deploying Workloads" lab.

If you have not deployed a Windows VM, follow this guide to deploy a Windows2012 VM:

In **Prism > VM > Table**, click **+ Create VM**.

Fill out the following fields and click **Save**:

- **Name** - SMB-Client-*intials*
- **Description** - Windows VM for testing Files SMB Shares
- **vCPU(s)** - 2
- **Number of Cores per vCPU** - 1
- **Memory** - 4 GiB
- Select **+ Add New Disk**

  - **Operation** - Clone from Image Service
  - **Image** - Windows2012R2
  - Select **Add**
- Select **Add New NIC**

  - **VLAN Name** - Primary
  - Select **Add**

Select the **SMB-Client-*intials* ** VM and click **Power on**.

Log into the VM and add it to the **ntnxlab.local** domain.

Connecting to SMB Share
.......................

.. note::

  You can use any Windows VM joined to the ntnxlab.local domain to complete the following steps.


Log into your Windows VM console, and open ``\\*intials*-Files.ntnxlab.local`` in **File Explorer**.

Right-click **home > Properties**.

.. figure:: images/

Select the **Security** tab and click **Advanced**.

.. figure:: images/

Select **Users (*intials*-Files\\Users)** and click **Remove**.

Click **Add**.

Click **Select a principal** and specify **Everyone** in the **Object Name** field. Click **OK**.

.. figure:: images/

Fill out the following fields and click **OK**:

  - **Type** - Allow
  - **Applies to** - This folder only
  - Select **Read & execute**
  - Select **List folder contents**
  - Select **Read**
  - Select **Write**

.. figure:: images/

Click **OK > OK > OK**.

.. figure:: images/

In your Windows VM console, open **Control Panel > Administrative Tools > Active Directory Users & Computers**.

Under **ntnxlab.local > Users**, right-click **devuser01 > Properties**.

.. figure:: images/

Click **Profile**. Under **Home folder**, select **Connect** and specify ``\\*intials*-Files.ntnxlab.local\home\%username%`` as the path. Click **OK**. Repeat for the following user accounts: **devuser02**, **devuser03**, **devuser04**.

.. figure:: images/

In **Prism > File Server > Share > home**, click **+ Add Quota Policy**. Fill out the following fields and click **Save**:

  - Select **Groups**
  - **Users or Group** - SSP Developers
  - **Quota** - 10 GiB
  - **Enforcement Type** - Hard Limit

.. figure:: images/

You can validate your file share configuration by logging into any domain-joined Windows VM as **NTNXLAB\\devuser01**.

Takeaways
+++++++++

  - Nutanix provides file services suitable for storing user profiles and application data via SMB or NFSv4.
  - AFS is capable of scaling up and out to meet workload requirements.
  - AFS has data protection built-in by leveraging native snapshots and replication. AFS 3.0 will also feature integration with 3rd party backup solutions.
  - AFS can be deployed on the same Nutanix cluster as your virtual desktops, resulting in better utilization of storage capacity and the elimination of an additional storage silo.
  - Supporting mixed workloads (e.g. virtual desktops and file services) is further enhanced by Nutanix's ability to mix different node configurations within a single cluster, such as:

    - Mixing storage heavy and compute heavy nodes
    - Expanding a cluster with Storage Only nodes to increase storage capacity without incurring additional virtualization licensing costs
    - Mixing different generations of hardware (e.g. NX-3460-G6 + NX-6235-G5)
    - Mixing all flash nodes with hybrid nodes
    - Mixing NVIDIA GPU nodes with non-GPU nodes
