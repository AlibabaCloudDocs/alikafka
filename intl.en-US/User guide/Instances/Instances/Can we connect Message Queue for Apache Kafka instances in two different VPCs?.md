# Can we connect Message Queue for Apache Kafka instances in two different VPCs?

Yes. You can use Cloud Enterprise Network \(CEN\) or Virtual Private Network \(VPN\) Gateway to connect Message Queue for Apache Kafka instances in two different virtual private clouds \(VPCs\).

## CEN

CEN allows you to establish private channels between VPCs. CEN uses automatic route distribution and learning. Therefore, it can speed up network convergence, improve quality and security in cross-network communication, and implement connection among network-wide resources. For more information, see [What is Cloud Enterprise Network?]()

You can use CEN to connect two VPCs under the same account or different accounts. The following table describes the scenarios.

|Scenario|Configuration method|
|--------|--------------------|
|Same-account VPC peering|[Connect two VPCs in the same region under the same account]()|
|[Connect two VPCs in different regions under the same account]()|
|Cross-account VPC peering|[Connect two VPCs in the same region under different accounts]()|
|[Connect two VPCs in different regions under different accounts]()|

CEN has the following benefits:

-   Worldwide connection

    CEN is an enterprise-class network that can connect Alibaba Cloud network resources around the world. CEN can also connect local data centers that are already connected to the Alibaba Cloud network. CEN validates the IP address ranges of the connected networks and ensures that the IP address ranges are not in conflict. In addition, CEN automatically forwards and learns multi-node routes through controllers to rapidly converge global routes.

-   Low latency and high speed

    CEN provides low-latency and high-speed network transmission. The maximum access rate between local networks can reach the port forwarding rate of the gateway device. In global network communication, the latency of CEN is much shorter than that of the Internet.

-   Nearest access and shortest path

    CEN deploys multiple access points and forwarding points in more than 60 regions around the world to support access to the nearest nodes of Alibaba Cloud. It enables traffic to transmit over a responsive and latency-free network.

-   Link redundancy and disaster recovery

    CEN provides at least four redundant links between any two access points. Therefore, it features high availability and network redundancy. If a link fails, CEN ensures your services to run normally without network jitters or interruption.

-   Systematic management

    CEN has systematic network monitoring capabilities that automatically detect route conflicts caused by system changes. Therefore, it can ensure the network stability.


## VPN Gateway

VPN Gateway is an Internet-based networking service that supports route-based IPsec-VPN connections. You can use IPsec-VPN connections to connect VPCs securely and reliably. For more information, see [Establish IPsec-VPN connections between two VPCs](/intl.en-US/User Guide/Configure IPsec-VPN connections/Establish IPsec-VPN connections between two VPCs.md).

VPN Gateway offers the following benefits:

-   High security

    You can use the IKE and IPsec protocols to encrypt data to ensure secure and reliable data transmission.

-   High availability

    VPN Gateway uses a hot-standby architecture and supports failover within seconds to ensure session persistence and zero service downtime.

-   Low cost

    The encrypted Internet-based channels established by VPN Gateway are more cost-effective than leased lines.

-   Easy configuration

    VPN Gateway is a ready-to-use service that requires no additional configuration.


