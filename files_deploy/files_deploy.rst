-----------------------
Acropolis File Services
-----------------------

Overview
++++++++

.. note::

  Estimated time to complete: **1 HOUR**

  **Due to limited resources, this lab should be completed as a group.**

  AFS 3.0 requires AOS 5.6 or later and cannot be deployed on the AOS 5.5.0.6 cluster used for the remainder of Tech Summit labs. Refer to :ref:`cluster_details` to find the Cluster Virtual IP of the shared AOS 5.6 cluster assigned to your team. Do **NOT** deploy any additional VMs on the shared AOS 5.6 clusters.

In this exercise you will use Prism to deploy Acropolis File Services (AFS), a native, distributed file server solution for Nutanix clusters. You will configure both SMB and NFS shares, and familiarize yourself with new features of the AFS offering.

Deploy Acropolis File Services
++++++++++++++++++++++++++++++

In **Prism > File Server**, click **+ File Server**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/vdi_ahv/lab7/1.png

The AFS 3.0.0.1 package has been already been uploaded and the Data Services IP has been configured as 10.21.XXX.38. Click **Continue**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/vdi_ahv/lab7/7.png

Fill out the following fields and click **Next**:

  - **Name** - TEAM##-AFS (e.g. TEAM01-AFS)
  - **Domain** - ntnxlab.local
  - **File Server Size** - 1 TiB

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/8c.png

.. note:: Clicking **Custom Configuration** will allow you to alter the scale up and scale out sizing of the AFS VMs based on User and Throughput targets.

Select the **Primary - Managed** VLAN for the Client Network. Specify your cluster's **DC** VM IP as the **DNS Resolver IP**. Click **Next**.

.. note::

  In order for the AFS cluster to successfully find and join the **NTNXLAB.local** domain it is critical that the **DNS Resolver IP** is set to the **DC** VM IP **FOR YOUR CLUSTER**. By default, this field is set to the primary **Name Server** IP configured for the Nutanix cluster, **this value is incorrect and will not work.**

.. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/9c.png

Select the **Secondary - Managed** VLAN for the Storage Network. Click **Next**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/10c.png

..
 .. note::

  It is typically desirable to deploy AFS with dedicated networks for client and storage. By design, however, AFS does not allow client connections from the storage network in this configuration. As the Hosted POC environment only provides 2 subnets per cluster, a single network deployment of AFS provides the most flexibility to connect to shares/exports via the Primary or Secondary networks.

Fill out the following fields and click **Next**:

  - Select **Use SMB Protocol**
  - **Username** - Administrator@ntnxlab.local
  - **Password** - nutanix/4u
  - Select **Make this user a File Server admin**
  - Select **Use NFS Protocol**
  - **User Management and Authentication** - Unmanaged

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/11b.png

.. note:: Similar to NFSv3, in Unmanaged mode, users are only identified by UID/GID. NFS connections will still require an NFSv4 capable client.

Click **Next**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/11c.png

Review the configuration and click **Create**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/ts18/afs/12b.png

Monitor deployment progress in **Prism > Tasks**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/vdi_ahv/lab7/13.png

.. note::

  If you receive a warning regarding DNS record validation failure, this can be safely ignored. The shared cluster does not use the same DNS servers as your AFS cluster, and as a result is unable to resolve the DNS entries created when deploying AFS.

Upon completion, select the **AFS** server and click **Protect**.

Observe the default Self Service Restore schedules, this feature controls the snapshot schedule for Windows' Previous Versions functionality. Supporting Previous Versions allows end users to roll back changes to files without engaging storage or backup administrators. Note these local snapshots do not protect the file server cluster from local failures and that replication of the entire file server cluster can be performed to remote Nutanix clusters. Click **Close**.

  .. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/vdi_ahv/lab7/16.png

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
