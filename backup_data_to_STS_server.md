
# Backup Script Tutorial

This tutorial guides you through the process of setting up and executing a backup script to ensure it works correctly. Follow these steps to prepare, test, and optimize the script.

## Prerequisites

Before you begin, ensure you have:

- SSH access to the remote server.
- `rsync` installed on both the local and remote machines.
- Proper permissions for accessing and writing to the specified directories.

## Steps to Follow

### 1. Preparatory Steps

#### Configure SSH Keys

Set up SSH key-based authentication between your local machine and the remote server to avoid password prompts.

```bash
ssh-keygen -t rsa
ssh-copy-id root@192.168.0.230
```

#### Verify Permissions

Ensure that the user running the script has the necessary permissions to read the local directories and write to the log file. Also, verify that the remote user (`root`) has permissions to create directories and write to `${REMOTE_DIR}` on the remote server.

#### Install `rsync`

Make sure `rsync` is installed on both the local and remote machines.

For Debian/Ubuntu:
```bash
sudo apt-get install rsync
```

For CentOS/RHEL:
```bash
sudo yum install rsync
```

#### Test SSH Connection

Manually test the SSH connection to verify that access works without a password prompt.

```bash
ssh root@192.168.0.230
```

### 2. Script Execution

#### Test with a Small Directory

Before running the full script, perform a test backup with a smaller directory to ensure everything works as expected.

Create a test directory and file:
```bash
mkdir ~/test_backup
echo "test" > ~/test_backup/test_file.txt
```

Temporarily modify the script to back up only this test directory:
```bash
#!/bin/bash

REMOTE_SERVER="192.168.0.230"
REMOTE_USER="root"
REMOTE_DIR="/mnt/user/Recovered/Baxkup_103"
LOG_FILE="$HOME/backup.log"
DATE=$(date +%Y-%m-%d_%H-%M-%S)
HOSTNAME=$(hostname)

INCLUDE_DIRS=(
  "~/test_backup"
)

ssh ${REMOTE_USER}@${REMOTE_SERVER} "mkdir -p ${REMOTE_DIR}/${HOSTNAME}/${DATE}"

for DIR in "${INCLUDE_DIRS[@]}"; do
  if [ -d "${DIR}" ]; then
    rsync -a --delete --progress -e ssh ${DIR} ${REMOTE_USER}@${REMOTE_SERVER}:${REMOTE_DIR}/${HOSTNAME}/${DATE} &>> ${LOG_FILE}
    if [ $? -eq 0 ]; then
      echo "[$(date)] Backup of ${DIR} successful." >> ${LOG_FILE}
    else
      echo "[$(date)] Backup of ${DIR} failed." >> ${LOG_FILE}
    fi
  else
    echo "[$(date)] Directory ${DIR} does not exist." >> ${LOG_FILE}
  fi
done
```

#### Run the Script in Verbose Mode

Execute the script with the `-x` option for debugging to see each command executed.

```bash
bash -x backup_script.sh
```

#### Check the Logs

Examine the log file to ensure that all steps were executed correctly.

```bash
cat ~/backup.log
```

#### Execute the Full Script

Once the test is successful, modify `INCLUDE_DIRS` to include all directories you want to back up and run the full script.

```bash
#!/bin/bash

REMOTE_SERVER="192.168.0.230"
REMOTE_USER="root"
REMOTE_DIR="/mnt/user/Recovered/Baxkup_103"
LOG_FILE="$HOME/backup.log"
DATE=$(date +%Y-%m-%d_%H-%M-%S)
HOSTNAME=$(hostname)

INCLUDE_DIRS=(
  "/mnt"
  "/references"
  "/home"
)

ssh ${REMOTE_USER}@${REMOTE_SERVER} "mkdir -p ${REMOTE_DIR}/${HOSTNAME}/${DATE}"

for DIR in "${INCLUDE_DIRS[@]}"; do
  if [ -d "${DIR}" ]; then
    rsync -a --delete --progress -e ssh ${DIR} ${REMOTE_USER}@${REMOTE_SERVER}:${REMOTE_DIR}/${HOSTNAME}/${DATE} &>> ${LOG_FILE}
    if [ $? -eq 0 ]; then
      echo "[$(date)] Backup of ${DIR} successful." >> ${LOG_FILE}
    else
      echo "[$(date)] Backup of ${DIR} failed." >> ${LOG_FILE}
    fi
  else
    echo "[$(date)] Directory ${DIR} does not exist." >> ${LOG_FILE}
  fi
done
```

### 3. Automate with `cron`

To automate the script execution, use `cron`.

Edit the crontab to add a scheduled task. For example, to run the script every day at 2 AM:

```bash
crontab -e
```

Add the following line:
```bash
0 2 * * * /path/to/backup_script.sh
```

### 4. Monitoring

Regularly check the log files for any issues. Configure email alerts to notify you of any backup failures.

By following these steps, you can ensure that your backup script operates correctly and optimally.
