.. _files_nfs_export:

------------------------------
Files: Create NFS Share Export
------------------------------

Overview
++++++++

.. note::

  Estimated time to complete: **1 HOUR**

In this exercise you will use Files to configure a NFS share.

Configuring NFS Export
++++++++++++++++++++++

In **Prism > File Server**, click **+ Share/Export**. Fill out the following fields and click **Next**:

  - **Name** - logs
  - **Protocol** - NFS
  - **Share/Export Type** - Non-Sharded Directories

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/22.png

Fill out the following fields and click **Create**:

  - **Authentication** - System
  - **Default Access** - No Access
  - Select **+ Add Client Exceptions**
  - **Clients with Read-Write Access** - *<Cluster IP Range>* (ex. 10.21.XX.*)

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/23.png

In **Prism > VM > Table**, click **+ Create VM**.

Fill out the following fields and click **Save**:

- **Name** - NFS-Client
- **Description** - CentOS VM for testing AFS NFS export
- **vCPU(s)** - 2
- **Number of Cores per vCPU** - 1
- **Memory** - 4 GiB
- Select **+ Add New Disk**

  - **Operation** - Clone from Image Service
  - **Image** - CentOS
  - Select **Add**
- Select **Add New NIC**

  - **VLAN Name** - Secondary
  - Select **Add**

Select the **NFS-Client** VM and click **Power on**.

Once the VM has started, click **Launch Console** and log in as **root** or connect via SSH.

Execute the following:

  .. code-block:: bash

    [root@CentOS ~]# yum install -y nfs-utils
    [root@CentOS ~]# mkdir /afsmnt
    [root@CentOS ~]# mount.nfs4 team##-afs.ntnxlab.local:/ /afsmnt/
    [root@CentOS ~]# df -kh
    Filesystem                      Size  Used Avail Use% Mounted on
    /dev/mapper/centos_centos-root  8.5G  1.7G  6.8G  20% /
    devtmpfs                        1.9G     0  1.9G   0% /dev
    tmpfs                           1.9G     0  1.9G   0% /dev/shm
    tmpfs                           1.9G   17M  1.9G   1% /run
    tmpfs                           1.9G     0  1.9G   0% /sys/fs/cgroup
    /dev/sda1                       494M  141M  353M  29% /boot
    tmpfs                           377M     0  377M   0% /run/user/0
    afs.ntnxlab.local:/             1.0T  7.0M  1.0T   1% /afsmnt
    [root@CentOS ~]# ls -l /afsmnt/
    total 1
    drwxrwxrwx. 2 root root 2 Mar  9 18:53 logs

Observe that the **logs** directory is mounted in ``/afsmnt/logs``.

Reboot the VM and observe the export is no longer mounted. To persist the mount, add it to ``/etc/fstab`` by executing the following:

  .. code-block:: bash

    echo 'team##-afs.ntnxlab.local:/logs /afsmnt nfs4' >> /etc/fstab

The following command will add 100 2MB files filled with random data to ``/afsmnt/logs``:

  .. code-block:: bash

    for i in {1..100}; do dd if=/dev/urandom bs=8k count=256 of=/afsmnt/logs/file$i; done

Return to **Prism > File Server > Share > logs** to monitor performance and usage.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/25.png

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
