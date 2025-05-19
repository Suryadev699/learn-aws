# Troubleshooting Internet Accessibility for EC2 Instances  

When an EC2 instance is unable to access the internet despite having all necessary resources (EC2, VPC, Subnet, Route Table, Internet Gateway) created, follow the step-by-step approach below to identify and resolve the issue.  

## Scenario: Internet Gateway Detached and Route Table Misconfigured  
1. **Verify Internet Gateway Attachment**  
    - Check if the Internet Gateway is attached to the VPC.  
    - If it is detached, reattach the Internet Gateway to the correct VPC.  

2. **Inspect Route Table Configuration**  
    - Open the Route Table associated with the Subnet where the EC2 instance resides.  
    - Ensure there is a route with the destination `0.0.0.0/0` pointing to the Internet Gateway.  
    - If the route is missing or misconfigured, add or correct it.  

3. **Test the Connection**  
    - After making the changes, test the internet connectivity from the EC2 instance.  

## Generic Issues to Check  
Apart from the above scenario, consider the following common issues:  

1. **Security Group Rules**  
    - Ensure the Security Group associated with the EC2 instance allows outbound traffic to the internet (e.g., `0.0.0.0/0` for HTTP/HTTPS).  

2. **Network ACLs**  
    - Verify that the Network ACLs associated with the Subnet allow both inbound and outbound traffic for the required ports.  

3. **Elastic IP Address**  
    - If the EC2 instance is in a public subnet, ensure it has a public IP or an Elastic IP assigned.  

4. **Subnet Configuration**  
    - Confirm that the Subnet is marked as a public subnet by associating it with a Route Table that has a route to the Internet Gateway.  

5. **DNS Resolution**  
    - Check if the VPC has DNS resolution and DNS hostnames enabled.  
    - Ensure the EC2 instance can resolve domain names to IP addresses.  

6. **Instance-Level Issues**  
    - Verify that the instance has the correct network interface configuration.  
    - Check if the operating system's firewall or network settings are blocking internet access.  

By systematically addressing these potential issues, you can identify and resolve the root cause of internet connectivity problems for your EC2 instance.  