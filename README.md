# AWS-Compute
Of course! Using the same template, here's a write-up for a foundational AWS Compute lab.

---

### Lab Write-Up: Launching and Configuring an Amazon EC2 Instance

**Lab Objective:** To demonstrate fundamental AWS compute capabilities by launching, configuring, and managing an Amazon Elastic Compute Cloud (EC2) instance, the core compute service in AWS.

#### What I was supposed to do
1.  Launch an EC2 instance from the AWS Management Console.
2.  Select an appropriate Amazon Machine Image (AMI) and instance type based on requirements.
3.  Configure a Key Pair for secure SSH access.
4.  Configure the instance's Network Settings and Security Group to control traffic.
5.  Connect to the running instance securely using SSH.
6.  Install a web server (Apache) on the instance to host a simple webpage.
7.  Terminate the instance to stop incurring charges.

#### How I did it

1.  **Initiated Instance Launch:**
    *   Logged into the AWS Management Console and navigated to the **EC2 service**.
    *   Clicked the **"Launch Instance"** button to begin the configuration process.

2.  **Selected Software and Hardware (AMI & Instance Type):**
    *   **Choose an AMI:** I selected the **Amazon Linux 2023 AMI**. This is a free-tier eligible, secure, and high-performance operating system maintained by AWS.
    *   **Choose an Instance Type:** I selected the **t2.micro** instance type, which is eligible for the free tier. This provided me with a low-cost, general-purpose virtual server with 1 vCPU and 1 GiB of memory.

*(Representative Image: Selecting an AMI and Instance Type)*
![AWS EC2 Launch Instance showing AMI and instance type selection](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/choose-instance-type.png)

3.  **Configured Secure Access (Key Pair):**
    *   In the "Key pair (login)" section, I selected **"Create new key pair"**.
    *   I named the key pair `my-first-ec2-key` and downloaded the `my-first-ec2-key.pem` file. I understood that this private key file is essential and must be kept secure, as it is the only way to SSH into the instance.

4.  **Configured Network Security (Security Group):**
    *   In the "Network settings" section, I clicked **"Create security group"**.
    *   I configured the firewall rules as follows:
        *   **Rule 1:** Allow **SSH (port 22)** from "My IP" to ensure only I could connect securely.
        *   **Rule 2:** Allow **HTTP (port 80)** from "Anywhere" (`0.0.0.0/0`) so that anyone can access the web server I planned to install.

5.  **Launched and Connected to the Instance:**
    *   I reviewed all settings and clicked **"Launch Instance"**. My instance entered the `pending` state before transitioning to `running`.
    *   Once running, I copied the **Public IPv4 address** from the instance description.
    *   On my local machine, I opened a terminal and used the downloaded `.pem` key to establish an SSH connection:
        ```bash
        ssh -i "my-first-ec2-key.pem" ec2-user@<PUBLIC_IP_ADDRESS>
        ```
    *   I successfully gained access to the instance's command line, confirming secure connectivity.

6.  **Installed and Tested a Web Server:**
    *   On the EC2 instance, I used the package manager to install the Apache web server:
        ```bash
        sudo dnf update -y
        sudo dnf install -y httpd
        sudo systemctl start httpd
        sudo systemctl enable httpd
        ```
    *   I created a simple custom HTML page at `/var/www/html/index.html`.
    *   I opened a web browser on my local machine and navigated to `http://<PUBLIC_IP_ADDRESS>`. The Apache test page (and later my custom page) loaded successfully, proving the instance was functioning as a web server.

7.  **Terminated the Instance:**
    *   To avoid ongoing charges, I returned to the EC2 console, selected the instance, and selected **"Instance State" -> "Terminate"**. I confirmed the action, understanding that this would permanently delete the instance and its data.

#### Key Compute Concepts Demonstrated

*   **Amazon EC2 (Elastic Compute Cloud):** I learned that EC2 provides resizable compute capacity in the cloud. It allows you to launch virtual servers on demand.

*   **Amazon Machine Image (AMI):** An AMI is a template that contains the software configuration (OS, application server, applications) required to launch an instance. I used a pre-configured Amazon Linux AMI.

*   **Instance Types:** These define the hardware of the host computer for your instance. They offer varying combinations of CPU, memory, storage, and networking capacity. I used a `t2.micro`, which is ideal for low-traffic workloads.

*   **Key Pairs:** AWS uses public-key cryptography to secure login information for Linux instances. The key pair (consisting of a public key AWS stores and a private key file I store) is the secure credential for accessing the instance.

*   **Security Groups:** A Security Group acts as a virtual firewall for my instance to control inbound and outbound traffic. The rules I set were crucial for both security (restricting SSH) and functionality (allowing HTTP).

*   **Elasticity:** The process of launching and terminating instances on demand demonstrates the **elastic** nature of AWS compute. I only used and paid for the compute capacity I needed for the duration of the lab.

By completing this lab, I gained hands-on experience with the core service for compute in AWS. Understanding how to provision, secure, and manage an EC2 instance is a fundamental skill for the AWS Cloud Practitioner exam and forms the basis for working with more complex architectures.
