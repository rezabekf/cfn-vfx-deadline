## Setup AWS Client VPN

#### Step 1: Generate Server and Client Certificates and Keys
This tutorial uses mutual authentication. With mutual authorization, Client VPN uses certificates to perform 
authentication between the client and the server.

For the detailed steps to generate the server and client certificates and keys, go to
[Mutual Authentication](https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/authentication-authrization.html#mutual).

Below is a summary of commands:
```bash
# 1. Clone the OpenVPN easy-rsa repo to your local computer.
git clone https://github.com/OpenVPN/easy-rsa.git

# 2. Navigate into the easy-rsa/easyrsa3 folder in your local repo.
cd easy-rsa/easyrsa3

# 3. Initialize a new PKI environment.
./easyrsa init-pki

# 4. Build a new certificate authority (CA).
./easyrsa build-ca nopass

# 5. Generate the server certificate and key.
./easyrsa build-server-full server nopass

# 6. Generate the client certificate and key.
./easyrsa build-client-full client1.domain.tld nopass

# 7. Copy the server certificate and key and the client certificate and key to a custom folder and then navigate into the custom folder.
cp pki/ca.crt /custom_folder/
cp pki/issued/server.crt /custom_folder/
cp pki/private/server.key /custom_folder/
cp pki/issued/client1.domain.tld.crt custom_folder
cp pki/private/client1.domain.tld.key /custom_folder/
cd /custom_folder/

# 8. Upload the server certificate and key to ACM.
aws acm import-certificate --certificate file://server.crt --private-key file://server.key --certificate-chain file://ca.crt --region region

# 9. Upload the client certificate and key to ACM.
aws acm import-certificate --certificate file://client1.domain.tld.crt --private-key file://client1.domain.tld.key --certificate-chain file://ca.crt --region region
```

#### Step 2: Create a Client VPN Endpoint
When you create a Client VPN endpoint, you create the VPN construct to which clients can connect to establish a VPN connection.

**To create a Client VPN endpoint (console)**

1. Open the Amazon VPC console at <https://console.aws.amazon.com/vpc/>.
2. In the navigation pane, choose **Client VPN Endpoints** and choose **Create Client VPN Endpoint.**
3. (Optional) For **Description**, enter a brief description for the Client VPN endpoint.
4. For **Client IPv4 CIDR**, specify the IP address range, in CIDR notation, from which to assign client IP addresses.
    For example `172.239.0.0/22`
    
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
4. For **VPC**, choose the Management VPC.
    
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
4. For **Destination network**, enter the 0.0.0.0/0 IP address.
5. Specify which clients are allowed to access the specified network. To grant access to all users, for **For grant access to**, choose **Allow access to all users**.
6. For **Description**, enter a brief description of the authorization rule (VPN clients with Internet Access).
7. Choose **Add authorization rule**.

#### Step 5: Enable Access to Additional Networks

You can enable access to additional networks connected to the VPC, such as AWS services, peered VPCs, and on-premises networks. For each additional network, you must add a route to the network and configure an authorization rule to give clients access.
In this tutorial, we add a route to the internet.

**To enable access to additional networks (console)**

1. Open the Amazon VPC console at <https://console.aws.amazon.com/vpc/>.
2. In the navigation pane, choose **Client VPN Endpoints**.
3. Select the Client VPN endpoint to which to add the route, choose **Route Table**, and then choose **Create Route**.
4. For **Route destination**, enter `0.0.0.0/0`.
5. For **Target VPC Subnet ID**, specify the ID of the `Management DMZ subnet` through which to route traffic.
6. For **Description**, enter a brief description for the route.
7. Choose **Create Route**.

#### Step 6: Whitelist RDP in Deadline workstation security group
To allow RDP connection to workstation instance, you must whitelist Client VPN endpoint security group in workstation security group.

1. Open the Amazon VPC console at <https://console.aws.amazon.com/vpc/>.
2. In the navigation pane, choose **Client VPN Endpoints**.
3. Select the Client VPN endpoint from which to copy security group ID, choose **Security Groups**, and then copy **sg-xxxxxxxxx** ID.

5. In the navigation pane, choose **Security Groups**.
6. Select the `vfx-deadline-workstation-sg` security group to which to add rule, choose **Inbound Rules**, and then choose **Edit rules**, **Add Rule**.
7. For **Type**, select `RDP`.
8. For **Source**, select `Custom` and paste in Client VPN security group ID.
9. Choose **Save rules**. 

#### Step 7: Download the Client VPN Endpoint Configuration File

In this step we download and prepare the Client VPN endpoint configuration file. 
The configuration file includes the Client VPN endpoint and certificate information required to establish a VPN connection. 
You must provide this file to the clients who need to connect to the Client VPN endpoint to establish a VPN connection. 
The client uploads this file into their VPN client application.

**To download and prepare the Client VPN endpoint configuration file (AWS CLI)**

1. Download the Client VPN endpoint configuration file.
    ```bash
    aws ec2 export-client-vpn-client-configuration --client-vpn-endpoint-id `endpoint_id` --output text>client-config.ovpn
    ```  
2. Copy the client certificate and key, which were generated in Step 1, to the same folder as the downloaded 
Client VPN endpoint configuration file. The client certificate and key can be found in the following locations in 
the cloned OpenVPN easy-rsa repo:
    * Client certificate — `easy-rsa/easyrsa3/pki/issued/client1.domain.tld.crt`
    * Client key — `easy-rsa/easyrsa3/pki/private/client1.domain.tld.key`

3. Append the client certificate and key to the Client VPN endpoint configuration file.
    ```bash
    cat >> client-config.ovpn
    cert /`path`/client1.domain.tld.crt
    key /`path`/client1.domain.tld.key
    CTRL+C
    ```
#### Step 8: Configure OpenVPN Client
Now that we have our infrastructure provisioned and configured, the last step is to install VPN client on your machine. 
The following procedure shows how to establish a VPN connection using the Tunnelblick client application on a macOS computer.

```bash
brew search tunnelblick
brew cask install tunnelblick
```

Once the client is installed, please follow the 
[AWS Connect to VPN documentation](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/macos.html) 

In the last Section of this guide we will [Cleanup](../04-CleanUp/README.md) all the resources created.