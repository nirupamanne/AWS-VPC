
# AWS - VPC, Subnets, Route Tables and NAT Gateway

## Objective
 I learned how to create an AWS Virtual Private Cloud (VPC), configure an Internet Gateway, create public, private and database subnets, configure route tables, and create a NAT Gateway.

The project uses the following network architecture for the Roboshop application.

---

# Step 1: Create a VPC

First, create a VPC in the AWS Console.

### Configuration

| Field | Value |
|---|---|
| Name | roboshop-dev |
| IPv4 CIDR | 10.0.0.0/16 |
| Region | US East (N. Virginia) |
| Region code | us-east-1 |

### Procedure

1. Log in to the AWS Management Console.
2. Search for VPC.
3. Open the VPC Dashboard.
4. Select Your VPCs.
5. Click Create VPC.
6. Enter the VPC name as `roboshop-dev`.
7. Select IPv4 CIDR manually.
8. Enter `10.0.0.0/16`.
9. Click Create VPC.

**Explanation:**

A VPC is a logically isolated network in AWS where we launch and manage resources such as EC2 instances, databases and other services.

The CIDR `10.0.0.0/16` defines the IP address range available within this VPC.

<img width="958" height="527" alt="image" src="https://github.com/user-attachments/assets/afddb2a9-6c1d-47e0-9a9f-9d1e5890a22a" />
---

# Step 2: Create and Attach an Internet Gateway

### Configuration

| Field | Value |
|---|---|
| Name | roboshop-dev |

### Procedure

1. Open VPC Dashboard.
2. Select Internet Gateways.
3. Click Create Internet Gateway.
4. Enter the name `roboshop-dev`.
5. Click Create Internet Gateway.
6. Select the newly created Internet Gateway.
7. Click Actions.
8. Select Attach to VPC.
9. Choose the VPC `roboshop-dev`.
10. Click Attach Internet Gateway.

**Explanation:**

An Internet Gateway allows resources in a VPC to communicate with the internet when their routing and security configurations permit it.

<img width="949" height="530" alt="image" src="https://github.com/user-attachments/assets/8f3a2000-efb7-4d9d-8b4c-acda3fa817b7" />

---

# Step 3: Create Six Subnets

We need six subnets in the Roboshop VPC.

They are divided into three categories:

1. Public Subnets
2. Private Subnets
3. Database Subnets

VPC CIDR for all subnets:

`10.0.0.0/16`

## 3.1 Public Subnets

### Subnet 1 – Public (Availability Zone 1a)

| Field | Value |
|---|---|
| Name | roboshop-public-us-east-1a |
| Availability Zone | us-east-1a |
| VPC | roboshop-dev |
| VPC CIDR | 10.0.0.0/16 |
| Subnet CIDR | 10.0.1.0/24 |

### Subnet 2 – Public (Availability Zone 1b)

| Field | Value |
|---|---|
| Name | roboshop-public-us-east-1b |
| Availability Zone | us-east-1b |
| VPC | roboshop-dev |
| VPC CIDR | 10.0.0.0/16 |
| Subnet CIDR | 10.0.2.0/24 |

## 3.2 Private Subnets

### Subnet 3 – Private (Availability Zone 1a)

| Field | Value |
|---|---|
| Name | roboshop-private-1a |
| Availability Zone | us-east-1a |
| VPC | roboshop-dev |
| Subnet CIDR | 10.0.11.0/24 |

### Subnet 4 – Private (Availability Zone 1b)

| Field | Value |
|---|---|
| Name | roboshop-private-1b |
| Availability Zone | us-east-1b |
| VPC | roboshop-dev |
| Subnet CIDR | 10.0.12.0/24 |

## 3.3 Database Subnets

### Subnet 5 – Database (Availability Zone 1a)

| Field | Value |
|---|---|
| Name | roboshop-database-us-east-1a |
| Availability Zone | us-east-1a |
| VPC | roboshop-dev |
| Subnet CIDR | 10.0.21.0/24 |

### Subnet 6 – Database (Availability Zone 1b)

| Field | Value |
|---|---|
| Name | roboshop-database-us-east-1b |
| Availability Zone | us-east-1b |
| VPC | roboshop-dev |
| Subnet CIDR | 10.0.22.0/24 |

### Subnet Summary

| Subnet | Type | Availability Zone | CIDR |
|---|---|---|---|
| Subnet 1 | Public | us-east-1a | 10.0.0.0/24 |
| Subnet 2 | Public | us-east-1b | 10.0.1.0/24 |
| Subnet 3 | Private | us-east-1a | 10.0.10.0/24 |
| Subnet 4 | Private | us-east-1b | 10.0.11.0/24 |
| Subnet 5 | Database | us-east-1a | 10.0.20.0/24 |
| Subnet 6 | Database | us-east-1b | 10.0.21.0/24 |

### Procedure to create subnets

1. Open VPC Dashboard.
2. Select Subnets.
3. Click Create subnet.
4. Choose the VPC `roboshop-dev`.
5. Enter the subnet name.
6. Select the Availability Zone.
7. Enter the IPv4 subnet CIDR.
8. Click Create subnet.
9. Repeat the same process for all six subnets using the configuration above.

**Explanation:**

- Public subnets are used for resources that need direct internet connectivity through an Internet Gateway.
- Private subnets are used for resources that should not have direct inbound internet access.
- Database subnets are used to host database resources in isolated network segments.
<img width="958" height="529" alt="image" src="https://github.com/user-attachments/assets/fdce0ad7-98ae-465a-a775-185107ee5284" />

---

# Step 4: Create Route Tables

Route tables control where network traffic from subnets is directed.

We need separate route tables for public, private and database subnets.

## 4.1 Create Public Route Table

### Configuration

| Field | Value |
|---|---|
| Name | roboshop-public |
| VPC | roboshop-dev |

### Procedure

1. Open VPC Dashboard.
2. Select Route Tables.
3. Click Create route table.
4. Enter the name `roboshop-public`.
5. Select VPC `roboshop-dev`.
6. Click Create route table.

## 4.2 Associate Public Subnets

1. Select the `roboshop-public` route table.
2. Open Subnet Associations.
3. Click Edit subnet associations.
4. Select the following subnets:
   - roboshop-public-1a
   - roboshop-public-1b
5. Click Save associations.

## 4.3 Add Internet Gateway Route

1. Select the `roboshop-public` route table.
2. Open the Routes tab.
3. Click Edit routes.
4. Click Add route.
5. Enter the following details:

| Field | Value |
|---|---|
| Destination | 0.0.0.0/0 |
| Target | Internet Gateway |
| Internet Gateway | roboshop-dev |

6. Click Save changes.

**Explanation:**

The route `0.0.0.0/0` represents all IPv4 destinations outside the VPC's local network.

By directing this traffic to the Internet Gateway, resources in the public subnets can communicate with the internet, provided they also have the required public IP and security configurations.

## 4.4 Create Private Route Table

Create another route table.

| Field | Value |
|---|---|
| Name | roboshop-private |
| VPC | roboshop-dev |

Associate these subnets:

- roboshop-private-1a
- roboshop-private-1b

## 4.5 Create Database Route Table

Create another route table.

| Field | Value |
|---|---|
| Name | roboshop-database |
| VPC | roboshop-dev |

Associate these subnets:

- roboshop-database-1a
- roboshop-database-1b

**Important:** The handwritten notes specify creating separate private and database route tables and associating their subnets, but do not specify their route destinations. Do not copy the public Internet Gateway default route into private or database route tables. Configure their routes according to the intended network design.

<img width="911" height="512" alt="image" src="https://github.com/user-attachments/assets/1bb6e85d-3ccc-4c45-ab42-9f6d4b250747" />

---

# Step 5: Allocate an Elastic IP Address

An Elastic IP is a static public IPv4 address that can be associated with AWS resources such as a NAT Gateway.

### Configuration

| Field | Value |
|---|---|
| Name | roboshop-NAT |

### Procedure

1. Open VPC Dashboard.
2. Select Elastic IP addresses.
3. Click Allocate Elastic IP address.
4. Keep the default Amazon IPv4 pool selection.
5. Click Allocate.
6. Select the newly allocated Elastic IP.
7. Add the name tag `roboshop-NAT`.

**Explanation:**

An Elastic IP provides a stable public IPv4 address. We use it for the NAT Gateway so private subnet resources can access the internet without receiving their own public IP addresses.

<img width="945" height="530" alt="image" src="https://github.com/user-attachments/assets/b653e795-5d9a-4642-8214-6b2e0a3ed9d6" />

---

# Step 6: Create a NAT Gateway

### Configuration

| Field | Value |
|---|---|
| Name | roboshop-dev |
| VPC | roboshop-dev |
| Connectivity type | Public |
| Elastic IP allocation | Manual |
| Elastic IP | roboshop-NAT |
| Availability Zone | us-east-1a |

### Procedure

1. Open VPC Dashboard.
2. Select NAT Gateways.
3. Click Create NAT Gateway.
4. Enter the name `roboshop-dev`.
5. Select VPC `roboshop-dev`.
6. Select the public subnet `roboshop-public-us-east-1a`.
7. Select Connectivity type as Public.
8. Choose Manual allocation of Elastic IP.
9. Select the Elastic IP allocated in Step 5.
10. Click Create NAT Gateway.
11. Wait until the NAT Gateway status changes to Available.

**Explanation:**

A NAT Gateway allows resources in private subnets to access the internet for outbound connections without allowing unsolicited inbound internet connections to those resources.

<img width="956" height="530" alt="image" src="https://github.com/user-attachments/assets/045d0280-4eff-403d-bc90-c08b37033da1" />

---

# Step 7: Configure Private Subnet Internet Access

For private subnet resources that need outbound internet access, add a default route to the NAT Gateway.

### Route configuration

| Field | Value |
|---|---|
| Destination | 0.0.0.0/0 |
| Target | NAT Gateway |
| NAT Gateway | roboshop-dev |

### Procedure

1. Open Route Tables.
2. Select `roboshop-private` and  roboshop-datbase
3. Open Routes.
4. Click Edit routes.
5. Add destination `0.0.0.0/0`.
6. Select the NAT Gateway as the target.
7. Save changes.

**Explanation:**

Private subnet resources can now initiate outbound internet connections through the NAT Gateway while remaining without direct public internet routing.

For a highly available production design, NAT Gateway placement and routing should also be planned per Availability Zone.

<img width="955" height="374" alt="image" src="https://github.com/user-attachments/assets/5455fc7d-ca36-4836-8ec7-bd93b5cb3a36" />

---

# Step 8: Verify the VPC Configuration

After completing all the steps, verify the resources in the AWS Console.

<img width="947" height="487" alt="image" src="https://github.com/user-attachments/assets/52a6e463-379d-4d31-8719-60f001d0006e" />


<img width="956" height="467" alt="image" src="https://github.com/user-attachments/assets/b3cce838-af97-417d-9319-c35ffe4dce5a" />


### Mandatory to delete the VPC ####
* disable the NAT gateway
* Delete the elastic IP, if you don't delete the elastic Ip amount will charge.
* Delete the VPC




mg width="955" height="527" alt="image" src="https://github.com/user-attachments/assets/c28882dd-7c0c-4db0-85e5-e6eca34b27b2" />
