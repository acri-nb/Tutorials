
# Ubuntu Server Update Guide

This guide provides the steps to safely update your Ubuntu server.

## Prerequisites

- Ensure you have a recent backup of the server.

## Update Steps

1. Update the package list:
   ```sh
   sudo apt update
   ```

2. Upgrade installed packages:
   ```sh
   sudo apt upgrade
   ```

3. Perform a distribution upgrade:
   ```sh
   sudo apt dist-upgrade
   ```

4. Clean up obsolete packages:
   ```sh
   sudo apt autoremove
   sudo apt autoclean
   ```

5. Check the current version:
   ```sh
   lsb_release -a
   ```

6. Upgrade to a new major release (if necessary):
   ```sh
   sudo do-release-upgrade
   ```

7. Reboot the server:
   ```sh
   sudo reboot
   ```

## Additional Steps for Key Issues

If you encounter GPG key errors, use the following commands to add or update the missing keys:

Add the missing GPG key for MySQL:
   ```sh
   sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B7B3B788A8D3785C
   ```

Update the expired GPG key for MongoDB:
   ```sh
   wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
   ```

Retry the update:
   ```sh
   sudo apt update
   sudo apt upgrade
   ```

## MySQL APT Configuration

During the MySQL APT configuration step, you will see a prompt.

### Recommended Configuration:

1. MySQL Server & Cluster: Leave it as the current version unless you want to upgrade.
2. MySQL Tools & Connectors: Enabled (recommended).
3. MySQL Preview Packages: Disabled (unless you want to test experimental features).

After selecting the appropriate options, click Ok to save the configuration.

## Post-Update Verification

After the server reboots, verify that all services are operational and check for any anomalies in the system logs:
   ```sh
   sudo journalctl -p err -b
   ```

This ensures that your Ubuntu server is updated and functioning correctly. If you encounter any issues or have further questions, feel free to reach out.

## Extra: Alternative Repository Configuration

If the default repositories are not working (e.g., for Bionic), you can use the following alternative repository configuration:

```sh
deb http://mirror.math.princeton.edu/pub/ubuntu/ focal main restricted
deb http://mirror.math.princeton.edu/pub/ubuntu/ focal-updates main restricted
deb http://mirror.math.princeton.edu/pub/ubuntu/ focal universe
deb http://mirror.math.princeton.edu/pub/ubuntu/ focal-updates universe
deb http://mirror.math.princeton.edu/pub/ubuntu/ focal multiverse
deb http://mirror.math.princeton.edu/pub/ubuntu/ focal-updates multiverse
deb http://mirror.math.princeton.edu/pub/ubuntu/ focal-backports main restricted universe multiverse
deb http://mirror.math.princeton.edu/pub/ubuntu/ focal-security main restricted
deb http://mirror.math.princeton.edu/pub/ubuntu/ focal-security universe
deb http://mirror.math.princeton.edu/pub/ubuntu/ focal-security multiverse
```
### Steps to Configure the Alternative Repository

1. **Backup Your Current Sources List**

   Before making any changes, it's a good practice to backup your current sources list:
   ```sh
   sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
   ```

2. **Edit the Sources List**

   Open the sources list file in a text editor of your choice:
   ```sh
   sudo nano /etc/apt/sources.list
   ```

3. **Add the Alternative Repository**

   Replace the existing repository URLs with the following alternative repository URLs.

4. **Save and Close the Sources List**

   Save the changes and close the text editor (in nano, you can do this by pressing `CTRL+X`, then `Y`, and `Enter`).

5. **Update the Package List**

   Update your package list to apply the new repository configuration:
   ```sh
   sudo apt update
   ```

6. **Upgrade Your System**

   Proceed with upgrading your system using the new repository:
   ```sh
   sudo apt upgrade
   sudo apt dist-upgrade
   ```
