---
title: How to create a {{ vpc-short-name }} static route
description: The default static route (0.0.0.0/0) is used for VMs with public IPs. If you need to create a NAT instance, create it in a separate subnet. To create a route table and add static routes to it, open the {{ vpc-name }} section in the folder you need to create a static route in. Select the network to create the route table in. Click Create route table.
---

# Creating static routes

{% note info %}

The `0.0.0.0/0` default static route is used for VMs with public IP addresses. If you need to [create a NAT instance](../../tutorials/routing/nat-instance/index.md), create it in a separate subnet.

{% endnote %}

{% list tabs group=instructions %}

- Management console {#console}

  To create a route table and add [static routes](../concepts/routing.md):
  1. In the [management console]({{ link-console-main }}), go to the folder you need to create a static route in.
  1. In the list of services, select **{{ ui-key.yacloud.iam.folder.dashboard.label_vpc }}**.
  1. In the left-hand panel, select ![image](../../_assets/console-icons/route.svg) **{{ ui-key.yacloud.vpc.network.switch_route-table }}**.
  1. Click **{{ ui-key.yacloud.common.create }}**.
  1. Enter a name for the route table. The naming requirements are as follows:

     {% include [name-format](../../_includes/name-format.md) %}

  1. (Optional) Add a description of a route table.
  1. Select the network to create the route table in.
  1. Click **{{ ui-key.yacloud.vpc.route-table-form.label_add-static-route }}**.
  1. In the window that opens, enter the prefix of the destination subnet in CIDR notation.
  1. Specify the **{{ ui-key.yacloud.vpc.add-static-route.field_next-hop-address }}**, which is an IP address from the [allowed ranges](../concepts/network.md#subnet).
  1. Click **{{ ui-key.yacloud.vpc.add-static-route.button_add }}**.
  1. Click **{{ ui-key.yacloud.vpc.route-table.create.button_create }}**.

  To use static routes, link the route table to a subnet:

  1. In the left-hand panel, select ![image](../../_assets/console-icons/nodes-right.svg) **{{ ui-key.yacloud.vpc.switch_networks }}**.
  1. In the row with the subnet you need, click ![image](../../_assets/console-icons/ellipsis.svg).
  1. In the menu that opens, select **{{ ui-key.yacloud.vpc.subnetworks.button_action-add-route-table }}**.
  1. In the window that opens, select the created table from the list.
  1. Click **{{ ui-key.yacloud.vpc.subnet.add-route-table.button_add }}**.

- CLI {#cli}

  To create a route table and add [static routes](../concepts/routing.md):
  1. View the description of the CLI command for creating route tables:

     ```
     yc vpc route-table create --help
     ```

  1. Get the IDs of cloud networks in your cloud:

     ```
     yc vpc network list
     ```

     Result:
     ```
     +----------------------+-----------------+
     |          ID          |      NAME       |
     +----------------------+-----------------+
     | enp34hbpj8dq******** | yc-auto-subnet  |
     | enp846vf5fus******** | routes-test     |
     +----------------------+-----------------+
     ```

  1. Create a route table in one of the networks:

     ```bash
     yc vpc route-table create \
       --name=test-route-table \
       --network-id=enp846vf5fus******** \
       --route destination=0.0.0.0/0,next-hop=192.168.1.5
     ```

     Where:

     * `--name`: Name of the route table.
     * `--network-id`: ID of the network the table will be created in.
     * `--route`: Route settings, which include these two parameters:
        * `destination`: Destination subnet prefix in CIDR notation.
        * `next-hop`: Internal IP address of the VM from the [allowed ranges](../concepts/network.md#subnet) the traffic will be sent through.

     Result:
     ```
     ...done
     id: enpsi6b08q2v********
     folder_id: b1gqs1teo2q2********
     created_at: "2019-06-24T09:57:54Z"
     name: test-route-table
     network_id: enp846vf5fus********
     static_routes:
     - destination_prefix: 0.0.0.0/0
       next_hop_address: 192.168.1.5
     ```

  To use static routes, link the route table to a subnet:

  1. Get a list of subnets in your cloud:

     ```
     yc vpc subnet list
     ```

     Result:
     ```
     +----------------------+------------------+----------------------+----------------+---------------+------------------+
     |          ID          |       NAME       |      NETWORK ID      | ROUTE TABLE ID |     ZONE      |      RANGE       |
     +----------------------+------------------+----------------------+----------------+---------------+------------------+
     | b0cf2b0u7nhl******** | subnet-1         | enp846vf5fus******** |                | {{ region-id }}-a | [192.168.0.0/24] |
     +----------------------+------------------+----------------------+----------------+---------------+------------------+
     ```

  1. Link the route table to one of the subnets:

     ```
     yc vpc subnet update b0cf2b0u7nhl******** --route-table-id enp1sdveovdp********
     ```

     Result:
     ```
     ..done
     id: b0cf2b0u7nhl********
     folder_id: b1gqs1teo2q2********
     created_at: "2019-03-12T13:27:22Z"
     name: subnet-1
     network_id: enp846vf5fus********
     zone_id: {{ region-id }}-a
     v4_cidr_blocks:
     - 192.168.0.0/24
     route_table_id: enp1sdveovdp********
     ```

- {{ TF }} {#tf}

  {% include [terraform-definition](../../_tutorials/_tutorials_includes/terraform-definition.md) %}

  {% include [terraform-install](../../_includes/terraform-install.md) %}

  To create a route table and add [static routes](../concepts/routing.md):

  1. In the configuration file, describe the parameters of the resources you want to create:

     * `name`: Name of the route table. The name format is as follows:

          {% include [name-format](../../_includes/name-format.md) %}

     * `network_id`: ID of the network the table will be created in.
     * `static_route`: Static route description:
        * `destination_prefix`: Destination subnet prefix in CIDR notation.
        * `next_hop_address`: Internal IP address of the VM from the [allowed ranges](../concepts/network.md#subnet) the traffic will be routed through.

     Here is an example of the configuration file structure:

     ```hcl
     resource "yandex_vpc_route_table" "lab-rt-a" {
       name       = "<route_table_name>"
       network_id = "<network_ID>"
       static_route {
         destination_prefix = "10.2.0.0/16"
         next_hop_address   = "172.16.10.10"
       }
     }
     ```

     To add, update, or delete a route table, use the `yandex_vpc_route_table` resource indicating the network in the `netword id` field, e.g. `network_id = "${yandex_vpc_network.lab-net.id}"`.

     For more information about the `yandex_vpc_route_table` resource parameters in {{ TF }}, see the [provider documentation]({{ tf-provider-resources-link }}/vpc_route_table).

  1. Make sure the configuration files are correct.

     1. In the command line, go to the folder where you created the configuration file.
     1. Run a check using this command:

        ```
        terraform plan
        ```

     If the configuration is described correctly, the terminal will display a list of created resources and their parameters. If the configuration contains any errors, {{ TF }} will point them out.

  1. Deploy cloud resources.

     1. If the configuration does not contain any errors, run this command:

        ```
        terraform apply
        ```

     1. Confirm creating the resources: type `yes` in the terminal and press **Enter**.

        All the resources you need will then be created in the specified folder. You can check the new resources and their settings using the [management console]({{ link-console-main }}) or this [CLI](../../cli/quickstart.md) command:

        ```
        yc vpc route-table list
        ```

- API {#api}

  To create a route table and add [static routes](../concepts/routing.md) to it, use the [create](../api-ref/RouteTable/create.md) REST API method for the [RouteTable](../api-ref/RouteTable/index.md) resource or the [RouteTableService/Create](../api-ref/grpc/RouteTable/create.md) gRPC API call, and provide the following in the request:

  * ID of the folder the route table will reside in, in the `folderId` parameter.
  * Route table name in the `name` parameter. The name format is as follows:

     {% include [name-format](../../_includes/name-format.md) %}
  * ID of the network the route table will reside in, in the `networkId` parameter.
  * Destination subnet prefix in CIDR notation in the `staticRoutes[].destinationPrefix` parameter.
  * Internal IP address of the VM the traffic will be routed through in the `staticRoutes[].nextHopAddress` parameter. The IP address must be within the [allowed range](../concepts/network.md#subnet).

  To use static routes, link the route table to a subnet. Use the [update](../api-ref/Subnet/update.md) REST API method for the [Subnet](../api-ref/Subnet/index.md) resource or the [SubnetService/Update](../api-ref/grpc/Subnet/update.md) gRPC API call and provide the following in the request:

  * Network ID in the `subnetId` parameter.

    {% include [get-subnet-id](../../_includes/vpc/get-subnet-id.md) %}

    {% include [get-catalog-id](../../_includes/get-catalog-id.md) %}

  * Route table ID in the `routeTableId` parameter.
  * The name of the `routeTableId` parameter in the `updateMask` parameter.

  {% include [Note API updateMask](../../_includes/note-api-updatemask.md) %}

{% endlist %}
