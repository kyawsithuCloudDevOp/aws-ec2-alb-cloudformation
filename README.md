# AWS EC2 Web Hosting with HTTPS and an Application Load Balancer

A CloudFormation lab demonstrating web hosting on private EC2 instances, HTTPS, hostname-based routing and DNS.

Based on concepts practised during my DevOps internship. Environment-specific identifiers have been replaced with placeholders.

## Architecture

- A public Application Load Balancer receives website traffic.
- HTTP requests on port 80 redirect to HTTPS on port 443.
- An existing AWS Certificate Manager certificate enables HTTPS.
- Hostname rules route requests to separate PHP and LAMP servers.
- Route 53 alias records point both website names to the load balancer.
- A public bastion host provides access to the private servers.
- A Windows server runs in the private subnet.

## Files

- `vpc.yaml` — networking infrastructure, if included.
- `ec2-alb.yaml` — EC2 instances, security groups, load balancer, target groups, listeners and DNS records.

## Security

- PHP and LAMP servers accept HTTP traffic only from the ALB.
- SSH to the bastion is restricted to the administrator's IP.
- Private Linux servers accept SSH only from the bastion.
- Windows RDP access is restricted to the bastion.
- EC2 root volumes are encrypted.
- EC2 instances require IMDSv2.
- Private servers have no public IP addresses.

HTTPS terminates at the ALB. Traffic from the ALB to the web servers uses HTTP on port 80.

## Requirements

Before deployment, provide:

- An existing VPC with two public subnets in different Availability Zones.
- A private subnet with outbound access through a NAT gateway.
- Compatible Ubuntu and Windows Server AMIs.
- An existing EC2 key pair.
- An issued ACM certificate in the ALB region covering both website names.
- A public Route 53 hosted zone for your domain.

Update the `Mappings` section with your own account ID, resource IDs, hostnames and administrator IP address.

The public template contains placeholders and cannot be deployed unchanged.

## Web Servers

The PHP server installs Apache and PHP.

The LAMP server installs Apache, MariaDB and PHP.

Both servers create a simple PHP page for testing. The LAMP database is installed locally; this lab does not include a database-backed application.

## Validation

The template passed CloudFormation lint checks. Successful AWS deployment and website access must be verified in the target environment.

After deployment:

1. Check that both target groups become healthy.
2. Open each configured HTTPS hostname.
3. Confirm that HTTP requests redirect to HTTPS.
4. Confirm that each hostname displays the correct server's page.

## Skills Demonstrated

AWS CloudFormation, EC2, VPC networking, security groups, Application Load Balancers, ACM, Route 53, Linux, PHP and infrastructure as code.

## Limitations and Cleanup

This is a learning lab with one instance per website and no Auto Scaling.

AWS resources incur charges. Delete the application stack before deleting its networking stack. Deleting the EC2 instances also deletes their configured root volumes.# aws-ec2-alb-cloudformation
AWS CloudFormation project deploying a private nginx web server, a public Application Load Balancer, a bastion host and a Windows server.
