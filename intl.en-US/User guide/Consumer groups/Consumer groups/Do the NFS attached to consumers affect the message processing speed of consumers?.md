---
keyword: [kafka, nfs, client, consumption]
---

# Do the NFS attached to consumers affect the message processing speed of consumers?

If the consumption speed is affected by the data storage at the consumer client, the data storage is processed synchronously in the main thread that process messages. This blocks message pulling and processing.

We recommend that you use independent and separate threads to process messages and store processing results. Messages are pulled and then consumed, even after they are cached. This ensures fast consumption.

A network file system \(NFS\) affects performance for the following two reasons:

-   The NFS does not run fast enough.

-   The NFS uses shared network storage, which is accessed by multiple nodes and processes. This reduces efficiency and this is why performance is degraded when the number of consumers increases. To solve this problem, you can attach a cloud disk to each consumer to store processing results independently. This prevents performance degradation even when more consumers contend for NFS resources.


Each attached cloud disk stores data independently. To store processing results in the same NFS, you can use an asynchronous tool or thread to forward the processing results stored in cloud disks to the NFS. This prevents message processing from being blocked by synchronous storage in the NFS.

Asynchronous processing is an effective way to avoid drops in efficiency due to resource access issues.

