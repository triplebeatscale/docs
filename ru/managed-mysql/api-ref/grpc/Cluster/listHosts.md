---
editable: false
sourcePath: en/_api-ref-grpc/mdb/mysql/v1/api-ref/grpc/Cluster/listHosts.md
---

# Managed Service for MySQL API, gRPC: ClusterService.ListHosts

Retrieves a list of hosts for a cluster.

## gRPC request

**rpc ListHosts ([ListClusterHostsRequest](#yandex.cloud.mdb.mysql.v1.ListClusterHostsRequest)) returns ([ListClusterHostsResponse](#yandex.cloud.mdb.mysql.v1.ListClusterHostsResponse))**

## ListClusterHostsRequest {#yandex.cloud.mdb.mysql.v1.ListClusterHostsRequest}

```json
{
  "cluster_id": "string",
  "page_size": "int64",
  "page_token": "string"
}
```

#|
||Field | Description ||
|| cluster_id | **string**

Required field. ID of the cluster to list hosts for.

To get this ID, make a [ClusterService.List](/docs/managed-mysql/api-ref/grpc/Cluster/list#List) request. ||
|| page_size | **int64**

The maximum number of results per page to return.

If the number of available results is larger than `page_size`, the API returns a [ListClusterHostsResponse.next_page_token](#yandex.cloud.mdb.mysql.v1.ListClusterHostsResponse) that can be used to get the next page of results in the subsequent [ClusterService.ListHosts](#ListHosts) requests. ||
|| page_token | **string**

Page token that can be used to iterate through multiple pages of results.

To get the next page of results, set `page_token` to the [ListClusterHostsResponse.next_page_token](#yandex.cloud.mdb.mysql.v1.ListClusterHostsResponse) returned by the previous [ClusterService.ListHosts](#ListHosts) request. ||
|#

## ListClusterHostsResponse {#yandex.cloud.mdb.mysql.v1.ListClusterHostsResponse}

```json
{
  "hosts": [
    {
      "name": "string",
      "cluster_id": "string",
      "zone_id": "string",
      "resources": {
        "resource_preset_id": "string",
        "disk_size": "int64",
        "disk_type_id": "string"
      },
      "role": "Role",
      "health": "Health",
      "services": [
        {
          "type": "Type",
          "health": "Health"
        }
      ],
      "subnet_id": "string",
      "assign_public_ip": "bool",
      "replication_source": "string",
      "backup_priority": "int64",
      "priority": "int64"
    }
  ],
  "next_page_token": "string"
}
```

#|
||Field | Description ||
|| hosts[] | **[Host](#yandex.cloud.mdb.mysql.v1.Host)**

List of hosts in the cluster. ||
|| next_page_token | **string**

The token that can be used to get the next page of results.

If the number of results is larger than [ListClusterHostsRequest.page_size](#yandex.cloud.mdb.mysql.v1.ListClusterHostsRequest), use the `next_page_token` as the value for the [ListClusterHostsRequest.page_token](#yandex.cloud.mdb.mysql.v1.ListClusterHostsRequest) in the subsequent [ClusterService.ListHosts](#ListHosts) request to iterate through multiple pages of results.

Each of the subsequent [ClusterService.ListHosts](#ListHosts) requests should use the `next_page_token` value returned by the previous request to continue paging through the results. ||
|#

## Host {#yandex.cloud.mdb.mysql.v1.Host}

#|
||Field | Description ||
|| name | **string**

Name of the host.

This name is assigned by the platform at the time of creation.
The name is unique across all MDB hosts that exist on the platform, as it defines the FQDN of the host. ||
|| cluster_id | **string**

ID of the cluster the host belongs to. ||
|| zone_id | **string**

ID of the availability zone where the host resides. ||
|| resources | **[Resources](#yandex.cloud.mdb.mysql.v1.Resources)**

Resources allocated to the host. ||
|| role | enum **Role**

Role of the host in the cluster. If the field has default value, it is not returned in the response.

- `ROLE_UNKNOWN`: Role of the host is unknown. Default value.
- `MASTER`: Host is the master.
- `REPLICA`: Host is a replica. ||
|| health | enum **Health**

Aggregated health of the host. If the field has default value, it is not returned in the response.

- `HEALTH_UNKNOWN`: Health of the host is unknown. Default value.
- `ALIVE`: Host is performing all its functions normally.
- `DEAD`: Host is inoperable, and cannot perform any of its essential functions.
- `DEGRADED`: Host is degraded, and can perform only some of its essential functions.
- `READONLY`: Host is alive, but in read-only mode. ||
|| services[] | **[Service](#yandex.cloud.mdb.mysql.v1.Service)**

List of services provided by the host. ||
|| subnet_id | **string**

ID of the subnet that the host belongs to. ||
|| assign_public_ip | **bool**

Flag that shows if public IP address is assigned to the host so that the host can be accessed from the internet. ||
|| replication_source | **string**

Name of the host to be used as the replication source for cascading replication. ||
|| backup_priority | **int64**

Host backup priority. ||
|| priority | **int64**

Host master promotion priority. ||
|#

## Resources {#yandex.cloud.mdb.mysql.v1.Resources}

Cluster resource preset.

#|
||Field | Description ||
|| resource_preset_id | **string**

ID of the resource preset that defines available computational resources (vCPU, RAM, etc.) for a cluster host.

All available presets are listed in [the documentation](/docs/managed-mysql/concepts/instance-types). ||
|| disk_size | **int64**

Volume of the storage (for each cluster host, in bytes). ||
|| disk_type_id | **string**

Type of the storage.

Possible values:
* `network-hdd` - standard network storage
* `network-ssd` - fast network storage
* `network-ssd-nonreplicated` - fast network nonreplicated storage
* `local-ssd` - fast local storage.

See [the documentation](/docs/managed-mysql/concepts/storage) for details. ||
|#

## Service {#yandex.cloud.mdb.mysql.v1.Service}

#|
||Field | Description ||
|| type | enum **Type**

Type of the service provided by the host. If the field has default value, it is not returned in the response.

- `TYPE_UNSPECIFIED`: Service type of the host is unspecified. Default value.
- `MYSQL`: The host is a MySQL server. ||
|| health | enum **Health**

Aggregated health of the service. If the field has default value, it is not returned in the response.

- `HEALTH_UNKNOWN`: Health of the service is unknown. Default value.
- `ALIVE`: The service is working normally.
- `DEAD`: The service is dead or unresponsive.
- `READONLY`: The service is in read-only mode. ||
|#