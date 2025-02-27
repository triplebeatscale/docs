# Adding your own geobase in {{ mch-name }}

Geobases in {{ CH }} are text files containing the hierarchy and names of regions. You can add several alternative geobases to {{ CH }} to support different stances on how regions pertain to countries. For more information, see the [{{ CH }} documentation](https://{{ ch-domain }}/docs/ru/sql-reference/dictionaries/internal-dicts/).

To add your own geobase to a {{ CH }} cluster:

1. [Create a geobase](#create).
1. [Upload the geobase to {{ objstorage-full-name }}](#upload).
1. [Add the geobase to a {{ CH }} cluster](#add).

## Creating a geobase {#create}

1. Create a file named `regions_hierarchy.txt`. The file must be in [TSV tabular format](https://ru.wikipedia.org/wiki/TSV) without headers and with the following columns:
   * Region ID (UInt32)
   * Parent region ID (UInt32)
   * Region type (UInt8):
        * `1`: Continent
        * `3`: Country
        * `4`: Federal district
        * `5`: Region
        * `6`: City
   * Population (UInt32): Optional.
1. To add an alternative hierarchy of regions, create the `regions_hierarchy_<suffix>.txt` files with the same structure. To use an alternative geobase, pass this suffix when invoking the function. For example:

    * `regionToCountry(RegionID)`: Uses the default `regions_hierarchy.txt` dictionary.
    * `regionToCountry(RegionID, 'alt')`: Uses the dictionary with the `alt` suffix: `regions_hierarchy_alt.txt`.

1. Create a file named `regions_names.txt`. The file must be in [TSV tabular format](https://ru.wikipedia.org/wiki/TSV) without headers and with the following columns:

    * Region ID (UInt32)
    * Region name (String): Cannot contain tab or newline characters, even escaped ones.

1. To add region names in other languages to your geobase, create the `regions_names_<language_code>.txt` files with the same structure. For example, you can create `regions_names_en.txt` for English and `regions_names_tr.txt` for Turkish.
1. Create a `tar`, `tar.gz`, or `zip` archive from the geobase files.

## Uploading a geobase to {{ objstorage-full-name }} {#upload}

{{ mch-short-name }} only works with publicly readable geobases that are uploaded to {{ objstorage-full-name }}:


1. To bind your [service account](../../iam/concepts/users/service-accounts.md) to the cluster, [make sure](../../iam/operations/roles/get-assigned-roles.md) your account in {{ yandex-cloud }} is assigned the [iam.serviceAccounts.user](../../iam/security/index.md#iam-serviceAccounts-user) role or higher.
1. [Upload](../../storage/operations/objects/upload.md) the geobase archive to {{ objstorage-full-name }}.
1. [Connect a service account to a cluster](s3-access.md#connect-service-account). Using your [service account](../../iam/concepts/users/service-accounts.md) you will set up access to the geobase archive.
1. [Assign](s3-access.md#configure-acl) the `storage.viewer` role to the service account.
1. In the bucket's ACL, [add](../../storage/operations/buckets/edit-acl.md) the `READ` permission to the service account.
1. [Get a link](s3-access.md#get-link-to-object) to the geobase archive.


## Adding the geobase to the {{ CH }} cluster {#add}

{% list tabs group=instructions %}

- Management console {#console}

  1. In the [management console]({{ link-console-main }}), go to the folder page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-clickhouse }}**.
  1. Select the cluster and click **{{ ui-key.yacloud.mdb.cluster.overview.button_action-edit }}** in the top panel.
  1. Under **{{ ui-key.yacloud.mdb.forms.section_settings }}**, click **{{ ui-key.yacloud.mdb.forms.button_configure-settings }}**.
  1. In the **Geobase uri** field, enter a link to the geobase archive in {{ objstorage-full-name }}.

- CLI {#cli}

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    To add a geobase:

    1. View a description of the update cluster configuration CLI command:

        ```bash
        {{ yc-mdb-ch }} cluster update-config --help
        ```

    1. Run the command by providing the link to the archive with the connected geobase in the `geobase_uri` parameter:

        ```bash
        {{ yc-mdb-ch }} cluster update-config <cluster_name_or_ID> \
             --set geobase_uri="<link_to_archive_with_geobase_in_Object_Storage>"
        ```

        You can request the cluster ID and name with a [list of clusters in the folder](cluster-list.md#list-clusters).

- {{ TF }} {#tf}

    1. Open the current {{ TF }} configuration file with an infrastructure plan.

        For more information about creating this file, see [Creating clusters](cluster-create.md).

    1. Add the `geobase_uri` parameter with a link to the archive with the geobase to connect in {{ objstorage-full-name }} to the {{ mch-name }} cluster settings:

        ```hcl
        resource "yandex_mdb_clickhouse_cluster" "<cluster_name>" {
          ...
          clickhouse {
            config {
              geobase_uri = "<link_to_archive_with_geobase_in_Object_Storage>"
              ...
            }
          ...
          }
        ...
        }
        ```

    1. Make sure the settings are correct.

        {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

    1. Confirm updating the resources.

        {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

    For more information, see the [{{ TF }} provider documentation]({{ tf-provider-resources-link }}/mdb_clickhouse_cluster).

    {% include [Terraform timeouts](../../_includes/mdb/mch/terraform/timeouts.md) %}

- API {#api}

    To add a geobase to a {{ CH }} cluster, use the [update](../api-ref/Cluster/update.md) REST API method for the [Cluster](../api-ref/Cluster/index.md) resource or the [ClusterService/Update](../api-ref/grpc/Cluster/update.md) gRPC API call and provide the link to the geobase archive in {{ objstorage-name }} in the `geobaseUri` parameter.

    {% note warning %}

    This API method resets any cluster settings that are not provided explicitly in the request to their defaults. To avoid this, make sure to provide the names of the fields you want to change in the `updateMask` parameter.

    {% endnote %}

{% endlist %}


{% include [clickhouse-disclaimer](../../_includes/clickhouse-disclaimer.md) %}
