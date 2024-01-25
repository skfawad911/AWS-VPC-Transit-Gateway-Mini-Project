# AWS VPC Transit Gateway Mini Project

## Overview

This project demonstrates the implementation of VPC Transit Gateway in AWS, connecting three VPCs—VPC1 (12.0.0.0/16), VPC2 (13.0.0.0/16), and VPC3 (14.0.0.0/16). Each VPC includes an internet gateway, public subnet, route table, and an EC2 instance hosting a webpage displaying server details.

## VPC Configuration

### VPC1

- **IPv4 CIDR Block:** 12.0.0.0/16
- **Internet Gateway**
- **Public Subnet:** 12.0.1.0/24

### VPC2

- **IPv4 CIDR Block:** 13.0.0.0/16
- **Internet Gateway**
- **Public Subnet:** 13.0.1.0/24

### VPC3

- **IPv4 CIDR Block:** 14.0.0.0/16
- **Internet Gateway**
- **Public Subnet:** 14.0.1.0/24

## EC2 Instances

- **Instance (in each VPC):**
  - Instance Type: t2.micro
  - Security Groups: Allow SSH, HTTP
  - User Data:
    ```bash
    #!/bin/bash

    # Update package information
    yes | sudo apt update

    # Install Apache2
    yes | sudo apt install apache2

    # Create the index.html file with server details
    echo "<h1>Welcome to EC2 Instance</h1><br><h1>Server Details</h1>
    <p><strong>Hostname: </strong>$(hostname)</p>
    <p><strong>IP Address: </strong>$(hostname -I | cut -d' ' -f1)</p>" > /var/www/html/index.html

    # Restart Apache2
    sudo systemctl restart apache2
    ```

## Transit Gateway Configuration

- **Transit Gateway**
- **Transit Attachments (3):**
  - Attach each VPC to the Transit Gateway

## Verification

1. SSH into each EC2 instance in VPC1, VPC2, and VPC3.
   ```bash
   ssh -i [Your_Key_Pair.pem] ec2-user@[Instance_Public_IP]

2. Run the following command to check connectivity between instances:
```bash
curl [Other_Instance_Private_IP]
```
- You should see the content of the index.html page, displaying server details.

## VPC Peering vs. Transit Gateway
- VPC Peering:

Use Case: Connect VPCs within the same AWS region.
Limitations: Limited to a single region, and a full mesh topology might become complex with many VPCs.

- Transit Gateway:

Use Case: Connect VPCs across multiple AWS regions and scale more efficiently.
Advantages: Simplifies network architecture, supports transitive routing, and can handle a large number of VPC connections.

## Conclusion

This project showcases the successful implementation of VPC Transit Gateway, providing a scalable and efficient solution for interconnecting multiple VPCs. Understanding the differences between VPC Peering and Transit Gateway is crucial in choosing the appropriate solution based on specific requirements.
