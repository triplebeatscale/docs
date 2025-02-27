# Managing shards in a {{ CH }} cluster

You can group several [shards](../concepts/sharding.md) of a {{ CH }} cluster in a _shard group_ and save tables in this group.

## Listing shard groups in a cluster {#list-shard-groups}

{% list tabs group=instructions %}

- Management console {#console}

  1. In the [management console]({{ link-console-main }}), go to the folder page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-clickhouse }}**.
  1. Click the cluster name and open the **{{ ui-key.yacloud.clickhouse.cluster.switch_shard-groups }}** tab.

- CLI {#cli}

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  To get a list of shard groups in a cluster, run the command:

  ```bash
  {{ yc-mdb-ch }} shard-groups list \
     --cluster-name=<cluster_name>
  ```

  You can request the cluster name with the [list of clusters in the folder](cluster-list.md#list-clusters).

- API {#api}

  To get a list of shard groups in a cluster, use the [listShardGroups](../api-ref/Cluster/listShardGroups.md) REST API method for the [Cluster](../api-ref/Cluster/index.md) resource or the [ClusterService/ListShardGroups](../api-ref/grpc/Cluster/listShardGroups.md) gRPC API call and provide the cluster ID in the `clusterId` request parameter.

  To find out the cluster ID, [get a list of clusters in the folder](cluster-list.md#list-clusters).

{% endlist %}

## Viewing detailed information about a shard group {#get-shard-group}

{% list tabs group=instructions %}

- Management console {#console}

  1. In the [management console]({{ link-console-main }}), go to the folder page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-clickhouse }}**.
  1. Click the cluster name and open the **{{ ui-key.yacloud.clickhouse.cluster.switch_shard-groups }}** tab.
  1. Select a shard group to view detailed information.

- CLI {#cli}

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  To view detailed information about a shard group in a cluster, run the command:

  ```bash
  {{ yc-mdb-ch }} shard-groups get \
    --cluster-name=<cluster_name> \
    --name=<shard_group_name>
  ```

  You can request the cluster name with the [list of clusters in the folder](cluster-list.md#list-clusters).

- API {#api}

  To get detailed information about a shard group, use the [getShardGroup](../api-ref/Cluster/getShardGroup.md) REST API method for the [Cluster](../api-ref/Cluster/index.md) resource or the [ClusterService/GetShardGroup](../api-ref/grpc/Cluster/getShardGroup.md) gRPC API call and provide the following in the request:
  * Cluster ID, in the `clusterId` parameter. To find out the cluster ID, [get a list of clusters in the folder](cluster-list.md#list-clusters).
  * Name of the shard group, in the `shardGroupName` parameter. To find out the name, [get a list of shard groups](#list-shard-groups) in the cluster.

{% endlist %}

## Creating a shard group {#create-shard-group}

{% list tabs group=instructions %}

- Management console {#console}

  1. In the [management console]({{ link-console-main }}), go to the folder page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-clickhouse }}**.
  1. Click the cluster name and open the **{{ ui-key.yacloud.clickhouse.cluster.switch_shard-groups }}** tab.
  1. Click **{{ ui-key.yacloud.mdb.shard-groups.button_add-group }}**.
  1. Fill in the form fields and click **{{ ui-key.yacloud.common.apply }}**.

- CLI {#cli}

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  To create a shard group in a cluster, run the command:

  ```bash
  {{ yc-mdb-ch }} shard-groups create \
    --cluster-name=<cluster_name> \
    --name=<shard_group_name> \
    --description=<shard_group_description> \
    --shards=<list_of_shard_names>
  ```

  Where `--shards` is the list of list of shard names to include in the group.

  You can request the cluster name with the [list of clusters in the folder](cluster-list.md#list-clusters).

  You can request a shard name with a [list of shards in the cluster](shards.md#list-shards).

- {{ TF }} {#tf}

  1. Open the current {{ TF }} configuration file with an infrastructure plan.

     For more information about creating this file, see [Creating clusters](cluster-create.md).
  1. Add the `shard_group` description section to the {{ mch-name }} cluster description.

     ```hcl
     resource "yandex_mdb_clickhouse_cluster" "<cluster_name>" {
       ...
       shard_group {
         name        = "<shard_group_name>"
         description = "<optional_shard_group_description>"
         shard_names = [
           # List of shards in the group
           "<shard_1_name>",
           ...
           "<shard_N_name>"
         ]
       }
     }
     ```

  1. Make sure the settings are correct.

     {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

  1. Confirm updating the resources.

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

  For more information, see the [{{ TF }} provider documentation]({{ tf-provider-resources-link }}/mdb_clickhouse_cluster).

  {% include [Terraform timeouts](../../_includes/mdb/mch/terraform/timeouts.md) %}

- API {#api}

  To create a shard group, use the [createShardGroup](../api-ref/Cluster/createShardGroup.md) REST API method for the [Cluster](../api-ref/Cluster/index.md) resource or the [ClusterService/CreateShardGroup](../api-ref/grpc/Cluster/createShardGroup.md) gRPC API call and provide the following in the request:
  * ID of the cluster in which you want to create a group, in the `clusterId` parameter. To find out the cluster ID, [get a list of clusters in the folder](cluster-list.md#list-clusters).
  * Name of the shard group, in the `shardGroupName` parameter.
  * List of shard names to include in the group, in the `shardNames` parameter. To find out the names, [get a list of shards](shards.md#list-shards) in the cluster.
  * Description of the shard group, in the `description` parameter, if required.

{% endlist %}

## Changing a shard group {#update-shard-group}

{% list tabs group=instructions %}

- Management console {#console}

  1. In the [management console]({{ link-console-main }}), go to the folder page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-clickhouse }}**.
  1. Click the cluster name and open the **{{ ui-key.yacloud.clickhouse.cluster.switch_shard-groups }}** tab.
  1. Click ![image](../../_assets/console-icons/ellipsis.svg) for the shard group you need and select **{{ ui-key.yacloud.common.edit }}**.

- CLI {#cli}

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  To change a shard group in a cluster, run the command:

  ```bash
  {{ yc-mdb-ch }} shard-groups update \
    --cluster-name=<cluster_name> \
    --name=<shard_group_name> \
    --description=<new_description_for_shard_group> \
    --shards=<new_list_of_shard_names>
  ```

  Where `--shards` is the new list of shard names to include in the group.

  This command replaces the existing list of shards in the group with the new one provided in the `--shards` parameter. Before running the command, make sure that you added all the appropriate shards in the new list.

  You can request the cluster name with the [list of clusters in the folder](cluster-list.md#list-clusters).

  You can request the name of the shard group with a [list of shard groups in the cluster](#list-shard-groups).

  You can request a shard name with a [list of shards in the cluster](shards.md#list-shards).

- {{ TF }} {#tf}

  1. Open the current {{ TF }} configuration file with an infrastructure plan.

     For more information about creating this file, see [Creating clusters](cluster-create.md).
  1. In the {{ mch-name }} cluster description, change the `shard_group` section to include the required shard group:

     ```hcl
     resource "yandex_mdb_clickhouse_cluster" "<cluster_name>" {
       ...
       shard_group {
         name        = "<new_name_for_shard_group>"
         description = "<new_description_for_shard_group>"
         shard_names = [
           # New list of shards in the group
           "<shard_1_name>",
           ...
           "<shard_N_name>"
         ]
       }
     }
     ```

  1. Make sure the settings are correct.

     {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

  1. Confirm updating the resources.

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

  For more information, see the [{{ TF }} provider documentation]({{ tf-provider-resources-link }}/mdb_clickhouse_cluster).

  {% include [Terraform timeouts](../../_includes/mdb/mch/terraform/timeouts.md) %}

- API {#api}

  To update a shard group, use the [updateShardGroup](../api-ref/Cluster/updateShardGroup.md) REST API method for the [Cluster](../api-ref/Cluster/index.md) resource or the [ClusterService/UpdateShardGroup](../api-ref/grpc/Cluster/updateShardGroup.md) gRPC API call and provide the following in the request:
  * ID of the cluster in which you want to change a group, in the `clusterId` parameter. To find out the cluster ID, [get a list of clusters in the folder](cluster-list.md#list-clusters).
  * Name of the shard group, in the `shardGroupName` parameter. To find out the name, [get a list of shard groups](#list-shard-groups) in the cluster.
  * New description for the shard group, in the `description` parameter, if required.
  * New list of shard names to included in the group, in the `shardNames` parameter, if required. To find out the names, [get a list of shards](shards.md#list-shards) in the cluster. This list will replace the current one, so make sure that you added all the appropriate shards in the new list.
  * Names of parameters to change, in the `updateMask` parameter.

  {% include [Note API updateMask](../../_includes/note-api-updatemask.md) %}

{% endlist %}

## Deleting a group of shards {#delete-shard-group}

Deleting a group of shards doesn't affect the shards in the group: they are kept in the cluster.

Tables created on the deleted group are kept, but they are disabled and attempts to query them result in errors. However, you can delete these tables before or after you delete the shard group.

{% list tabs group=instructions %}

- Management console {#console}

  1. In the [management console]({{ link-console-main }}), go to the folder page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-clickhouse }}**.
  1. Click the cluster name and open the **{{ ui-key.yacloud.clickhouse.cluster.switch_shard-groups }}** tab.
  1. Click ![image](../../_assets/console-icons/ellipsis.svg) for the shard group you need and select **{{ ui-key.yacloud.common.delete }}**.

- CLI {#cli}

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  To delete a shard group in a cluster, run the command:

  ```bash
  {{ yc-mdb-ch }} shard-groups delete \
     --cluster-name=<cluster_name> \
     --name=<shard_group_name>
  ```

  You can request the cluster name with the [list of clusters in the folder](cluster-list.md#list-clusters).

  You can request the name of the shard group with a [list of shard groups in the cluster](#list-shard-groups).

- {{ TF }} {#tf}

  1. Open the current {{ TF }} configuration file with an infrastructure plan.

     For more information about creating this file, see [Creating clusters](cluster-create.md).
  1. Delete the `shard_group` description section for the appropriate group from the {{ mch-name }} cluster description.
  1. Make sure the settings are correct.

     {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

  1. Type `yes` and press **Enter**.

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

  For more information, see the [{{ TF }} provider documentation]({{ tf-provider-resources-link }}/mdb_clickhouse_cluster).

  {% include [Terraform timeouts](../../_includes/mdb/mch/terraform/timeouts.md) %}

- API {#api}

  To delete a shard group, use the [deleteShardGroup](../api-ref/Cluster/deleteShardGroup.md) REST API method for the [Cluster](../api-ref/Cluster/index.md) resource or the [ClusterService/DeleteShardGroup](../api-ref/grpc/Cluster/deleteShardGroup.md) gRPC API call and provide the following in the request:
  * ID of the cluster to delete a group from, in the `clusterId` parameter. To find out the cluster ID, [get a list of clusters in the folder](cluster-list.md#list-clusters).
  * Name of the shard group, in the `shardGroupName` parameter. To find out the name, [get a list of shard groups](#list-shard-groups) in the cluster.

{% endlist %}

{% include [clickhouse-disclaimer](../../_includes/clickhouse-disclaimer.md) %}