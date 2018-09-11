We have our Linux VM deployed and running, but it's not configured to do any work. Let's connect to it with SSH and configure Apache, so we have a running web server.

## Connect to the VM with SSH

To connect to an Azure VM with an SSH client, you will need:

- SSH client software (present on most modern operating systems)
- The public IP address of the VM (or private if the VM is configured to connect to your network)

### Get the public IP address

1. In the [Azure portal](https://portal.azure.com?azure-portal=true), ensure the **Overview** panel for the virtual machine that you created earlier is open. You can find the VM under **All Resources** if you need to open it. The overview panel has a lot of information about the VM.

    - You can see whether the VM is running
    - Stop or restart it
    - Get the public IP address to connect to the VM
    - See the activity of the CPU, disk, and network

1. Click the **Connect** button at the top of the pane.

1. In the **Connect to virtual machine** blade, note the **IP address** and **Port number** settings. On the **SSH** tab, you will also find the command you need to execute locally to connect to the VM. Copy this to the clipboard.

<!-- TODO: This will be necessary if we ever have inline portal integration. 

### Open Azure Cloud Shell

Let's use Cloud Shell in the Azure portal. If you generated the SSH key locally, you need to use your local session since the private key won't be in your storage account:

1. Switch back to the **Dashboard** by clicking the **Dashboard** button in the Azure sidebar.

1. Open Cloud Shell by clicking the **shell** button in the top toolbar.

    ![Screenshot of the Azure portal top navigation bar with the Azure Cloud Shell button highlighted.](../media/6-cloud-shell.png)

1. Select **Bash** as the shell type. PowerShell is also available if you are a Windows administrator.

-->

## Connect with SSH

1. Paste the command line you got from the SSH tab into Azure Cloud Shell. It should look something like this; however, it will have a different IP address (and perhaps a different username if you didn't use **jim**!):

    ```bash
    ssh jim@137.117.101.249
    ```

1. This command will open a Secure Shell connection and place you at a traditional shell command prompt for Linux.

1. Try executing a few Linux commands
    - `ls -la /` to show the root of the disk
    - `ps -l` to show all the running processes
    - `dmesg` to list all the kernel messages
    - `lsblk` to list all the block devices - here you will see your drives

The more interesting thing to observe in the list of drives is what is _missing_. Notice that our **Data** drive (`sdc`) is present but not mounted into the file system. Azure added a VHD but didn't initialize it.

## Initialize data disks

Any additional drives you create from scratch will need to be initialized and formatted. The process for doing this is identical to a physical disk:

1. First, identify the disk. We did that above. You could also use `dmesg | grep SCSI`, which will list all the messages from the kernel for SCSI devices.

1. Once you know the drive (`sdc`) you need to initialize, you can use `fdisk` to do that. You will need to run the command with `sudo` and supply the disk you want to partition:

    ```bash
    sudo fdisk /dev/sdc
    ```
1. Use the `n` command to add a new partition. In this example, we also choose **p** for a primary partition and accept the rest of the default values. The output will be similar to the following example:   

    ```output
    Device does not contain a recognized partition table.
    Created a new DOS disklabel with disk identifier 0x1f2d0c46.

    Command (m for help): n
    Partition type
       p   primary (0 primary, 0 extended, 4 free)
       e   extended (container for logical partitions)
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-2145386495, default 2048):
    Last sector, +sectors or +size{K,M,G,T,P} (2048-2145386495, default 2145386495):

    Created a new partition 1 of type 'Linux' and of size 1023 GiB.
    ```

1. Print the partition table with the `p` command. It should look something like this:

    ```output
    Disk /dev/sdc: 1023 GiB, 1098437885952 bytes, 2145386496 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: dos
    Disk identifier: 0x1f2d0c46

    Device     Boot Start        End    Sectors  Size Id Type
    /dev/sdc1        2048 2145386495 2145384448 1023G 83 Linux
    ```

1. Write the changes with the `w` command. This will exit the tool.

1. Next, we need to write a file system to the partition with the `mkfs` command. We will need to specify the file system type and device name that we got from the `fdisk` output:
    - Pass `-t ext4` to create an _ext4_ filesystem.
    - The device name is `/dev/sdc`.

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    This command will take a few minutes to complete.

    ```output
    mke2fs 1.44.1 (24-Mar-2018)
    Discarding device blocks: done
    Creating filesystem with 268173056 4k blocks and 67043328 inodes
    Filesystem UUID: e311c905-e0d9-43ab-af63-7f4ee4ef108e
    Superblock backups stored on blocks:
            32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
            4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
            102400000, 214990848

    Allocating group tables: done
    Writing inode tables: done
    Creating journal (262144 blocks): done
    Writing superblocks and filesystem accounting information: done
    ```

1. Next, create a directory we will use as our mount point. Let's assume we will have a `data` folder:

    ```bash
    sudo mkdir /data
    ```
1. Finally, use `mount` to attach the disk to the mount point:

    ```bash
    sudo mount /dev/sdc1 /data
    ```
    You should be able to use `lsblk` to see the mounted drive now:
    
    ```output
    NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda       8:0    0   30G  0 disk
    ├─sda1    8:1    0 29.9G  0 part /
    ├─sda14   8:14   0    4M  0 part
    └─sda15   8:15   0  106M  0 part /boot/efi
    sdb       8:16   0   16G  0 disk
    └─sdb1    8:17   0   16G  0 part /mnt
    sdc       8:32   0 1023G  0 disk
    └─sdc1    8:33   0 1023G  0 part /data
    sr0      11:0    1  628K  0 rom
    ```

### Mounting the drive automatically

To ensure that the drive is mounted automatically after a reboot, it must be added to the `/etc/fstab` file. It is also highly recommended that the UUID (universally unique identifier) is used in `/etc/fstab` to refer to the drive rather than just the device name (such as `/dev/sdc1`). If the OS detects a disk error during boot, using the UUID avoids the incorrect disk being mounted to a given location. Remaining data disks would then be assigned those same device IDs. To find the UUID of the new drive, use the `blkid` utility:

```bash
sudo -i blkid
```

It will return something like:

```output
/dev/sda1: UUID="36a59c42-c04c-4632-b83f-7015abd10358" TYPE="ext4"
/dev/sdb1: UUID="21092a62-79d4-4d8c-91de-4dd4e8b97d83" TYPE="ext4"
/dev/sdc1: UUID="e311c905-e0d9-43ab-af63-7f4ee4ef108e" TYPE="ext4"
```

1. Copy the UUID for the `/dev/sdc1` drive and open the `/etc/fstab` file in a text editor:

    ```bash
    sudo vi /etc/fstab
    ```

> [!WARNING]
> Improperly editing the `/etc/fstab` file could result in an unbootable system. If unsure, refer to the distribution's documentation for information on how to properly edit this file. It is also recommended that a backup of the file is created before editing when you are working with production systems.

1. Press **G** to move to the last line in the file.

1. Press **I** to enter INSERT mode. It should indicate the mode at the bottom of the screen.

1. Press the **END** key to move to the end of the line. Alternatively, you can use the arrow keys. Press **ENTER** to move to a new line.

1. Type the following line into the editor. The values can be space or tab separated. Check the documentation for more information on each of the columns:

    ```output
    UUID=<uuid-goes-here>    /data    ext4    defaults,nofail    1    2
    ```
1. Press **ESC**, then type **:w!** to write the file and **:q** to quit the editor.

1. Finally, let's check to make sure the entry is correct by asking the OS to refresh the mount points:

    ```bash
    sudo mount -a
    ```

    If it returns an error, edit the file to find the problem.

> [!TIP]
> Some Linux kernels support TRIM to discard unused blocks on disks. This feature is available on Azure disks and can save you money if you create large files and then delete them. Learn how to [turn this feature on](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure) in our documentation.

## Install software onto the VM

You have several options to install software onto the VM. First, as mentioned, you can use `scp` to copy local files from your machine to the VM. This lets you copy over data or custom applications you want to run.

You can also install software through Secure Shell. Azure machines are, by default, internet connected. You can use standard commands to install popular software packages directly from standard repositories. Let's use this approach to install Apache.

### Install the Apache web server

Apache is available within Ubuntu's default software repositories, so we will install it using conventional package management tools:

1. Start by updating the local package index to reflect the latest upstream changes:

    ```bash
    sudo apt-get update
    ```
    
1. Next, install Apache:

    ```bash
    sudo apt-get install apache2
    ```

1. It should start automatically - we can check the status using `systemctl`:

    ```bash
    sudo systemctl status apache2
    ```

    This should return something like:

    ```output
    apache2.service - The Apache HTTP Server
       Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
      Drop-In: /lib/systemd/system/apache2.service.d
               └─apache2-systemd.conf
       Active: active (running) since Mon 2018-09-03 21:00:03 UTC; 1min 34s ago
     Main PID: 11156 (apache2)
        Tasks: 55 (limit: 4915)
       CGroup: /system.slice/apache2.service
               ├─11156 /usr/sbin/apache2 -k start
               ├─11158 /usr/sbin/apache2 -k start
               └─11159 /usr/sbin/apache2 -k start

    test-web-eus-vm1 systemd[1]: Starting The Apache HTTP Server...
    test-web-eus-vm1 apachectl[11129]: AH00558: apache2: Could not reliably determine the server's fully qua
    test-web-eus-vm1 systemd[1]: Started The Apache HTTP Server.
    ```

1. Finally, we can try retrieving the default page through the public IP address. It should return a default page.

    ![Screenshot of a web browser showing the Apache default web page hosted at the IP of the new Linux VM.](../media/6-apache-works.png)

As you can see, SSH allows you to work with the Linux VM just like a local computer. You can administer this VM as you would any other Linux computer: installing software, configuring roles, adjusting features, and other everyday tasks. However, it's a manual process - if we always need to install some software, you might consider automating the process using scripting.
