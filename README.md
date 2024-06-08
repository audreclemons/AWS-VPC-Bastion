# AWS-VPC-Bastion

# Professional Explanation of AWS Security Configuration with Jump Box/Bastion Host


## Introduction

This document illustrates the deployment of a bastion host, comparing its placement in a private subnet versus a public subnet.

We'll start by looking at the traditional approach and examine the differences.

Currently, we have a bastion host placed in a public subnet. This setup offers good protection by reducing the attack surface from external threats.

We use the internet-facing bastion host to protect our internal resources. The internal resources trust the bastion host but not any external entities.

This trust model and layered protection have been effective, but given the increased frequency of attacks, especially since the pandemic, we need to reconsider our approach. There has been a 500% increase in attacks on internet-facing devices (Petrosyan, 2023).

## Architecture Before Hardening / Security Controls

![Before](https://github.com/audreclemons/AWS-VPC-Bastion/assets/171464782/7982a3fb-0b4e-4583-8b32-1639e9ed9216)


## Architecture After Hardening / Security Controls
![After](https://github.com/audreclemons/AWS-VPC-Bastion/assets/171464782/9b6df1ad-287a-4307-8866-b1b4b78c8f28)



To enhance our security, we propose a new model. This model involves adding another layer of protection by moving the bastion host from the public subnet to a private subnet.

In this new configuration, external entities will no longer directly communicate with the bastion host. Instead, they must go through a public-facing Elastic Load Balancer managed by AWS.

AWS will handle the due diligence, including monitoring and auditing, allowing us to focus on our core responsibilities.

Here's how it works: users needing access will have their specific IP addresses allowed through port 22. This access will be routed through the Elastic Load Balancer, which serves as the first line of defense with comprehensive monitoring features provided by AWS.

The internal resources will only trust the Elastic Load Balancer, adding a third layer of protection.

Additionally, we have auto-scaling configured. If the bastion host goes down, a new instance will automatically come online, enhancing our system's resiliency.

Each bastion host will have a file system for audit logs. These logs will be mounted on the bastion hosts and managed on AWS EFS, adding another layer of security.

Furthermore, the new configuration will utilize the Lynis security auditing tool to generate log files. This tool will enhance our security posture by providing detailed audits and compliance checks.

With these layers of protection, we believe this configuration will better defend against rising threats such as ransomware.



## Conclusion

In conclusion, by moving the bastion host to a private subnet, routing external access through an AWS-managed Elastic Load Balancer, and leveraging the Lynis security auditing tool, we are significantly enhancing our security framework. The use of AWS Elastic File System (EFS) to manage audit logs further strengthens our security measures by ensuring logs are securely stored and accessible. This new configuration not only reduces the attack surface but also provides multiple layers of protection, ensuring a more resilient and secure environment. The combination of these measures positions us to better defend against the increasing threats, especially in the current landscape of rising cyber attacks. This approach aligns with our commitment to maintaining robust security and ensuring the integrity of our internal resources.



Petrosyan, A. (2023, April 5). Retrieved from Statista: https://www.statista.com/statistics/1258261/covid-19-increase-in-cyber-attacks/
