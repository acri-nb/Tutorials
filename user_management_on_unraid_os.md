
# SSH Configuration and User Management on Unraid

This Tutorial provides step-by-step instructions for configuring SSH access on an Unraid server, including how to add a new user and ensure proper SSH connectivity.

You have to follow this step after created the new user throught the Dashboard.

## SSH Configuration

Ensure the SSH configuration allows root login, password authentication, and specific users.

1. Open the SSH configuration file:

   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

2. Add or modify the following lines to ensure proper configuration:

   ```plaintext
   PermitRootLogin yes
   PasswordAuthentication yes
   AllowUsers root marora hgayap acheema eallain
   ```

3. Save the file and exit the editor.

4. Restart the SSH service:

   ```bash
   sudo /etc/rc.d/rc.sshd restart
   ```

## Adding a New User

Follow these steps to add a new user and configure SSH access.

1. Create the new user:

   ```bash
   sudo adduser newuser
   ```

2. Set the password for the new user:

   ```bash
   sudo passwd newuser
   ```

3. Modify the user's shell to `/bin/bash`:

   ```bash
   sudo chsh -s /bin/bash newuser
   ```

4. Create the user's home directory (if not automatically created):

   ```bash
   sudo mkdir -p /home/newuser
   sudo chown newuser:users /home/newuser
   sudo chmod 700 /home/newuser
   ```

5. Configure SSH for the new user:

   ```bash
   sudo mkdir -p /home/newuser/.ssh
   sudo touch /home/newuser/.ssh/authorized_keys
   sudo chown -R newuser:users /home/newuser/.ssh
   sudo chmod 700 /home/newuser/.ssh
   sudo chmod 600 /home/newuser/.ssh/authorized_keys
   ```

6. Add the new user to the `AllowUsers` line in the SSH configuration file:

   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

   Update the `AllowUsers` line to include the new user:

   ```plaintext
   AllowUsers root marora hgayap acheema eallain newuser
   ```

7. Save the file and exit the editor.

8. Restart the SSH service:

   ```bash
   sudo /etc/rc.d/rc.sshd restart
   ```

## Troubleshooting SSH Connectivity

If users are unable to connect via SSH, follow these steps to diagnose and resolve the issue.

1. Verify the home directory and permissions for the user:

   ```bash
   sudo mkdir -p /home/username
   sudo chown username:users /home/username
   sudo chmod 700 /home/username
   ```

2. Ensure the `.ssh` directory and `authorized_keys` file are correctly configured:

   ```bash
   sudo mkdir -p /home/username/.ssh
   sudo touch /home/username/.ssh/authorized_keys
   sudo chown -R username:users /home/username/.ssh
   sudo chmod 700 /home/username/.ssh
   sudo chmod 600 /home/username/.ssh/authorized_keys
   ```

3. Check the SSH configuration file:

   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

   Ensure the following lines are present and correctly configured:

   ```plaintext
   PermitRootLogin yes
   PasswordAuthentication yes
   AllowUsers root marora hgayap acheema eallain newuser
   ```

4. Save the file and exit the editor.

5. Restart the SSH service:

   ```bash
   sudo /etc/rc.d/rc.sshd restart
   ```

6. Check the logs for authentication issues:

   ```bash
   sudo tail -f /var/log/secure
   sudo tail -f /var/log/messages
   sudo grep -r "sshd" /var/log/
   ```

7. If `systemctl` is available, use `journalctl` to view SSH logs:

   ```bash
   sudo journalctl -u sshd
   ```

## Testing SSH Connectivity

Test SSH connectivity for each user:

```bash
ssh root@<server-ip>
ssh marora@<server-ip>
ssh hgayap@<server-ip>
ssh acheema@<server-ip>
ssh eallain@<server-ip>
ssh newuser@<server-ip>
```

Replace `<server-ip>` with the actual IP address of your Unraid server.

By following these steps, you should be able to add new users and ensure they can connect via SSH to your Unraid server.
