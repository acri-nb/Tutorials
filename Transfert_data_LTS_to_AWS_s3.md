# Transferring Data from LTS Server to AWS S3 via Compute Server

This guide outlines the process of transferring data from an LTS server to the **acri243data** AWS S3 bucket using a **compute server** as an intermediary. This is necessary when the LTS server only holds data and lacks the required packages for direct S3 transfer.

**Assumptions:**

* You have an active AWS account with access to the **acri243data** S3 bucket.
* You have SSH access to both the compute server (10.111.243.103) and the LTS server (10.111.243.104).
* The compute server has the necessary packages installed: `sshfs`, `awscli`.

## Steps:

### 1. Connect to the Compute Server:

Open your terminal and SSH into the compute server:

```bash
ssh <user>@10.111.243.103
```

### 2. Install Necessary Packages (if not already installed):

Install `sshfs` and `awscli` if they are not already installed on the compute server.

```bash
sudo apt update  # Update package lists
sudo apt install sshfs awscli -y 
```

> **Note :** The previous packages are already installed in the compute server (10.111.243.103), but it's important to keep in mind that you'll need them if you want to use them on other calculation servers at the institute.

### 3. Configure AWS Credentials:

Configure the AWS CLI with your AWS access key ID and secret access key. You can find these credentials in your AWS console.

```bash
aws configure
```

### 4. Mount LTS Server Filesystem:

Create a directory on the compute server to serve as the mount point for the LTS server's filesystem:

```bash
mkdir /mnt/lts-server
```

Mount the **mnt/gth/ACRI** directory from the LTS server using `sshfs`:

```bash
sshfs <user>@10.111.243.104:/mnt/gth/ACRI /mnt/lts-server
```

**Please note:** Replace `<user>` with your username on the LTS server.

### 5. Transfer Data to AWS S3:

Use the `aws s3 cp` command to transfer the data from the mounted directory to your S3 bucket:

```bash
aws s3 cp /mnt/lts-server/ s3://acri243data --recursive
```

This command will recursively copy all files and subfolders within the `/mnt/gth/ACRI` directory on the LTS server to the root of the **acri243data** S3 bucket.

### 6. Unmount LTS Server Filesystem:

Once the transfer is complete, unmount the LTS server's filesystem:

```bash
sudo umount /mnt/lts-server
```

### 7. Verify Data Transfer:

Log in to your AWS console, navigate to your **acri243data** S3 bucket, and check if the data has been transferred successfully.

**Troubleshooting:**

* **Permission errors:** Ensure that the user on the compute server has read access to the data on the LTS server and write access to the S3 bucket.
* **Connectivity issues:** Verify network connectivity between the compute server, LTS server, and AWS.
* **AWS CLI errors:** Refer to the AWS CLI documentation for specific error messages.

**Security Considerations:**

* **SSH Key Authentication:** It is recommended to use SSH key authentication instead of passwords for secure access to the servers.
* **Data Encryption:** Consider encrypting your data at rest on the LTS server and in transit during the transfer process.
* **Access Control:** Implement appropriate access control mechanisms on your AWS S3 bucket to restrict data access. 

This document provides a comprehensive guide to transferring data from your LTS server to AWS S3 using a compute server as an intermediary. 
