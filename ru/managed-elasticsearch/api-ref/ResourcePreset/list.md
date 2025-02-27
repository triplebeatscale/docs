---
editable: false
sourcePath: en/_api-ref/mdb/elasticsearch/v1/api-ref/ResourcePreset/list.md
---

# Managed Service for Elasticsearch API, REST: ResourcePreset.List

Retrieves the list of available resource presets.

## HTTP request

```
GET https://{{ api-host-mdb }}/managed-elasticsearch/v1/resourcePresets
```

## Query parameters {#yandex.cloud.mdb.elasticsearch.v1.ListResourcePresetsRequest}

#|
||Field | Description ||
|| pageSize | **string** (int64)

The maximum number of results per page to return.

If the number of available results is larger than `page_size`, the service returns a [ListResourcePresetsResponse.nextPageToken](#yandex.cloud.mdb.elasticsearch.v1.ListResourcePresetsResponse) that can be used to get the next page of results in subsequent list requests. ||
|| pageToken | **string**

Page token.

To get the next page of results, set `page_token` to the [ListResourcePresetsResponse.nextPageToken](#yandex.cloud.mdb.elasticsearch.v1.ListResourcePresetsResponse) returned by the previous list request. ||
|#

## Response {#yandex.cloud.mdb.elasticsearch.v1.ListResourcePresetsResponse}

**HTTP Code: 200 - OK**

```json
{
  "resourcePresets": [
    {
      "id": "string",
      "zoneIds": [
        "string"
      ],
      "cores": "string",
      "memory": "string"
    }
  ],
  "nextPageToken": "string"
}
```

#|
||Field | Description ||
|| resourcePresets[] | **[ResourcePreset](#yandex.cloud.mdb.elasticsearch.v1.ResourcePreset)**

List of resource presets. ||
|| nextPageToken | **string**

This token allows you to get the next page of results for list requests.

If the number of results is larger than [ListResourcePresetsRequest.pageSize](#yandex.cloud.mdb.elasticsearch.v1.ListResourcePresetsRequest), use the `next_page_token` as the value for the [ListResourcePresetsRequest.pageToken](#yandex.cloud.mdb.elasticsearch.v1.ListResourcePresetsRequest) parameter in the next list request.
Each subsequent list request will have its own `next_page_token` to continue paging through the results. ||
|#

## ResourcePreset {#yandex.cloud.mdb.elasticsearch.v1.ResourcePreset}

A ResourcePreset resource for describing hardware configuration presets.

#|
||Field | Description ||
|| id | **string**

ID of the resource preset. ||
|| zoneIds[] | **string**

IDs of availability zones where the resource preset is available. ||
|| cores | **string** (int64)

Number of CPU cores for an Elasticsearch node created with the preset. ||
|| memory | **string** (int64)

RAM volume for an Elasticsearch node created with the preset, in bytes. ||
|#