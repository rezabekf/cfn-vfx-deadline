## Setup AWS Client VPN

#### Step 1: Generate Server and Client Certificates and Keys
This tutorial uses mutual authentication. With mutual authorization, Client VPN uses certificates to perform 
authentication between the client and the server.

For the steps to generate the server and client certificates and keys, 
see [Mutual Authentication](https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/authentication-authrization.html#mutual).

#### Step 2: Create a Client VPN Endpoint
When you create a Client VPN endpoint, you create the VPN construct to which clients can connect to establish a VPN connection.

**To create a Client VPN endpoint (console)**

1. Open the Amazon VPC console at <https://console.aws.amazon.com/vpc/>.
2. In the navigation pane, choose **Client VPN Endpoints** and choose **Create Client VPN Endpoint.**
3. (Optional) For **Description**, enter a brief description for the Client VPN endpoint.
4. For **Client IPv4 CIDR**, specify the IP address range, in CIDR notation, from which to assign client IP addresses.
    >**Note**
    The IP address range cannot overlap with the target network or any of the routes that will be associated with the 
    Client VPN endpoint. The client CIDR range must have a block size that is between /16 and /22 and not overlap with 
    VPC CIDR or any other route in the route table.
5. For **Server certificate ARN**, specify the ARN for the TLS certificate to be used by the server. Clients use the 
    server certificate to authenticate the Client VPN endpoint to which they are connecting.
    >**Note** 
    The server certificate must be provisioned in AWS Certificate Manager (ACM).  
6. Specify the authentication method to be used to authenticate clients when they establish a VPN connection. 
    To use mutual certificate authentication, select **Use mutual authentication**, and then for **Client certificate ARN**, 
    specify the ARN of the client certificate generated in Step 1.
7. Specify whether to log data about client connections using Amazon CloudWatch Logs. 
    For **Do you want to log the details on client connections?**, do one of the following:
    * To enable client connection logging, choose **Yes**, 
        for **CloudWatch Logs log group name** enter the name of the log group to use,
        and for **CloudWatch Logs log stream name**, enter the name of the log stream to use.
    * To disable client connection logging, choose **No**.
8. Specify which DNS servers to use for DNS resolution. For example for **DNS Server 1 IP address** use 1.1.1.1 and for 
    **DNS Server 2 IP address** 1.0.0.1 IP addresses.
9. By default, the Client VPN server uses the `UDP` transport protocol. 
    UDP typically offers better performance than TCP.
10. Choose **Create Client VPN Endpoint**.

#### Step 3: Enable VPN Connectivity for Clients

To enable clients to establish a VPN session, you must associate a target network with the Client VPN endpoint. 
A target network is a subnet in a VPC. 

**To associate a subnet with the Client VPN endpoint (console)**

1. Open the Amazon VPC console at <https://console.aws.amazon.com/vpc/>.
2. In the navigation pane, choose **Client VPN Endpoints**.
3. Select the Client VPN endpoint with which to associate the subnet and choose **Associations**, **Associate**.
4. For **VPC**, choose the VPC in which the subnet is provisioned.
    >**Note**
    For the first subnet you associate, choose `Management DMZ Subnet B`, this will allow clients to connect to the internet.    
5. For **Subnet to associate**, choose the subnet to associate with the Client VPN endpoint.
6. Choose **Associate**.
7. Repeat steps 3.-6. for the second subnet `Management App Subnet A`, this will allow clients to connect to Deadline workstation instance.

#### Step 4: Authorize Clients to Access a Network

To authorize clients to access the VPC in which the associated subnet is located, you must create an authorization rule.
The authorization rule specifies which clients have access to the VPC. In this tutorial, we grant access to all users.

**To add an authorization rule to the target network**

1. Open the Amazon VPC console at <https://console.aws.amazon.com/vpc/>.
2. In the navigation pane, choose **Client VPN Endpoints**.
3. Select the Client VPN endpoint to which to add the authorization rule, choose **Authorization**, and then choose **Authorize ingress**.
4. For **Destination network**, enter the IP address, in CIDR notation, of the network for which you want to allow access.
5. Specify which clients are allowed to access the specified network. To grant access to all users, for **For grant access to**, choose **Allow access to all users**.
6. For **Description**, enter a brief description of the authorization rule.
7. Choose **Add authorization rule**.

- next [Cleanup](../04-CleanUp/README.md)