---
editable: false
---

# yc iot registry

Manage IoT registries

#### Command Usage

Syntax: 

`yc iot registry <group|command>`

Aliases: 

- `registries`

#### Command Tree

- [yc iot registry add-labels](add-labels.md) — Add labels to specified registry
- [yc iot registry create](create.md) — Create new device registry
- [yc iot registry delete](delete.md) — Delete specified registry
- [yc iot registry disable](disable.md) — Disable this registry
- [yc iot registry enable](enable.md) — Enable this registry
- [yc iot registry get](get.md) — Show information about specified registry
- [yc iot registry list](list.md) — List IoT registries
- [yc iot registry list-device-topic-aliases](list-device-topic-aliases.md) — List all topic aliases set for devices in this registry
- [yc iot registry logs](logs.md) — Show logs for the specified registry
- [yc iot registry remove-labels](remove-labels.md) — Remove labels from specified registry
- [yc iot registry update](update.md) — Update specified registry
- [yc iot registry certificate](certificate/index.md) — Manage IoT device registry certificates
	- [yc iot registry certificate add](certificate/add.md) — Add new certificate to specified registry
	- [yc iot registry certificate delete](certificate/delete.md) — Delete specified certificate from registry
	- [yc iot registry certificate list](certificate/list.md) — List certificates associated with specified registry
- [yc iot registry password](password/index.md) — Manage IoT device registry passwords
	- [yc iot registry password add](password/add.md) — Add new password to specified registry
	- [yc iot registry password delete](password/delete.md) — Delete specified password from specified registry
	- [yc iot registry password list](password/list.md) — List passwords associated with specified registry
- [yc iot registry yds-export](yds-export/index.md) — Manage IoT device registry YDS exports
	- [yc iot registry yds-export add](yds-export/add.md) — Add new data stream export to specified registry
	- [yc iot registry yds-export delete](yds-export/delete.md) — Delete specified data stream export from specified registry
	- [yc iot registry yds-export list](yds-export/list.md) — List data stream exports associated with specified registry

#### Global Flags

| Flag | Description |
|----|----|
|`--profile`|<b>`string`</b><br/>Set the custom configuration file.|
|`--debug`|Debug logging.|
|`--debug-grpc`|Debug gRPC logging. Very verbose, used for debugging connection problems.|
|`--no-user-output`|Disable printing user intended output to stderr.|
|`--retry`|<b>`int`</b><br/>Enable gRPC retries. By default, retries are enabled with maximum 5 attempts.<br/>Pass 0 to disable retries. Pass any negative value for infinite retries.<br/>Even infinite retries are capped with 2 minutes timeout.|
|`--cloud-id`|<b>`string`</b><br/>Set the ID of the cloud to use.|
|`--folder-id`|<b>`string`</b><br/>Set the ID of the folder to use.|
|`--folder-name`|<b>`string`</b><br/>Set the name of the folder to use (will be resolved to id).|
|`--endpoint`|<b>`string`</b><br/>Set the Cloud API endpoint (host:port).|
|`--token`|<b>`string`</b><br/>Set the OAuth token to use.|
|`--impersonate-service-account-id`|<b>`string`</b><br/>Set the ID of the service account to impersonate.|
|`--no-browser`|Disable opening browser for authentication.|
|`--format`|<b>`string`</b><br/>Set the output format: text (default), yaml, json, json-rest.|
|`--jq`|<b>`string`</b><br/>Query to select values from the response using jq syntax|
|`-h`,`--help`|Display help for the command.|
