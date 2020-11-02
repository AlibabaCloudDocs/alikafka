---
keyword: [kafka, client, send messages]
---

# How can I determine whether a message is sent on the client?

If the callback is successful, the message is sent.

Most clients return Callback or Future after a message is sent. If the callback is successful, the message is sent.

You can also check whether the message is properly sent in the console in the following ways:

-   View the partition status of a topic. The number of messages in each partition is displayed in real time.
-   View the traffic monitoring of a topic. The minute-level traffic curve is displayed.

