# Managing {{ RD }} cluster hosts

You can add and remove cluster hosts and manage their settings. For information about moving cluster hosts to a different [availability zone](../../overview/concepts/geo-scope.md), see [this guide](host-migration.md).

## Getting a list of cluster hosts {#list}

{% list tabs group=instructions %}

- Management console {#console}

  1. Go to the [folder page]({{ link-console-main }}) and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-redis }}**.
  1. Click the name of the cluster you need and select the **{{ ui-key.yacloud.mdb.cluster.hosts.label_title }}** tab.

- CLI {#cli}

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  To get a list of cluster hosts, run the command:

  ```bash
  {{ yc-mdb-rd }} host list \
    --cluster-name=<cluster_name>
  ```

  Result:



  ```text
  +---------------------------------+----------------------+------------+---------+--------+-------------------+
  |              NAME               |      CLUSTER ID      | SHARD NAME |  ROLE   | HEALTH |      ZONE ID      |
  +---------------------------------+----------------------+------------+---------+--------+-------------------+
  | rc1a-...caf.{{ dns-zone }} | c9qb2q230gg1******** | shard1     | MASTER  | ALIVE  | {{ region-id }}-a     |
  | rc1b-...bgc.{{ dns-zone }} | c9qb2q230gg1******** | shard1     | REPLICA | ALIVE  | {{ region-id }}-b     |
  +---------------------------------+----------------------+------------+---------+--------+-------------------+
  ```




  You can request the cluster name with the [list of clusters in the folder](cluster-list.md#list-clusters).

- API {#api}

  To get a list of cluster hosts, use the [listHosts](../api-ref/Cluster/listHosts.md) REST API method for the [Cluster](../api-ref/Cluster/index.md) resource or the [ClusterService/ListHosts](../api-ref/grpc/Cluster/listHosts.md) gRPC API call and provide the cluster ID in the `clusterId` request parameter.

  To find out the cluster ID, [get a list of clusters in the folder](cluster-list.md).

{% endlist %}

## Creating a host {#add}

The number of hosts in {{ mrd-name }} clusters is limited by the CPU and RAM quotas available to DB clusters in your cloud. To check the resources currently in use, open the [Quotas]({{ link-console-quotas }}) page and find **{{ ui-key.yacloud.iam.folder.dashboard.label_mdb }}**.

{% note info %}

Public access to hosts can only be configured for clusters created with enabled TLS support.

{% endnote %}

{% list tabs group=instructions %}

- Management console {#console}

  To create a host:
  1. Go to the [folder page]({{ link-console-main }}) and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-redis }}**.
  1. Click the cluster name and go to the **{{ ui-key.yacloud.mdb.cluster.hosts.label_title }}** tab.
  1. Click **{{ ui-key.yacloud.mdb.cluster.hosts.action_add-host }}**.
  1. Specify the host parameters:
     * Availability zone.


     * Subnet (if the required subnet is not on the list, [create it](../../vpc/operations/subnet-create.md)).


     * [Priority for assigning the host as a master](../concepts/replication.md#master-failover).
     * If necessary, configure public access to the host.
     * If you are adding a host to a sharded cluster, select a shard.

- CLI {#cli}

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  To create a host:
  1. Request a list of cluster subnets to select one for the new host:

     ```bash
     yc vpc subnet list
	   ```

     Result:


     ```text
     +----------------------+-----------+-----------------------+---------------+------------------+
     |          ID          |   NAME    |       NETWORK ID      |     ZONE      |      RANGE       |
     +----------------------+-----------+-----------------------+---------------+------------------+
     | b0cl69a2b4c6******** | default-d | enp6rq72rndgr******** | {{ region-id }}-d | [172.16.0.0/20]  |
     | e2lkj9qwe762******** | default-b | enp6rq72rndgr******** | {{ region-id }}-b | [10.10.0.0/16]   |
     | e9b0ph42bn96******** | a-2       | enp6rq72rndgr******** | {{ region-id }}-a | [172.16.32.0/20] |
     | e9b9v22r88io******** | default-a | enp6rq72rndgr******** | {{ region-id }}-a | [172.16.16.0/20] |
     +----------------------+-----------+-----------------------+---------------+------------------+
     ```




     If the required subnet is not in the list, [create it](../../vpc/operations/subnet-create.md).


  1. View a description of the CLI command for adding a host:

     ```bash
     {{ yc-mdb-rd }} host add --help
     ```

  1. Run the add host command:

     ```bash
     {{ yc-mdb-rd }} host add \
       --cluster-name=<cluster_name> \
       --host zone-id=<availability_zone>,`
         `subnet-id=<subnet_ID>,`
         `assign-public-ip=<public_access>,`
         `replica-priority=<host_priority>,`
         `shard-name=<shard_name>
     ```

     Where:
     * `--cluster-name`: {{ mrd-name }} cluster name. You can retrieve it with a [list of clusters in a folder](cluster-list.md#list-clusters).
     * `--host`: Host parameters:
       * `zone-id`: [Availability zone](../../overview/concepts/geo-scope.md).
       * `subnet-id`: [Subnet ID](../../vpc/concepts/network.md#subnet). Specify if two or more subnets are created in the selected availability zone.
       * `assign-public-ip`: Internet access to the host via a public IP address, `true` or `false`.
       * `replica-priority`: Priority for assigning the host as a master if the [primary master fails](../concepts/replication.md#master-failover). It is only available for non-sharded clusters.
       * `shard-name`: Name of the shard to create the host in if the cluster is sharded.

- {{ TF }} {#tf}

  To create a host:
  1. Open the current {{ TF }} configuration file with an infrastructure plan.

     For more information about creating this file, see [Creating clusters](cluster-create.md).
  1. Add a `host` section to the {{ mrd-name }} cluster description:

     ```hcl
     resource "yandex_mdb_redis_cluster" "<cluster_name>" {
       ...
       host {
         zone             = "<availability_zone>"
         subnet_id        = "<subnet_ID>"
         assign_public_ip = <public_access>
         replica_priority = <host_priority>
         shard_name       = "<shard_name>"
       }
     }
     ```

     Where `assign_public_ip` is public access to the host, `true` or `false`.

  1. Make sure the settings are correct.

     {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

  1. Confirm updating the resources.

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

  For more information, see the [{{ TF }} provider documentation]({{ tf-provider-mrd }}).

  {% include [Terraform timeouts](../../_includes/mdb/mrd/terraform/timeouts.md) %}

- API {#api}

  To add a host, use the [addHosts](../api-ref/Cluster/addHosts.md) REST API method for the [Cluster](../api-ref/Cluster/index.md) resource or the [ClusterService/AddHosts](../api-ref/grpc/Cluster/addHosts.md) gRPC API call and provide the following in the request:
  * Cluster ID in the `clusterId` parameter. To find out the cluster ID, [get a list of clusters in the folder](cluster-list.md#list-clusters).
  * New host settings in one or more `hostSpecs` parameters.

{% endlist %}


{% note warning %}

If you cannot [connect](connect/index.md) to the host you added, check that the cluster [security group](../concepts/network.md#security-groups) is configured correctly for the host's subnet.

{% endnote %}


## Changing a host {#update}

{% include [mrd-public-access](../../_includes/mdb/mrd/note-public-access.md) %}

{% list tabs group=instructions %}

- Management console {#console}

  To change the parameters of the cluster host:
  1. Go to the [folder page]({{ link-console-main }}) and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-redis }}**.
  1. Click the cluster name and open the **{{ ui-key.yacloud.mdb.cluster.hosts.label_title }}** tab.
  1. Click ![image](../../_assets/console-icons/ellipsis.svg) in the host's row and select **{{ ui-key.yacloud.common.edit }}**.
  1. Set new settings for the host:

      1. Enable **{{ ui-key.yacloud.mdb.hosts.dialog.field_public_ip }}** if the host must be accessible from outside {{ yandex-cloud }}.
      1. Specify the [priority for assigning the host as a master](../concepts/replication.md#master-failover).

  1. Click **{{ ui-key.yacloud.mdb.hosts.dialog.button_choose }}**.

- CLI {#cli}

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  To change the parameters of the host, run the command:

  ```bash
  {{ yc-mdb-rd }} host update <host_name> \
    --cluster-name=<cluster_name> \
    --assign-public-ip=<public_access> \
    --replica-priority=<host_priority>
  ```

  Where `--assign-public-ip` is public access to the host, `true` or `false`.

  You can request the host name with a [list of cluster hosts](#list), and the cluster name, with a [list of clusters in the folder](cluster-list.md#list-clusters).

- {{ TF }} {#tf}

  To change the parameters of the cluster host:
  1. Open the current {{ TF }} configuration file with an infrastructure plan.

     For more information about creating this file, see [Creating clusters](cluster-create.md).
  1. In the {{ mrd-name }} cluster description, change the attributes of the `host` block corresponding to the host you are updating.

     ```hcl
     resource "yandex_mdb_redis_cluster" "<cluster_name>" {
       ...
       host {
         zone             = "<availability_zone>"
         subnet_id        = "<subnet_ID>"
         assign_public_ip = <public_access>
         replica_priority = <host_priority>
         shard_name       = "<shard_name>"
       }
     }
     ```

     Where `assign_public_ip` is public access to the host, `true` or `false`.

  1. Make sure the settings are correct.

     {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

  1. Confirm updating the resources.

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

  For more information, see the [{{ TF }} provider documentation]({{ tf-provider-mrd }}).

- API {#api}

  To update host parameters, use the [updateHosts](../api-ref/Cluster/updateHosts.md) REST API method for the [Cluster](../api-ref/Cluster/index.md) resource or the [ClusterService/UpdateHosts](../api-ref/grpc/Cluster/updateHosts.md) gRPC API call and provide the following in the request:
  * The ID of the cluster to update the host in, in the `clusterId` parameter. To find out the cluster ID, [get a list of clusters in the folder](cluster-list.md#list-clusters).
  * The name of the host you need to update in the `updateHostSpecs.hostName` parameter. To find out the name, [get a list of hosts in the cluster](#list).
  * The host's public access settings in the `updateHostSpecs.assignPublicIp` parameter.
  * [Host priority](../concepts/replication.md#master-failover) in the `updateHostSpecs.replicaPriority` parameter.
  * List of cluster configuration fields to update in the `updateMask` parameter.

  {% include [Note API updateMask](../../_includes/note-api-updatemask.md) %}

{% endlist %}


{% note warning %}

If you cannot [connect](connect/index.md) to the host you added, check that the cluster [security group](../concepts/network.md#security-groups) is configured correctly for the host's subnet.

{% endnote %}


## Removing a host {#remove}

You can remove a host from a {{ RD }} cluster if it is not the only host in it. To replace a single host, first create a new host and then remove the old one.

If the host is the master at the time of deletion, {{ mrd-name }} will automatically assign another replica as the master.

You cannot remove a host if the number of hosts in a cluster or cluster shard is equal to the minimum value. For more information, see [Quotas and limits](../concepts/limits.md#mrd-limits).

{% list tabs group=instructions %}

- Management console {#console}

  To remove a host from a cluster:
  1. Go to the [folder page]({{ link-console-main }}) and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-redis }}**.
  1. Click the cluster name and open the **{{ ui-key.yacloud.mdb.cluster.hosts.label_title }}** tab.
  1. In the appropriate cluster row, click ![image](../../_assets/console-icons/ellipsis.svg) and select **{{ ui-key.yacloud.common.delete }}**.
  1. In the window that opens, enable **Delete host** and click **{{ ui-key.yacloud.mdb.cluster.hosts.popup-confirm_button }}**.

- CLI {#cli}

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  To remove a host from the cluster, run:

  ```bash
  {{ yc-mdb-rd }} host delete <host_name> \
    --cluster-name=<cluster_name>
  ```

  You can request the host name with a [list of cluster hosts](#list), and the cluster name, with a [list of clusters in the folder](cluster-list.md#list-clusters).

- {{ TF }} {#tf}

  To remove a host from a cluster:
  1. Open the current {{ TF }} configuration file with an infrastructure plan.

     For more information about creating this file, see [Creating clusters](cluster-create.md).
  1. Delete the `host` block from the {{ mrd-name }} cluster description.
  1. Make sure the settings are correct.

     {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

  1. Type `yes` and press **Enter**.

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

  For more information, see the [{{ TF }} provider documentation]({{ tf-provider-mrd }}).

  {% include [Terraform timeouts](../../_includes/mdb/mrd/terraform/timeouts.md) %}

- API {#api}

  To delete a host, use the [deleteHosts](../api-ref/Cluster/deleteHosts.md) REST API method for the [Cluster](../api-ref/Cluster/index.md) resource or the [ClusterService/DeleteHosts](../api-ref/grpc/Cluster/deleteHosts.md) gRPC API call and provide the following in the request:
  * Cluster ID in the `clusterId` parameter. To find out the cluster ID, [get a list of clusters in the folder](cluster-list.md#list-clusters).
  * Name or array of names of hosts to delete in the `hostNames` parameter.

{% endlist %}
