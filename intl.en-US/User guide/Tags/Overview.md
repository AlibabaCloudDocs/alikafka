---
keyword: [Kafka, Tag]
---

# Overview

Tags can identify resources. You can use tags to classify Message Queue for Apache Kafka resources for easy resource search and aggregation. Message Queue for Apache Kafka allows you to bind or unbind tags to or from instances, topics, and consumer groups.

## Scenarios

You can use tags to group Message Queue for Apache Kafka resources you created for easy retrieval and batch operations.

## Instructions

-   Each tag consists of a key-value pair.
-   A tag must have a unique tag key.

    For example, an ECS instance is bound to the `city:shanghai` tag. If the instance is subsequently bound to the `city:newyork` tag, the `city:shanghai` tag is automatically unbound from the instance.

-   Tag information is not shared across regions. For example, tags created in the China \(Hangzhou\) region are not visible to the China \(Shanghai\) region.
-   Tags are deleted when they are not bound to any resources.
-   For more information about the best practices for tag design, see [Best practices for tag design](/intl.en-US/Best Practices/Best practices for tag design.md).

## Limits

-   Up to 20 tags can be bound to a resource.
-   A tag can be bound to a maximum of 50 resources.
-   A maximum of 20 tags can be bound or unbound at a time.

## References

-   [Bind a tag](/intl.en-US/User guide/Tags/Bind a tag.md)
-   [Edit a tag](/intl.en-US/User guide/Tags/Edit a tag.md)
-   [Unbind a tag](/intl.en-US/User guide/Tags/Unbind a tag.md)
-   [Use tags to retrieve resources](/intl.en-US/User guide/Tags/Use tags to retrieve resources.md)

