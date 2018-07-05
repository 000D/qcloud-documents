## KTE Security Group Configuration
Security has always been our major concern. By listing security as the top priority of the product design, Tencent Cloud requires its products fully isolated and provides multiple security protections with its basic network. KTE is a typical example. It adopts VPC as the underlying network of KTE in Phase I. This document mainly introduces the best practices of the security group under the KTE to help you select the security group policy.

### What Is the Security Group?
Security group is a virtual firewall with a stateful packet filtering feature and critical network security isolation means to set network access control for one or more CVM(s). For more information on security groups, please see [Security Details](https://cloud.tencent.com/document/product/213/5221).

### Principles on Security Group Selection with KTE

 1. It is recommended that CVMs in the same cluster are bound to the same security group. The cluster's security group does not add other CVMs.
 2. Security groups grant the minimum permission externally.
 3. The following KTE rules need to be open to the Internet:
  - Container network and node network
  - Container network and node network of the cluster if different clusters in the same VPC cannot be connected
  - Port 22 in SSH login node
  - Port 30000-32767 for KTE access

### Suggestions
It is recommended that you configure the security groups for the cluster through the security group template provided by the KTE.
