# EC2 Instance Management Guide

This document provides an overview of how to manage your EC2 instances, specifically focusing on stopping, rebooting, and terminating instances, as well as the impact on costs, IP addresses, and storage.

## EC2 Instance States

### 1. **Stopping an EC2 Instance**

#### Process:

- You can stop an EC2 instance by selecting the instance and choosing the "Stop" action.

#### Impact:

- **IP Address:** When the instance is stopped, the public IP address associated with it will be released. If you restart the instance, a new public IP address will be assigned.
- **Cost:**
  - **CPU Charges:** You will not be charged for the instance's CPU usage once it is stopped.
  - **Storage Charges:** However, you will still incur charges for the attached storage (EBS volume), which is where your data is stored.
  - **IP Address Charges:** You will stop incurring charges for the IP address once it is released. AWS charges for the IP address (around $0.5 per hour). The Free Tier may cover these costs depending on your account.

#### Considerations:

- **Domain & SSH:** If you use the IP address for domain mapping or SSH access, you will need to update these configurations after the instance is restarted, as the IP will change.
- **Storage Costs:** The data stored on the instance will remain intact, but you will continue to pay for storage.

### 2. **Rebooting an EC2 Instance**

#### Process:

- Rebooting the instance restarts it without changing its status or configuration.

#### Impact:

- **IP Address:** The public IP address remains the same after a reboot.
- **Cost:** The same as stopping the instance, you will still incur storage charges, but no additional CPU charges will be applied.

#### Considerations:

- Rebooting is useful when you want to restart the instance without releasing the IP address or incurring additional charges from stopping the instance.

### 3. **Terminating an EC2 Instance**

#### Process:

- Termination removes the instance and its associated resources, including the EBS volume.

#### Impact:

- **IP Address:** The public IP address is permanently released.
- **Data Deletion:** The attached EBS volume (storage) will be deleted, which means any data stored on the instance will be lost.

#### Cost:

- Once terminated, you will stop incurring charges for both the instance and the storage (unless other resources are still being used).

#### Considerations:

- **Irreversible:** Terminating the instance is a permanent action. You will need to launch a new instance if you need to continue using EC2.
- **Data Backup:** Ensure that important data is backed up before terminating the instance to avoid data loss.

## Summary of Actions

| Action    | IP Address Impact                | Data Impact                               | Cost Impact                                                | Irreversible |
| --------- | -------------------------------- | ----------------------------------------- | ---------------------------------------------------------- | ------------ |
| Stop      | IP released, new IP when started | Data preserved, storage costs still apply | No CPU costs, storage costs still apply                    | No           |
| Reboot    | IP remains unchanged             | Data preserved, storage costs still apply | No CPU costs, storage costs still apply                    | No           |
| Terminate | IP permanently released          | Data deleted, EBS volume removed          | No instance or storage costs, unless other resources exist | Yes          |

## Conclusion

- **Stopping** an instance reduces costs related to CPU usage but retains storage costs. The IP address will change when restarted.
- **Rebooting** is useful for restarting an instance without losing the IP address and retaining storage costs.
- **Terminating** an instance removes it entirely, including the IP address and storage, and is irreversible.

Understanding these different instance management options will help you optimize EC2 resource usage and manage costs effectively.
