## node-red-contrib-pinecone



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

### Quick Start

Install with the built in <b>Node-RED Palette manager</b> or using npm:
```
npm install node-red-contrib-pinecone
```

## Properties Index Operations

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

