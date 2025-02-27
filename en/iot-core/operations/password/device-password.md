# Managing device passwords

For devices and registries to begin exchanging data and commands, you need to [log in](../../concepts/authorization.md). This section describes how to manage device passwords for the appropriate authorization method.

{% include [pass-priority-note](../../../_includes/iot-core/pass-priority-note.md) %}

* [Adding a password](#create-or-add)
* [Viewing a password list](#list)
* [Deleting a password](#delete)

## Adding a password to a device {#create-or-add}

You can add a password to an already created device or set it when creating a device using the `--password` parameter.

{% include [read-pass](../../../_includes/iot-core/read-pass.md) %}

### Adding a password to an existing device {#add}

{% list tabs group=instructions %}

- Management console {#console}

   To add a password to an existing device:

   1. In the [management console]({{ link-console-main }}), select the folder where you want to add a password for an existing device.
   1. Select **{{ ui-key.yacloud.iam.folder.dashboard.label_iot-core }}**.
   1. Select the registry with the required device from the list.
   1. On the left side of the window, select the **{{ ui-key.yacloud.iot.label_devices }}** section.
   1. Select the device from the list.
   1. Under **{{ ui-key.yacloud.iot.label_passwords }}**, click **{{ ui-key.yacloud.iot.button_add-password }}**.
   1. In the **{{ ui-key.yacloud.common.password }}** field, enter the password you will use to access the device.<br/>To create a password, you can use the [password generator](https://passwordsgenerator.net/).<br/>Make sure to save your password, as you will need it later.
   1. Click **{{ ui-key.yacloud.common.add }}**.

- CLI {#cli}
  
    {% include [cli-install](../../../_includes/cli-install.md) %}

    To add a password: 
    1. Get a list of devices in the registry: 
    
        ```
        yc iot device --registry-name my-registry list
        ```
		
		Result:
		```
		+----------------------+--------+
        |          ID          |  NAME  |
        +----------------------+--------+
        | arenak5ciqss******** | second |
        | areqjd6un3af******** | first  |
        +----------------------+--------+
        ```    
    1. Add a password to the device:
    
        ```
        yc iot device password add --device-name first --password Passw0rdForDevice
        ```
		
		Result:
		```
		device_id: areqjd6un3af********
        id: areqjd6un3af********
        created_at: "2019-12-16T15:11:36.892167Z"
        ```

- {{ TF }} {#tf}

  {% include [terraform-definition](../../../_tutorials/_tutorials_includes/terraform-definition.md) %}
  
  {% include [terraform-install](../../../_includes/terraform-install.md) %}

  To add a password to a device created using {{ TF }}:
  
  1. In the configuration file, describe the parameters of the resource to create:

     * `yandex_iot_core_registry`: Device properties:
       * `registry_id`: [ID of the registry](../registry/registry-list.md#registry-list) where the device was created.
       * `name`: [Device name](../device/device-list.md#device-list).
       * `description`: Device description.
       * `passwords`: List of passwords for authorization with [login and password](../../concepts/authorization.md#log-pass).

      Here is an example of the resource structure in the configuration file:

      ```hcl
      resource "yandex_iot_core_device" "my_device" {
        registry_id = "<registry_ID>"
        name        = "<device_name>"
        description = "test device for terraform provider documentation"
      ...
        passwords = [
          "<password>",
        ]
      ...
      }
      ```

      For more information about the `yandex_iot_core_device` resource parameters in {{ TF }}, see the [relevant provider documentation]({{ tf-provider-resources-link }}/iot_core_device).
  1. In the command line, change to the folder where you edited the configuration file.
  1. Make sure the configuration file is correct using this command:

      ```bash
      terraform validate
      ```
     
      If the configuration is correct, you will get this message:
     
      ```bash
      Success! The configuration is valid.
      ```

  1. Run this command:

      ```bash
      terraform plan
      ```
  
      The terminal will display a list of resources with parameters. No changes will be made at this step. If the configuration contains any errors, {{ TF }} will point them out.
  1. Apply the configuration changes:

      ```bash
      terraform apply
      ```
     
  1. Confirm the changes: type `yes` into the terminal and press **Enter**.

      You can verify device passwords in the [management console]({{ link-console-main }}) or using the following [CLI](../../../cli/quickstart.md) command:

      ```bash
      yc iot device password list --device-name <device_name>
      ```

- API {#api}

  To add a password to a device, use the [addPassword](../../api-ref/Device/addPassword.md) REST API method for the [Device](../../api-ref/Device/index.md) resource or the [DeviceService/AddPassword](../../api-ref/grpc/Device/addPassword.md) gRPC API call.

{% endlist %}

### Setting a password for a device when creating it {#create}

{% list tabs group=instructions %}

- Management console {#console}

   For information about how to set a password for a device when creating it, see [{#T}](../device/device-create.md).

- CLI {#cli}
  
    {% include [cli-install](../../../_includes/cli-install.md) %}
    
    {% include [default-catalogue](../../../_includes/default-catalogue.md) %}
    
    To set a password when creating a device:
    1. Get a list of registries in the folder: 
        
        ```
        yc iot registry list
		```
		
		Result:
		```
        +----------------------+-------------------+
        |          ID          |       NAME        |
        +----------------------+-------------------+
        | arenou2oj4ct******** | my-registry       |
        +----------------------+-------------------+
        ```
    1. Create a device with a password:       
    
        ```
        yc iot device create --registry-name my-registry --name device-with-pass --password Passw0rdForDevice
        ```
		
		Result:
		```
		id: arepomfambsg********
        registry_id: arenou2oj4ct********
        created_at: "2019-12-16T15:18:39.358922Z"
        name: device-with-pass
        ```

- {{ TF }} {#tf}

   For information about how to set a password for a device when creating it, see [{#T}](../device/device-create.md).

- API {#api}

  To set a password for a device when creating it, use the [create](../../api-ref/Device/create.md) REST API method for the [Device](../../api-ref/Device/index.md) resource or the [DeviceService/Create](../../api-ref/grpc/Device/create.md) gRPC API call.

{% endlist %}

## Getting a list of device passwords {#list}

{% list tabs group=instructions %}

- Management console {#console}

   To view the list of device passwords:

   1. In the [management console]({{ link-console-main }}), select the folder to get the list of device passwords for.
   1. Select **{{ ui-key.yacloud.iam.folder.dashboard.label_iot-core }}**.
   1. Select the registry with the required device from the list.
   1. On the left side of the window, select the **{{ ui-key.yacloud.iot.label_devices }}** section.
   1. Select the device from the list.
   1. On the **{{ ui-key.yacloud.common.overview }}** page, go to the **{{ ui-key.yacloud.iot.label_passwords }}** section.

   The list of device passwords will be displayed in the **{{ ui-key.yacloud.iot.label_passwords }}** section.

- CLI {#cli}
  
    {% include [cli-install](../../../_includes/cli-install.md) %}
    
    {% include [default-catalogue](../../../_includes/default-catalogue.md) %}
    
    To get a list of passwords:  
    1. Get a list of devices in the registry: 
    
        ```
        yc iot device --registry-name my-registry list
        ```
		
		Result:
		```
		+----------------------+------------------+
        |          ID          |       NAME       |
        +----------------------+------------------+
        | arenak5ciqss******** | second           |
        | arepomfambsg******** | device-with-pass |
        | areqjd6un3af******** | first            |
        +----------------------+------------------+
        ```
    1. Get a list of device passwords: 
    
        ```
        yc iot device password list --device-name device-with-pass
        ```
		
		Result:
		```
		+----------------------+---------------------+
        |          ID          |     CREATED AT      |
        +----------------------+---------------------+
        | areuin5t7pnd******** | 2019-12-16 15:18:39 |
        +----------------------+---------------------+
        ```

- API {#api}

  To get a list of device passwords, use the [listPasswords](../../api-ref/Device/listPasswords.md) REST API method for the [Device](../../api-ref/Device/index.md) resource or the [DeviceService/ListPasswords](../../api-ref/grpc/Device/listPasswords.md) gRPC API call.

{% endlist %}
   
## Deleting a device password {#delete}

{% list tabs group=instructions %}

- Management console {#console}

   To delete a device password:

   1. In the [management console]({{ link-console-main }}), select the folder to delete a device password from.
   1. Select **{{ ui-key.yacloud.iam.folder.dashboard.label_iot-core }}**.
   1. Select the registry with the required device from the list.
   1. On the left side of the window, select the **{{ ui-key.yacloud.iot.label_devices }}** section.
   1. Select the device from the list.
   1. In the row with the password, click ![image](../../../_assets/console-icons/ellipsis.svg) and select **{{ ui-key.yacloud.common.delete }}** from the drop-down list.
   1. In the window that opens, click **{{ ui-key.yacloud.common.delete }}**.

- CLI {#cli}
  
    {% include [cli-install](../../../_includes/cli-install.md) %}
    
    {% include [default-catalogue](../../../_includes/default-catalogue.md) %}
    
    To delete a password:  
    1. Get a list of device passwords: 
    
        ```
        yc iot device password list --device-name device-with-pass
        ```
		
		Result:
		```
		+----------------------+---------------------+
        |          ID          |     CREATED AT      |
        +----------------------+---------------------+
        | areuin5t7pnd******** | 2019-12-16 15:18:39 |
        +----------------------+---------------------+
        ```
    1. Delete the password: 
        ```
        yc iot device password delete --device-name device-with-pass --password-id areuin5t7pnd********
        ```
    1. Make sure that the password was deleted: 
        
        ```
        yc iot device password list --device-name device-with-pass
        ```
		
		Result:
		```
		+----+------------+
        | ID | CREATED AT |
        +----+------------+
        +----+------------+
        ```

- {{ TF }} {#tf}

  {% include [terraform-definition](../../../_tutorials/_tutorials_includes/terraform-definition.md) %}
  
  {% include [terraform-install](../../../_includes/terraform-install.md) %}

  To delete the password of a device created using {{ TF }}:
  
  1. Open the {{ TF }} configuration file and delete the password value in the `passwords` section, in the device description fragment. To delete all passwords, delete the entire `passwords` section.

      Example device description in the {{ TF }} configuration:

      ```hcl
      resource "yandex_iot_core_device" "my_device" {
        registry_id = "<registry_ID>"
        name        = "<device_name>"
        description = "test device for terraform provider documentation"
      ...
        passwords = [
          "<password>",
        ]
      ...
      }
      ```

      For more information about the `yandex_iot_core_device` resource parameters in {{ TF }}, see the [relevant provider documentation]({{ tf-provider-resources-link }}/iot_core_device).
  1. In the command line, change to the folder where you edited the configuration file.
  1. Make sure the configuration file is correct using this command:

      ```bash
      terraform validate
      ```
     
      If the configuration is correct, you will get this message:
     
      ```bash
      Success! The configuration is valid.
      ```

  1. Run this command:

      ```bash
      terraform plan
      ```
  
      The terminal will display a list of resources with parameters. No changes will be made at this step. If the configuration contains any errors, {{ TF }} will point them out.
  1. Apply the configuration changes:

      ```bash
      terraform apply
      ```
     
  1. Confirm the changes: type `yes` into the terminal and press **Enter**.

      You can verify device passwords in the [management console]({{ link-console-main }}) or using the following [CLI](../../../cli/quickstart.md) command:

      ```bash
      yc iot device password list --device-name <device_name>
      ```

- API {#api}

  To delete a device password, use the [deletePassword](../../api-ref/Device/deletePassword.md) REST API method for the [Device](../../api-ref/Device/index.md) resource or the [DeviceService/DeletePassword](../../api-ref/grpc/Device/deletePassword.md) gRPC API call.

{% endlist %}