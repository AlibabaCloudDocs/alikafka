---
keyword: [kafka, endpoint, plaintext, sasl, ssl]
---

# Comparison among endpoints

This topic compares different types of Message Queue for Apache Kafka endpoints to help you choose an appropriate access method.

|Item|Default endpoint|Secure Sockets Layer \(SSL\) endpoint|Simple Authentication and Security Layer \(SASL\) endpoint|
|----|----------------|-------------------------------------|----------------------------------------------------------|
|Instance edition|-   [Standard Edition](/intl.en-US/Pricing/Billing.md)
-   [Professional Edition](/intl.en-US/Pricing/Billing.md)

|-   [Standard Edition](/intl.en-US/Pricing/Billing.md)
-   [Professional Edition](/intl.en-US/Pricing/Billing.md)

|[Professional Edition](/intl.en-US/Pricing/Billing.md)|
|Major version|0.10.x~2.x|0.10.x~2.x|2.x|
|Network type|VPC|Internet and VPC|-   VPC
-   Internet and VPC |
|Enabling method|Automatically enabled|Automatically enabled|[Enable in the console](/intl.en-US/Access control/Authorize SASL users.md)|
|Security protocol|PLAINTEXT|SASL\_SSL|SASL\_PLAINTEXT|
|Port|9092|9093|9094|

