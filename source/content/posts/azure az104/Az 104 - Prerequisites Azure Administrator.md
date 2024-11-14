---
draft: false
date: 2024-11-14
slug: Az 104 - Prerequisites Azure Administrator
tags:
  - azure
  - az104
---
# Introduction to Azure Cloud Shell

Azure Cloud Shell is a command-line environment you can access through your web browser. You can use this environment to manage Azure resources, including VMs, storage, and networking. Just like you do when using the Azure CLI or Azure PowerShell.

Because Microsoft manages Cloud Shell, you always have access to the most recent versions of the Azure CLI and PowerShell modules right from any browser. You don't have to worry about keeping modules up to date. With Cloud Shell, you just open your browser and sign in. Just like that, you have access to a command-line environment fully connected with your account's permissions and the resources to which you have access. All that works in an infrastructure that's compliant with double encryption at rest by default. You don't need to take any further action!

Azure Cloud Shell also provides cloud storage to persist files such as SSH keys, scripts, and more. This functionality lets you access important files in between sessions and with different machines. Finally, you can use the Cloud Shell editor to make changes to files, such as scripts, that are saved into this cloud storage directly from the Cloud Shell interface.

---
# How does Azure Cloud Shell work?

As an IT admin for Contoso Corporation, you're frequently on-call to perform administrative tasks and resolve workload disruptions to resources in your organization's Azure subscriptions. When visiting a family member during a weekend that you're on call, the development team notifies you of a problem with an Azure virtual machine (VM). The VM became nonresponsive during scheduled maintenance for the upgrade of an application that runs on it. Because the developers weren't granted access to the underlying Azure virtual machine hosting infrastructure, they're only able to remotely access the VM when it's operating normally. So, you're being called to diagnose and remediate the problem.

Since you're visiting family, you don’t have access to your administrative workstation and diagnostic scripts. You do have access to a laptop with an internet browser. Using the laptop, you browse to the Azure portal, authenticate against your organization’s Azure subscription, open Azure Cloud Shell, mount an Azure File Share, access your diagnostic scripts, and diagnose and remediate the problems with the VM, returning it to operation.

## Access Cloud Shell

You have a few different options for accessing Azure Cloud Shell:

- From a direct link: [https://shell.azure.com](https://shell.azure.com/)
    
    [![A screenshot of Cloud Shell accessed directly from a link.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/access-cloud-shell-directly.png)](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/access-cloud-shell-directly.png#lightbox)
    
- From the Azure portal
    
    [![A screenshot of Cloud Shell accessed from Azure portal.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/access-cloud-shell-from-azure-portal.png)](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/access-cloud-shell-from-azure-portal.png#lightbox)
    
- From code snippets when accessing Microsoft Learn:
    
    [![A screenshot of Cloud Shell accessed from code snippets.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/access-cloud-shell-from-code-snippets.png)](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/access-cloud-shell-from-code-snippets.png#lightbox)
    

When you open a Cloud Shell session, a temporary host is allocated to your session. This VM is preconfigured with the latest versions of PowerShell and Bash. You can then select the command-line experience you want to use:

[![A screenshot of how to choose a command-line experience in a Cloud Shell session.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/select-cli-experience.png)](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/select-cli-experience.png#lightbox)

After you select the shell experience you want to use, you can start managing your Azure resources:

[![A screenshot of how to use Cloud Shell to manage Azure resources.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/manage-azure-resources-in-cloud-shell.png)](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/manage-azure-resources-in-cloud-shell.png#lightbox)

Cloud Shell sessions terminate after 20 minutes of inactivity. When a session terminates, files on your CloudDrive are persisted, but you need to start a new session to access the Cloud Shell environment.

## Access your own scripts and files

When using Cloud Shell, you might also need to run scripts or use files for different actions. You can persist files on Cloud Shell by using the Azure CloudDrive:

[![A screenshot of how to access CloudDrive in a Cloud Shell session.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/use-azure-cloud-drive.png)](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/use-azure-cloud-drive.png#lightbox)

After uploading files, you can interact with them as you would in a regular PowerShell or Bash session:

[![A screenshot of how to manage files in CloudDrive.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/manage-files-in-cloud-drive.png)](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/manage-files-in-cloud-drive.png#lightbox)

Now that your file resides on CloudDrive, you can close the session and open another session on a different device and still access the same file. Cloud Shell also lets you map an Azure Storage File Share, which is tied to a specific region. Access to an Azure File Share lets you work with the contents of that share through Cloud Shell.

If you need to edit scripts hosted on the CloudDrive or File Share, you can use the Cloud Shell editor. Select the curly brackets {} icon on the browser and open the file you want to edit, or use the command `code` and specify the filename; for example:

BashCopy

```
code temp.txt
```

[![A screenshot of how to access the Cloud Shell editor mode.](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/cloud-shell-edit-scripts.png)](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-cloud-shell/media/cloud-shell-edit-scripts.png#lightbox)

 Note

The `code` command only works in the Classic mode in Cloud Shell. To enable Classic mode, select the **More** icon (**...**), then select **Settings** > **Go to Classic version**.

## Cloud Shell tools

If you need to manage resources (such as Docker containers or Kubernetes Clusters) or want to use non-Microsoft tools (such as Ansible and Terraform) in Cloud Shell, the Cloud Shell session comes with these add-ons already preconfigured.

Here’s a list of all add-ons available to you within a Cloud Shell session:

Expand table

| Category           | Name                                                                                                                                                                       |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Linux tools**    | bash  <br>zsh  <br>sh  <br>tmux  <br>dig                                                                                                                                   |
| **Azure tools**    | Azure CLI and [Azure classic CLI](https://github.com/Azure/azure-xplat-cli)  <br>AzCopy  <br>Azure Functions CLI  <br>Service Fabric CLI  <br>Batch Shipyard  <br>blobxfer |
| **Text editors**   | code (Cloud Shell editor)  <br>vim  <br>nano  <br>emacs                                                                                                                    |
| **Source control** | git                                                                                                                                                                        |
| **Build tools**    | make  <br>maven  <br>npm  <br>pip                                                                                                                                          |
| **Containers**     | Docker Machine  <br>Kubectl  <br>Helm  <br>DC/OS CLI                                                                                                                       |
| **Databases**      | MySQL client  <br>PostgreSql client  <br>sqlcmd Utility  <br>mssql-scripter                                                                                                |
| **Other**          | iPython Client  <br>Cloud Foundry CLI  <br>Terraform  <br>Ansible  <br>Chef InSpec  <br>Puppet Bolt  <br>HashiCorp Packer  <br>Office 365 CLI                              |

---
# When should you use Azure Cloud Shell?

As an IT Admin for Contoso Corporation, you need alternatives to interact with Azure resources from the command line even when not using your default administrative device.

You can use Azure Cloud Shell to:

- Open a secure command-line session from any browser-based device.
- Interact with Azure resources without the need to install plug-ins or add-ons to your device.
- Persist files between sessions for later use.
- Use either Bash or PowerShell, whichever you prefer, to manage Azure resources.
- Edit files (such as scripts) via the Cloud Shell editor.

You shouldn't use Azure Cloud Shell if:

- You intend to leave a session open for more than 20 minutes for long running scripts or activities. In these cases, your session is disconnected without warning, and the current state is lost.
- You need admin permissions, such as sudo access, from within the Azure CLI or PowerShell environment.
- You need to install tools that aren't supported in the limited Cloud Shell environment, but instead require an environment such as a custom virtual machine or container.
- You need storage from different regions. You might need to back up and synchronize this content since only one region can have the storage allocated to Azure Cloud Shell.
- You need to open multiple sessions at the same time. Azure Cloud Shell allows only one instance at time and isn't suitable for concurrent work across multiple subscriptions or tenants.

---

# Introduction to Bash

# Introduction

Imagine you just started a new job as a system administrator (sysadmin) at Northwind, a high-frequency trading (HFT) firm that runs Windows on its client computers and Linux on its server computers. Computers are the lifeblood of the company, and you need to learn more about how to manage Linux boxes. It's time to skill up!

Bash is the standard shell scripting language for Linux. Let's learn the basics of Bash, starting with the syntax and commonly used commands, like `ls` and `cat`.

# What is Bash?

Bash is a vital tool for managing Linux machines. The name is short for "**B**ourne **A**gain **Sh**ell."

A shell is a program that commands the operating system to perform actions. You can enter commands in a console on your computer and run the commands directly, or you can use scripts to run batches of commands. Shells like PowerShell and Bash give system administrators the power and precision they need for fine-tuned control of the computers they're responsible for.

There are other Linux shells, including csh and zsh, but Bash became the de facto Linux standard. That's because Bash is compatible with Unix's first serious shell, the Bourne shell, also known as sh. Bash incorporates the best features of its predecessors. But Bash also has some fine features of its own, including built-in commands and the ability to invoke external programs.

One reason for Bash's success is its simplicity. Bash, like the rest of Linux, is based on the Unix design philosophy. As Peter Salus summarized in his book _A Quarter Century of Unix_, three of the "big ideas" embodied in Unix are:

- Programs do one thing and do it well
- Programs work together
- Programs use text streams as the universal interface

The last part is key to understanding how Bash works. In Unix and Linux, everything is a file. That means you can use the same commands without worrying about whether the I/O stream—the input and output—comes from a keyboard, a disk file, a socket, a pipe, or another I/O abstraction.

Let's learn the basics of Bash, starting with the syntax and commonly used commands, like `ls` and `cat`.

---

# Bash fundamentals

An understanding of Bash starts with an understanding of Bash syntax. After you know the syntax, you can apply it to every Bash command you run.

The full syntax for a Bash command is:

BashCopy

```
command [options] [arguments]
```

Bash treats the first string it encounters as a command. The following command uses Bash's `ls` (for "list") command to display the contents of the current working directory:

BashCopy

```
ls
```

Arguments often accompany Bash commands. For example, you can include a path name in an `ls` command to list the contents of another directory:

BashCopy

```
ls /etc
```

Most Bash commands have options for modifying how they work. Options, also called _flags_, give a command more specific instructions. As an example, files and directories whose names begin with a period are hidden from the user and aren't displayed by `ls`. However, you can include the `-a` (for "all") flag in an `ls` command and see everything in the target directory:

BashCopy

```
ls -a /etc
```

You can even combine flags for brevity. For example, rather than enter `ls -a -l /etc` to show all files and directories in Linux's **/etc** directory in long form, you can enter this instead:

BashCopy

```
ls -al /etc
```

Bash is concise. It's sometimes remarkable (and a point of pride among Bash aficionados) how much you can accomplish with a single command.

## Get help

Which options and arguments can be used, or must be used, varies from command to command. Fortunately, Bash documentation is built into the operating system. Help is never more than a command away. To learn about the options for a command, use the `man` (for "manual") command. For instance, to see all the options for the `mkdir` ("make directory") command, do this:

BashCopy

```
man mkdir
```

`man` is your best friend as you learn Bash. `man` is how you find the information you need to understand how any command works.

Most Bash and Linux commands support the `--help` option. This shows a description of the command's syntax and options. To demonstrate, enter `mkdir --help`. The output looks something like this:

OutputCopy

```
Usage: mkdir [OPTION]... DIRECTORY...
Create the DIRECTORY(ies), if they do not already exist.
    
Mandatory arguments to long options are mandatory for short options too.
  -m, --mode=MODE   set file mode (as in chmod), not a=rwx - umask
  -p, --parents     no error if existing, make parent directories as needed
  -v, --verbose     print a message for each created directory
  -Z                   set SELinux security context of each created directory
                         to the default type
      --context[=CTX]  like -Z, or if CTX is specified then set the SELinux
                         or SMACK security context to CTX
      --help     display this help and exit
      --version  output version information and exit
    
GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
Report mkdir translation bugs to <http://translationproject.org/team/>
Full documentation at: <http://www.gnu.org/software/coreutils/mkdir>
or available locally via: info '(coreutils) mkdir invocation'
```

Help obtained this way is typically more concise than help obtained with `man`.

## Use wildcards

Wildcards are symbols that represent one or more characters in Bash commands. The most frequently used wildcard is the asterisk. It represents zero characters or a sequence of characters. Suppose your current directory contains hundreds of image files, but you only want to see the PNG files; the ones whose file names end with **.png**. Here's the command to list only those files:

BashCopy

```
ls *.png
```

 Note

Linux has no formal concept of a file-name extension as other operating systems do. This doesn't mean that PNG files won't have a **.png** extension. It simply means Linux attaches no special significance to the fact that the file names end with **.png**.

Now let's say the current directory also contains JPEG files. Some end in **.jpg**, while others end in **.jpeg.** Here's one way to list all the JPEG files:

BashCopy

```
ls *.jpg *.jpeg
```

And here's another:

BashCopy

```
ls *.jp*g
```

The `*` wildcard matches on zero or more characters, but the `?` wildcard represents a single character. If the current directory contains files named **0001.jpg**, **0002.jpg**, and so on through **0009.jpg**, the following command lists them all:

BashCopy

```
ls 000?.jpg
```

Yet another way to use wildcards to filter output is to use square brackets, which denote groups of characters. The following command lists all the files in the current directory whose names contain a period immediately followed a lowercase J or P. It lists all the **.jpg**, **.jpeg**, and **.png** files, but not **.gif** files:

BashCopy

```
ls *.[jp]*
```

In Linux, file names and the commands that operate upon them are case-sensitive. So to list all the files in the current directory whose names contain periods followed by an uppercase _or_ lowercase J or P, you could enter this:

BashCopy

```
ls *.[jpJP]*
```

Expressions in square brackets can represent ranges of characters. For example, the following command lists all the files in the current directory whose names begin with a lowercase letter:

BashCopy

```
ls [a-z]*
```

This command, by contrast, lists all the files in the current directory whose names begin with an uppercase letter:

BashCopy

```
ls [A-Z]*
```

And this one lists all the files in the current directory whose names begin with a lowercase _or_ uppercase letter:

BashCopy

```
ls [a-zA-Z]*
```

Based on all this, can you guess what the following commands do?

BashCopy

```
ls [0-9]*
ls *[0-9]*
ls *[0-9]
```

If you need to use one of the wildcard characters as an ordinary character, you make it literal or "escape it" by prefacing it with a backslash. So, if for some reason you had an asterisk as part of a file name—something you should never do intentionally—you could search for it by using a command such as:

BashCopy

```
$ ls *\**
```

---

# Bash commands and operators

Every shell language has its most-used commands. Let's start building your Bash repertoire by examining the most commonly used commands.

## Bash commands

Let's look at common Bash commands and how to use them.

### `ls` command

`ls` lists the contents of your current directory or the directory specified in an argument to the command. By itself, it lists the files and directories in the current directory:

BashCopy

```
ls
```

Files and directories whose names begin with a period are hidden by default. To include these items in a directory listing, use an `-a` flag:

BashCopy

```
ls -a
```

To get even more information about the files and directories in the current directory, use an `-l` flag:

BashCopy

```
ls -l
```

Here's some sample output from a directory that contains a handful of JPEGs and PNGs and a subdirectory named **gifs**:

OutputCopy

```
-rw-rw-r-- 1 azureuser azureuser  473774 Jun 13 15:38 0001.png
-rw-rw-r-- 1 azureuser azureuser 1557965 Jun 13 14:43 0002.jpg
-rw-rw-r-- 1 azureuser azureuser  473774 Mar 26 09:21 0003.png
-rw-rw-r-- 1 azureuser azureuser 4193680 Jun 13 09:40 0004.jpg
-rw-rw-r-- 1 azureuser azureuser  423325 Jun 10 12:53 0005.jpg
-rw-rw-r-- 1 azureuser azureuser 2278001 Jun 12 04:21 0006.jpg
-rw-rw-r-- 1 azureuser azureuser 1220517 Jun 13 14:44 0007.jpg
drwxrwxr-x 2 azureuser azureuser    4096 Jun 13 20:16 gifs
```

Each line provides detailed information about the corresponding file or directory. That information includes the permissions assigned to it, its owner, its size in bytes, the last time it was modified, and the file or directory name.

### `cat` command

Suppose you want to see what's inside a file. You can use the `cat` command for that. The output won't make much sense unless the file is a text file. The following command shows the contents of the **os-release** file stored in the **/etc** directory:

BashCopy

```
cat /etc/os-release
```

This is a useful command because it tells you which Linux distribution you're running:

OutputCopy

```
NAME="Ubuntu"
VERSION="18.04.2 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04.2 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic
```

The **/etc** directory is a special one in Linux. It contains system-configuration files. You don't want to delete any files from this directory unless you know what you're doing.

### `sudo` command

Some Bash commands can only be run by the root user; a system administrator or superuser. If you try one of these commands without sufficient privileges, it fails. For example, only users logged in as a superuser can use `cat` to display the contents of **/etc/at.deny**:

BashCopy

```
cat /etc/at.deny
```

**at.deny** is a special file that determines who can use other Bash commands to submit jobs for later execution.

You don't want to run as root most of the time; it's too dangerous. To run commands that require admin privilege without logging in as a superuser, you'll preface the commands with `sudo`:

BashCopy

```
sudo cat /etc/at.deny
```

`sudo` stands for "superuser do." When you use it, you're telling the shell that for this one command, you're acting with the root-user level of permission.

### `cd`, `mkdir`, and `rmdir` commands

`cd` stands for "change directory," and it does exactly what the name suggests: it changes the current directory to another directory. It enables you to move from one directory to another just like its counterpart in Windows. The following command changes to a subdirectory of the current directory named **orders**:

BashCopy

```
cd orders
```

You can move up a directory by specifying `..` as the directory name:

BashCopy

```
cd ..
```

This command changes to your home directory; the one you land in when you first sign in:

BashCopy

```
cd ~
```

You can create directories by using the `mkdir` command. The following command creates a subdirectory named **orders** in the current working directory:

BashCopy

```
mkdir orders
```

If you want to create a subdirectory and another subdirectory under it with one command, use the `--parents` flag:

BashCopy

```
mkdir --parents orders/2019
```

The `rmdir` command deletes (removes) a directory, but only if it's empty. If it's not empty, you get a warning instead. Fortunately, you can use the `rm` command to delete directories that aren't empty in combination with the `-r` (recursive) flag. The command would then look like so, `rm -r`.

### `rm` command

The `rm` command is short for "remove." As you'd expect, `rm` deletes files. So this command puts an end to **0001.jpg**:

BashCopy

```
rm 0001.jpg
```

And this command deletes all the files in the current directory:

BashCopy

```
rm *
```

Be wary of `rm`. It's a dangerous command.

Running `rm` with a `-i` flag lets you think before you delete:

BashCopy

```
rm -i *
```

Make it a habit to include `-i` in every `rm` command, and you might avoid falling victim to one of Linux's biggest blunders. The dreaded `rm -rf /` command deletes every file on an entire drive. It works by recursively deleting all the subdirectories of root and their subdirectories. The `-f` (for "force") flag compounds the problem by suppressing prompts. _Don't do this._

If you want to delete a subdirectory named **orders** that isn't empty, you can use the `rm` command this way:

BashCopy

```
rm -r orders
```

This deletes the **orders** subdirectory and everything in it, including other subdirectories.

### `cp` command

The `cp` command copies not just files, but entire directories (and subdirectories) if you want. To make a copy of **0001.jpg** named **0002.jpg**, use this command:

BashCopy

```
cp 0001.jpg 0002.jpg
```

If **0002.jpg** already exists, Bash silently replaces it. That's great if it's what you intended, but not so wonderful if you didn't realize you were about to overwrite the old version.

Fortunately, if you use the `-i` (for "interactive") flag, Bash warns you before deleting existing files. This is safer:

BashCopy

```
cp -i 0001.jpg 0002.jpg
```

You can use wildcards to copy several files at once. To copy all the files in the current directory to a subdirectory named **photos**, do this:

BashCopy

```
cp * photos
```

To copy all the files in a subdirectory named **photos** into a subdirectory named **images**, do this:

BashCopy

```
cp photos/* images
```

This assumes that the **images** directory already exists. If it doesn't, you can create it _and_ copy the contents of the **photos** directory by using this command:

BashCopy

```
cp -r photos images
```

The `-r` stands for "recursive." An added benefit of the `-r` flag is that if **photos** contains subdirectories of its own, they too are copied to the **images** directory.

### `ps` command

The `ps` command gives you a snapshot of all the currently running processes. By itself, with no arguments, it shows all your shell processes; in other words, not much. But it's a different story when you include a `-e` flag:

BashCopy

```
ps -e
```

`-e` lists _all_ running processes, and there are typically many of them.

For a more comprehensive look at what processes are running in the system, use the `-ef` flag:

BashCopy

```
ps -ef 
```

This flag shows the names of all the running processes, their process identification numbers (PIDs), the PIDs of their parents (PPIDs), and when they began (STIME). It also shows what terminal, if any, they're attached to (TTY), how much CPU time they've racked up (TIME), and their full path names. Here's an abbreviated example:

Copy

```
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 13:35 ?        00:00:03 /sbin/init
root          2      0  0 13:35 ?        00:00:00 [kthreadd]
root          3      2  0 13:35 ?        00:00:00 [rcu_gp]
root          4      2  0 13:35 ?        00:00:00 [rcu_par_gp]
root          5      2  0 13:35 ?        00:00:00 [kworker/0:0-cgr]
root          6      2  0 13:35 ?        00:00:00 [kworker/0:0H-kb]
root          8      2  0 13:35 ?        00:00:00 [mm_percpu_wq]
root          9      2  0 13:35 ?        00:00:01 [ksoftirqd/0]
root         10      2  0 13:35 ?        00:00:02 [rcu_sched]
```

As an aside, you might find documentation that shows `ps` being used this way:

BashCopy

```
ps aux
```

`ps aux` and `ps -ef` are the same. This duality traces back to historical differences between POSIX Unix systems (of which Linux is one) and BSD Unix systems (the most common of which is macOS). In the beginning, POSIX used `-ef` while the BSD required `aux`. Today, both operating-system families accept either format.

This serves as an excellent reminder of why you should look closely at the manual for all Linux commands. Learning Bash is like learning English as a second language. There are many exceptions to the rules.

### `w` command

Users come, users go, and sometimes you get users you don't want at all. When an employee leaves to pursue other opportunities, the sysadmin is called upon to ensure that the worker can no longer sign in to the company's computer systems. Sysadmins are also expected to know who's logged in, and who shouldn't be.

To find out who's on your servers, Linux provides the `w` (for "who") command. It displays information about the users currently on the computer system and those users' activities. `w` shows user names, their IP addresses, when they logged in, what processes they're currently running, and how much time those processes are consuming. It's a valuable tool for sysadmins.

## Bash I/O operators

You can do a lot in Linux just by exercising Bash commands and their many options. But you can really get work done when you combine commands by using I/O operators:

- `<` for redirecting input to a source other than the keyboard
- `>` for redirecting output to destination other than the screen
- `>>` for doing the same, but appending rather than overwriting
- `|` for piping output from one command to the input of another

Suppose you want to list everything in the current directory but capture the output in a file named **listing.txt**. The following command does just that:

BashCopy

```
ls > listing.txt
```

If **listing.txt** already exists, it gets overwritten. If you use the `>>` operator instead, the output from `ls` is appended to what's already in **listing.txt**:

BashCopy

```
ls >> listing.txt
```

The piping operator is powerful (and often used). It redirects the output of the first command to the input of the second command. Let's say you use `cat` to display the contents of a large file, but the content scrolls by too quickly for you to read. You can make the output more manageable by piping the results to another command such as `more`. The following command lists all the currently running processes. But once the screen is full, the output pauses until you select **Enter** to show the next line:

BashCopy

```
ps -ef | more
```

You can also pipe output to `head` to see just the first several lines:

BashCopy

```
ps -ef | head
```

Or suppose you want to filter the output to include only the lines that contain the word "daemon." One way to do that is by piping the output from `ps` to Linux's useful `grep` tool:

BashCopy

```
ps -ef | grep daemon
```

The output might look like this:

OutputCopy

```
azureus+  52463  50702  0 23:28 pts/0    00:00:00 grep --color=auto deamon
azureuser@bash-vm:~$ ps -ef | grep daemon
root        449      1  0 13:35 ?        00:00:17 /usr/lib/linux-tools/4.18.0-1018-azure/hv_kvp_daemon -n
root        988      1  0 13:35 ?        00:00:00 /usr/lib/accountsservice/accounts-daemon
message+   1002      1  0 13:35 ?        00:00:00 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
daemon     1035      1  0 13:35 ?        00:00:00 /usr/sbin/atd -f
root       1037      1  0 13:35 ?        00:00:00 /usr/bin/python3 -u /usr/sbin/waagent -daemon
root       1039      1  0 13:35 ?        00:00:00 /usr/lib/linux-tools/4.18.0-1018-azure/hv_vss_daemon -n
azureus+  52477  50702  0 23:28 pts/0    00:00:00 grep --color=auto daemon
```

You can also use files as input. By default, standard input comes from the keyboard, but it too can be redirected. To get input from a file instead of the keyboard, use the `<` operator. One common sysadmin task is to sort the contents of a file. As the name suggests, `sort` sorts text in alphabetical order:

BashCopy

```
sort < file.txt
```

To save the sorted results to a new file, you can redirect input _and_ output:

BashCopy

```
sort < file.txt > sorted_file.txt
```

You can use I/O operators to chain Linux commands as needed. Consider the following command:

BashCopy

```
cat file.txt | fmt | pr | lpr
```

The output from `cat` goes to `fmt`, the output from `fmt` goes to `pr`, and so on. `fmt` formats the results into a tidy paragraph. `pr` paginates the results. And `lpr` sends the paginated output to the printer. All in a single line!

---

# Introduction to PowerShell

PowerShell is a command-line shell and a scripting language all in one. It was designed as a task engine that uses cmdlets to wrap tasks that people need to do. In PowerShell, you can run commands on local or remote machines. You can do tasks like managing users and automating workflows.

Whether you're part of an operations team or a development team that's adopting DevOps principles, PowerShell can help. You can use it to address various tasks, such as managing cloud resources and continuous integration and continuous delivery (CI/CD). PowerShell offers many helpful commands, but you can expand its capabilities at any time by installing modules.

When you install PowerShell, you can evaluate its features to see if it's a good fit for your team.

# What is PowerShell?

PowerShell consists of two parts: a command-line shell and a scripting language. It started out as a framework to automate administrative tasks in Windows. PowerShell has grown into a cross-platform tool that's used for many kinds of tasks.

A command-line shell lacks a graphical interface, where you use a mouse to interact with graphical elements. Instead, you type text commands into a computer console. Here are some of the benefits of using a console:

- Interacting with a console is often faster than using a graphical interface.
- In a console, you can run batches of commands, so it's ideal for task automation for continuous-integration pipelines.
- You can use a console to interact with cloud resources and other resources.
- You can store commands and scripts in a text file and use a source-control system. This capability is probably one of the biggest benefits, because your commands are repeatable and auditable. In many systems, especially government systems, everything must be traced and evaluated, or _audited_. Audits cover everything from database changes to changes done by a script.

## Features

PowerShell shares some features with traditional shells:

- **Built-in help system**: Most shells have some kind of help system, in which you can learn more about a command. For example, you can learn what the command does and what parameters it supports. The help system in PowerShell provides information about commands and also integrates with online help articles.
- **Pipeline**: Traditional shells use a pipeline to run many commands sequentially. The output of one command is the input for the next command. PowerShell implements this concept like traditional shells, but it differs because it operates on objects over text. You learn more about this feature later in this module.
- **Aliases**: Aliases are alternate names that can be used to run commands. PowerShell supports the use of common aliases such as `cls` (clear the screen) and `ls` (list the files). Therefore, new users can use their knowledge of other frameworks and don't necessarily have to remember the PowerShell name for familiar commands.

PowerShell differs from a traditional command-line shell in a few ways:

- **It operates on objects over text.** In a command-line shell, you have to run scripts whose output and input might differ, so you end up spending time formatting the output and extracting the data you need. By contrast, in PowerShell you use objects as input and output. That means you spend less time formatting and extracting.
- **It has cmdlets.** Commands in PowerShell are called cmdlets (pronounced _commandlets_). In PowerShell, cmdlets are built on a common runtime rather than separate executables as they are in many other shell environments. This characteristic provides a consistent experience in parameter parsing and pipeline behavior. Cmdlets typically take object input and return objects. The core cmdlets in PowerShell are built in .NET Core, and are open source. You can extend PowerShell by using more cmdlets, scripts, and functions from the community and other sources, or you can build your own cmdlets in .NET Core or PowerShell.
- **It has many types of commands.** Commands in PowerShell can be native executables, cmdlets, functions, scripts, or aliases. Every command you run belongs to one of these types. The words _command_ and _cmdlet_ are often used interchangeably, because a cmdlet is a type of command.

## Installation

In this module, you practice using PowerShell on your computer. PowerShell is available across platforms. However, if you use a computer that runs Linux, macOS, or an older version of Windows, you need to install it.

Instructions for installing PowerShell are different for each OS. Before you continue, take a few minutes to install PowerShell or to verify your PowerShell installation. The next unit in this module shows you how to verify your installation.

### Windows

If you're running Windows 8 or later, a version of PowerShell called _Windows PowerShell_ should already be installed. This version differs slightly from the most up-to-date PowerShell release, but it works fine for learning purposes.

You can open Windows PowerShell from the Start menu.

### Other operating systems

If your computer runs something other than Windows 8 or later, you need to install PowerShell. To find the installation instructions for your OS, see [Install various versions of PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell).

### PowerShell extension for Visual Studio Code

We recommend that you use the [PowerShell extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) to author your PowerShell scripts and to run the commands in this module. This extension lets you run commands, and also helps you with snippets, code completion, and syntax highlighting.

---

# Locate commands

A _cmdlet_ (pronounced "command-let") is a compiled command. A cmdlet can be developed in .NET or .NET Core and invoked as a command within PowerShell. Thousands of cmdlets are available in your PowerShell installation. The challenge lies in discovering what the cmdlets are and what they can do for you.

Cmdlets are named according to a verb-noun naming standard. This pattern can help you to understand what they do and how to search for them. It also helps cmdlet developers create consistent names. You can see the list of approved verbs by using the `Get-Verb` cmdlet. Verbs are organized according to activity type and function.

Here's a part of the output from running `Get-Verb`:

OutputCopy

```
Verb        AliasPrefix Group          Description
----        ----------- -----          -----------
Add         a           Common         Adds a resource to a container, or atta…
Clear       cl          Common         Removes all the resources from a contai…
```

This listing shows the verb and its description. Cmdlet developers should use an approved verb, and also ensure that the verb description fits their cmdlet's function.

Three core cmdlets allow you to delve into what cmdlets exist and what they do:

- **Get-Command**: The `Get-Command` cmdlet lists all of the available cmdlets on your system. Filter the list to quickly find the command you need.
- **Get-Help**: Run the `Get-Help` core cmdlet to invoke a built-in help system. You can also run an alias `help` command to invoke `Get-Help` but improve the reading experience by paginating the response.
- **Get-Member**: When you call a command, the response is an object that contains many properties. Run the `Get-Member` core cmdlet to drill down into that response and learn more about it.

## Locate commands by using Get-Command

When you run the `Get-Command` cmdlet in Cloud Shell, you get a list of every command that's installed in PowerShell. Because thousands of commands are installed, you need a way to filter the response so you can quickly locate the command that you need.

To filter the list, keep in mind the verb-noun naming standard for cmdlets. For example, in the `Get-Random` command, `Get` is the verb and `Random` is the noun. Use flags to target either the verb or the noun in the command you want. The flag you specify expects a value that's a string. You can add pattern-matching characters to that string to ensure you express that, for example, a flag's value should start or end with a certain string.

These examples show how to use flags to filter a command list:

- **-Noun**: The `-Noun` flag targets the part of the command name that's related to the noun. Here's a typical search for a command name using _alias_ as the noun for which we're searching:
    
    PowerShellCopy
    
    ```
    Get-Command -Noun alias*
    ```
    
    This command searches for all cmdlets whose noun part starts with `alias`.
    
- **-Verb**: The `-Verb` flag targets the part of the command name that's related to the verb. You can combine the `-Noun` flag and the `-Verb` flag to create an even more detailed search query and type. Here's an example:
    
    PowerShellCopy
    
    ```
    Get-Command -Verb Get -Noun alias*
    ```
    
    Now you've narrowed the search to specify that the verb part needs to match `Get`, and the noun part needs to match `alias`.

---

