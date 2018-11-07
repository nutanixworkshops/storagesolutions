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

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/14.png

Select **Enable Access Based Enumeration** and **Self Service Restore** and click **Create**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/15.png

In the **XD** VM console, open ``\\*intials*-Files.ntnxlab.local`` in **File Explorer**.

  .. note::

  The **XD** VM has already been created on your cluster. Alternatively, you can use any Windows VM joined to the ntnxlab.local domain to complete the following steps.

Right-click **home > Properties**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/vdi_ahv/lab7/19.png

Select the **Security** tab and click **Advanced**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/vdi_ahv/lab7/20.png

Select **Users (*intials*-Files\\Users)** and click **Remove**.

Click **Add**.

Click **Select a principal** and specify **Everyone** in the **Object Name** field. Click **OK**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/vdi_ahv/lab7/21b.png

Fill out the following fields and click **OK**:

  - **Type** - Allow
  - **Applies to** - This folder only
  - Select **Read & execute**
  - Select **List folder contents**
  - Select **Read**
  - Select **Write**

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/vdi_ahv/lab7/22.png

Click **OK > OK > OK**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/23b.png

In the **XD** VM console, open **Control Panel > Administrative Tools > Active Directory Users & Computers**.

Under **ntnxlab.local > Users**, right-click **devuser01 > Properties**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/17.png

Click **Profile**. Under **Home folder**, select **Connect** and specify ``\\team##-afs.ntnxlab.local\home\%username%`` as the path. Click **OK**. Repeat for the following user accounts: **devuser02**, **devuser03**, **devuser04**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/18.png

In **Prism > File Server > Share > home**, click **+ Add Quota Policy**. Fill out the following fields and click **Save**:

  - Select **Groups**
  - **Users or Group** - SSP Developers
  - **Quota** - 10 GiB
  - **Enforcement Type** - Hard Limit

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/20.png

.. note::

    The remainder of `Configuring SMB Home Share`_ should be completed **AFTER** the :ref:`citrix_lab` lab. Alternatively you can validate your file share configuration by logging into any domain-joined Windows VM as **NTNXLAB\\devuser01**, the VM does not strictly need to be a Citrix virtual desktop.

Open \http://<*XD-VM-IP*>/Citrix/StoreWeb in a browser on the same L3 LAN as your XD VM.

Log in as **NTNXLAB\\devuser01**.

Select the **Desktops** tab and click your **Personal Windows 10 Desktop** to launch the session.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/vdi_ahv/lab5/31.png

Open ``Z:\`` in **File Explorer** and create multiple files, with at least one populated text file.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/19.png

Open ``\\*intials*-Files.ntnxlab.local\home`` and observe your **%username%** directory is the only directory visible. Disable **Access Based Enumeration (ABE)** in **Prism > File Server > Share > home > Update** and try again.

After ~2 hours, validate the presense of **Self Service Restore Snapshots** in **Prism > File Server > Share > home**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/21a.png

  From **NTNXLAB\\devuser01's Personal Windows 10 Desktop** session, browse to your home directory. Open, modify, and save a text file. Right-click that file and select **Restore previous versions**. Open a previous version of the document corresponding to AFS snapshots and save as a new file.

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
