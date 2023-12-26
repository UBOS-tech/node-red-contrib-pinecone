## node-red-contrib-pinecone

Pinecone makes it easy to provide long-term memory for high-performance AI applications

<img width="100%"  alt="Ubos - End-to-End Software Development Platform" src="https://ubos.tech/wp-content/uploads/2023/03/cropped-Group-21015-1.png">

<h3 align="center">
  <b><a href="https://ubos.tech/">UBOS</a></b>
  •
  <a href="https://community.ubos.tech/">Community</a>
  •
  <a href="https://www.youtube.com/@ubos_tech">Youtube</a>
  •
  <a href="https://discord.com/invite/dt59QaptH2">Discord</a>
  •
  <a href="https://github.com/UBOS-tech">GitHub</a>
</h3>

<div align="center">
 
[![flow](https://img.shields.io/badge/platform-Node--RED-red)](https://flows.nodered.org/node/node-red-contrib-pinecone)
[![npm](https://img.shields.io/npm/v/node-red-contrib-pinecone)](https://www.npmjs.com/package/node-red-contrib-pinecone)
 
</div>

### Quick Start

Install with the built in <b>Node-RED Palette manager</b> or using npm:
```
npm install node-red-contrib-pinecone
```

## Properties Index Operations

```js
msg.PINECONE_API_KEY = "Your API key";
msg.PINECONE_ENVIRONMENT = "Your environment";
msg.PINECONE_BASE_URL = "https://controller.[Your environment].pinecone.io";
```

1. When `msg.actionType` is set to `list_collections`: This operation returns a list of your Pinecone collections

2. When `msg.actionType` is set to `create_collection`:
    - **[Required]** `name`: [Type: string] The name of the collection to be created
    
    - **[Required]** `source`: [Type: string] The name of the source index to be used as the source for the collection

3. When `msg.actionType` is set to `describe_collection`:
    - **[Required]** `name`: [Type: string] The name of the collection

4. When `msg.actionType` is set to `delete_collection`: 
    - **[Required]** `name`: [Type: string] The name of the collection

5. When `msg.actionType` is set to `list_collections`: This operation returns a list of your Pinecone indexes

6. When `msg.actionType` is set to `create_index`:
    - **[Required]** `name`: [Type: string] The name of the collection to be created

    - **[Required]** `dimension`: [Type: integer] The dimensions of the vectors to be inserted in the index
    - `metric`: [Type: string] The distance metric to be used for similarity search. You can use 'euclidean', 'cosine', or 'dotproduct'
    - `pods`: [Type: integer] The number of pods for the index to use,including replicas
    - `replicas`: [Type: integer] The number of replicas. Replicas duplicate your index. They provide higher availability and throughput
    - `pod_type`: [Type: string] The type of pod to use. One of **s1**, **p1**, or **p2** appended with **.** and one of **x1**, **x2**, **x4**, or **x8**.
    - `metadata_config`: [Type: object | null] Configuration for the behavior of Pinecone's internal metadata index. By default, all metadata is indexed; when metadata_config is present, only specified metadata fields are indexed. To specify metadata fields to index, provide a JSON object of the following form:
    ```json
    {"indexed": ["example_metadata_field"]}
    ```
    - `source_collection`: [Type: string] The name of the collection to create an index from

Example:
```js
    msg.payload = {
        name: "pinecone-index",
        metric: "cosine",
        pods: 1,
        replicas: 1,
        pod_type: "p1.x1"
    }
```

7. When `msg.actionType` is set to `describe_index`:
    - **[Required]** `name`: [Type: string] The name of the collection

8. When `msg.actionType` is set to `delete_index`:
    - **[Required]** `name`: [Type: string] The name of the collection

9. When `msg.actionType` is set to `configure_index`:
    - **[Required]** `name`: [Type: string] The name of the collection

    - `replicas`: [Type: string] The desired number of replicas for the index
    - `pod_type`: [Type: string] The new pod type for the index. One of **s1**, **p1**, or **p2** appended with **.** and one of **x1**, **x2**, **x4**, or **x8**.

## Properties Vector operations

```js
msg.PINECONE_API_KEY = "Your API key";
msg.PINECONE_ENVIRONMENT = "Your environment";
msg.PINECONE_BASE_URL = "https://index_name-project_id.svc.environment.pinecone.io";
```

1. When `msg.actionType` is set to `fetch`:
    - **[Required]** `msg.name`: The name of the index

    - **[Required]** `msg.vectors`: The vector IDs to fetch. Does not accept values containing spaces. For example:
   ```js
    msg.payload = {
        name: "pinecone-index",
        vectors: ["vec1", "vec2"]
    };
   ```

2. When `msg.actionType` is set to `query`:
    - **[Required]** `name`: [Type: string] The name of the index

    - **[Required]** `topK`: [Type: int64] The number of results to return for each query
    - `filter`: [Type: object] The filter to apply. You can use vector metadata to limit your search. See [docs](https://www.pinecone.io/docs/metadata-filtering)
    - `includeValues`: [Type: boolean] Indicates whether vector values are included in the response
    - `includeMetadata`: [Type: boolean] Indicates whether metadata is included in the response as well as the ids
    - `vector`: [Type: array of floats] The query vector. This should be the same length as the dimension of the index being queried. Each query() request can contain only one of the parameters id or vector.
    - `sparseVector`: [Type: object] Vector sparse data. Represented as a list of indices and a list of corresponded values, which must be the same length
    - `id`: [Type: string] The unique ID of the vector to be used as a query vector. Each query() request can contain only one of the parameters queries, vector, or id
   
   Example:
   ```js
    msg.payload = {
        name: "pinecone-index",
        topK: 10,
        vector: [0.1,0.2,0.3],
        namespace: 'example-namespace'
    };
   ```

3. When `msg.actionType` is set to `update`:
    - **[Required]** `name`: [Type: string] The name of the index

    - **[Required]** `id`: [Type: string] Vector's unique id
    - `values`: [Type: array of floats] Vector data
    - `sparseValues`: [Type: object] Vector sparse data. Represented as a list of indices and a list of corresponded values, which must be the same length
    - `setMetadata`: [Type: object] Metadata to set for the vector
    - `namespace`: [Type: string] The namespace containing the vector to update
   
   Example:
   ```js
    msg.payload = {
        name: "pinecone-index",
        id:'vec1',
        values: [0.1,0.2,0.3,0.4],
        setMetadata: {'genre': 'drama'},
        namespace: 'example-namespace'
    };
   ```

4. When `msg.actionType` is set to `upsert`:
    - **[Required]** `name`: [Type: string] The name of the index

    - **[Required]** `vectors`: [Type: array of objects] An array containing the vectors to upsert. Recommended batch limit is 100 vectors
    - `namespace`: [Type: string] This is the namespace name where you upsert vectors
   
   Example:
   ```js
    msg.payload = {
        name: "pinecone-index",
        vectors: [
            {
                id: 'vec2',
                values: [0.2,0.3,0.4,0.5],
                metadata: {'genre': 'action'}
            }
        ]
    };
   ```

5. When `msg.delete` is set to `upsert`:
    - **[Required]** `name`: [Type: string] The name of the index

    - **[Required]** `ids`: [Type: array of strings] Vectors to delete
    - `deleteAll`: [Type: boolean] This indicates that all vectors in the index namespace should be deleted
    - `namespace`: [Type: string] The namespace to delete vectors from, if applicable
    - `filter`: [Type: boolean] If specified, the metadata filter here will be used to select the vectors to delete. This is mutually exclusive with specifying IDs to delete in the ids param or using delete_all=True. See [doc](https://www.pinecone.io/docs/metadata-filtering)
   
   Example:
   ```js
    msg.payload = {
        name: "pinecone-index",
        ids: ["vec1", "vec2"]
    };
   ```

6. When `msg.delete` is set to `describeIndexStats`:
    - **[Required]** `name`: [Type: string] The name of the index

    - `filter`: [Type: boolean] If this parameter is present, the operation only returns statistics for vectors that satisfy the filter. See [doc](https://www.pinecone.io/docs/metadata-filtering)

### Bugs reports and feature requests

Please report any issues or feature requests at <a href="https://github.com/UBOS-tech/node-red-contrib-pinecone/issues">GitHub</a>.

## License

[MIT License](https://github.com/UBOS-tech/node-red-contrib-pinecone/blob/main/LICENSE)
