# HPC-setup
Steps for the HPC setup


here are steps to setup HPC on your local machines. there are some steps that are different to access the clustor. the below stes and commands have to run on the local system for the setup:-



This guide describes how to use OpenSSH to connect to the HPC systems. OpenSSH is supported under Linux, Mac, and Windows.

Reporting SSH connection issues

When you experience connection issues check Troubleshooting first.

If you cannot resolve the issue yourself, contact us under hpc-support@fau.de and include the invocation and the output of your command with the flags -vv added, like:

ssh -vv <your SSH options and arguments>
Generating an SSH key pair#
For connecting to the HPC systems an SSH key pair is required. For each machine you are using to connect to our systems this step has to be repeated.

You can create an SSH key pair by executing:

ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_nhr_fau
(Note: Accepted key formats are RSA (at least 4096 bit), ECDSA (at least 512 bit), ED25519)
You will be asked to enter a passphrase. YOU ARE REQUIRED TO USE A STRONG PASSPHRASE

The filename after the -f flag can be freely chosen. If you use a different filename you have to use it instead throughout this guide. Typically, SSH key pairs reside under the ~/.ssh/ directory.

After successful execution, two keys are created:

A private key ~/.ssh/id_ed25519_nhr_fau
Must be kept secret and local to the machine.
A public key ~/.ssh/id_ed25519_nhr_fau.pub
Has to be uploaded to the HPC portal.
Example of generating an SSH key pair
Upload SSH public key to the HPC Portal#
Follow the instruction on uploading an SSH public key to the HPC-Portal.

Configuring connection settings#
For connecting to our systems we provide a template you can add to your local ~/.ssh/config. Please copy the whole template even if you do not need access to all clusters.

With the template connections to our systems automatically use the previously generated private SSH key and connect to the clusters through our dialog server csnhr.nhr.fau.de by performing a so called proxy jump.

Before usage the templates must be adjusted:

Replace <HPC ACCOUNT> by your HPC account, as listed in the HPC portal under Your accounts > Active accounts, not your SSO identity.
Change the path to the private SSH key at IdentityFile, if you did not name your key ~/.ssh/id_ed25519_nhr_fau.
If your config file contains ProxyJump csnhr.nhr.fau.de in one of the sections, make sure to add the entry for Host csnhr.nhr.fau.de, too.
Aliases for hostnames#
Do not shorten the hostnames used in the templates below, like replacing csnhr.nhr.fau.de with csnhr. Add an alias instead.
Aliases are appended to the line starting with Host. For example, to use csnhr as an alias for csnhr.nhr.fau.de the original line Host csnhr.nhr.fau.de from the templates below is changed to Host csnhr.nhr.fau.de csnhr. You then can use ssh csnhr and ssh csnhr.nhr.fau.de interchangeably.

Template for connecting to HPC systems#
Template to adjust and add to your local ~/.ssh/config.
Host csnhr.nhr.fau.de csnhr
    HostName csnhr.nhr.fau.de
    User <HPC account>
    IdentityFile ~/.ssh/id_ed25519_nhr_fau
    IdentitiesOnly yes
    PasswordAuthentication no
    PreferredAuthentications publickey
    ForwardX11 no
    ForwardX11Trusted no

Host fritz.nhr.fau.de fritz
    HostName fritz.nhr.fau.de
    User <HPC account>
    ProxyJump csnhr.nhr.fau.de
    IdentityFile ~/.ssh/id_ed25519_nhr_fau
    IdentitiesOnly yes
    PasswordAuthentication no
    PreferredAuthentications publickey
    ForwardX11 no
    ForwardX11Trusted no

Host alex.nhr.fau.de alex
    HostName alex.nhr.fau.de
    User <HPC account>
    ProxyJump csnhr.nhr.fau.de
    IdentityFile ~/.ssh/id_ed25519_nhr_fau
    IdentitiesOnly yes
    PasswordAuthentication no
    PreferredAuthentications publickey
    ForwardX11 no
    ForwardX11Trusted no

# needed only for Tier3-Grundversorgung
Host tinyx.nhr.fau.de tinyx
    HostName tinyx.nhr.fau.de
    User <HPC account>
    ProxyJump csnhr.nhr.fau.de
    IdentityFile ~/.ssh/id_ed25519_nhr_fau
    IdentitiesOnly yes
    PasswordAuthentication no
    PreferredAuthentications publickey
    ForwardX11 no
    ForwardX11Trusted no

# needed only for Tier3-Grundversorgung
Host woody.nhr.fau.de woody
    HostName woody.nhr.fau.de
    User <HPC account>
    ProxyJump csnhr.nhr.fau.de
    IdentityFile ~/.ssh/id_ed25519_nhr_fau
    IdentitiesOnly yes
    PasswordAuthentication no
    PreferredAuthentications publickey
    ForwardX11 no
    ForwardX11Trusted no

# needed only for Tier3-Grundversorgung
Host meggie.rrze.fau.de meggie.rrze.uni-erlangen.de meggie
    HostName meggie.rrze.uni-erlangen.de
    User <HPC account>
    ProxyJump csnhr.nhr.fau.de
    IdentityFile ~/.ssh/id_ed25519_nhr_fau
    IdentitiesOnly yes
    PasswordAuthentication no
    PreferredAuthentications publickey
    ForwardX11 no
    ForwardX11Trusted no
Explanation:

setting	description
Host ...	The hostname to apply the following configuration for.
HostName ...	The actual hostname to use for this connection.
User <HPC account>	The HPC account to use. Specify the account name as found in the HPC portal.
PreferredAuthentication publickey	For authentication prefer public key.
PasswordAuthentication no	Do not use password authentication.
IdentityFile ...	Use the specified SSH private key.
IdentitiesOnly yes	Use only the specified SSH key, even if, e.g. ssh-agent, could provide more SSH keys.
ProxyJump csnhr.nhr.fau.de	Use proxy jump to connect to csnhr.nhr.fau.de first and from there connect to cluster frontend.
Template for connecting to cluster nodes#
This is an optional template that is rarely needed. It is only useful, if you have to connect to the cluster nodes directly from your local machine, e.g., when you need to create a port forwarding to a remote Jupyter Notebook.

For the template to work you have to:

add the template for connecting to HPC systems to your ~/.ssh/config
use the fully qualified domain name (FQDN) of a cluster node, like a0123.nhr.fau.de and not a0123.
Template for your .ssh/config to connect to cluster nodes
Testing the connection#
Test if your configuration works by

Connecting to csnhr.nhr.fau.de first:

ssh csnhr.nhr.fau.de
In case it is your first connection to csnhr.nhr.fau.de you will see the following message and be prompted to verify the authenticity of the server:

The authenticity of host 'csnhr.nhr.fau.de (2001:638:a000:1001::83bc:327)' can't be established.
ED25519 key fingerprint is SHA256:ZbsDL9koTZ48+trswAeqvjVGZyv/kBUtOjodEN4XMCo.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])?
See Host key fingerprints for the fingerprints of our cluster front ends.

Connecting to one of the cluster frontend nodes by executing one of the following commands:

Fritz: ssh fritz.nhr.fau.de
Alex: ssh alex.nhr.fau.de
TinyGPU/TinyFat: ssh tinyx.nhr.fau.de
Woody: ssh woody.nhr.fau.de
Not all accounts have access to all clusters.

In case it is your first connection to the cluster frontend you will see a similar message as above for csnhr.nhr.fau.de for your selected frontend and be prompted to verify the authenticity of the server.

See Host key fingerprints on how to obtain the fingerprints of the host keys in advance.

Setup port forwarding to a cluster frontend or node#
A port forwarding allows you to access a remote port on a cluster frontend or node directly from your local computer. The tunnel between your computer and the remote one is created by the ssh command.

This is typically needed if you remotely start an application that has a web interface you want to access through your computer's browser, like it is the case with Jupyter Notebooks.

For this setup you need:

If not already done, configure your SSH connection with the correct template, either for the cluster frontends or nodes.

Determine the fully qualified domain name (FQDN) of the of the cluster frontend or node you want to access, further on called remote server:

hostname
# output can look like one of the following lines:
# <name>.nhr.fau.de  
# <name>.rrze.fau.de
# <name>.rrze.uni-erlangen.de
Determine the remote port on the remote server you want to access.

On your computer setup the port forwarding. This makes the remote port of the remote server under the <local port> accessible:

ssh -L <local port>:localhost:<remote port> <remote server>
<local port> must be replaced by a port number from the range of 1025 and 65535 that is currently unused on your local system.
<remote port> has to be replaced by the remote port from step 3.
<remote server> has to be replaced by FQDN of the remote server from step 2.
The command opens a new SSH connection and establishes the port forwarding in the background.

If the port forwarding is now longer needed, you can exit the SSH connection.

Access Jupyter Notebook on cluster node.

Assuming you have a Jupyter Notebook running on the cluster node a123.nhr.fau.de under the port 54321 and want to access it from your computer's browser on port 12345 (which you can freely choose), then the command from step 4 will look like:

ssh -L 12345:localhost:54321 a123.nhr.fau.de
After that, you can access the Jupyter Notebook in your local browser under http://localhost:12345.
SSH-agent#
Since you set a passphrase for your private SSH key, you will be prompted to enter the passphrase every time you use the key to connect to a remote host. To avoid this, you can use an SSH agent to store your private key for the duration of your session.

If you are using a current Linux distribution with a graphical desktop session (Unity, GNOME,...), an SSH agent will be started automatically in the background. Your private keys in ~/.ssh will be stored automatically and used when connecting to a remote host.

Troubleshooting#
Enable debug output#
The get more information on SSH problems, add the -v or even -vv option to your ssh command. This will output moderate debug information, e.g. show which SSH keys are tried:

ssh -v <your usual SSH options and arguments>
Compare fingerprints of your SSH key#
Check if the fingerprint of the SSH key you are using to connect to our systems, matches the fingerprint in the HPC Portal.

Make sure you compare the correct format of the fingerprint, MD5 vs SHA256. Both are listed in the portal.

Replace ~/.ssh/id_ed25519_nhr_fau with the path to the key you are using:

Obtain MD5 fingerprint of your SSH key:
max@notebook:~$ ssh-keygen -E MD5 -l -f ~/.ssh/id_ed25519_nhr_fau
256 MD5:24:11:32:31:3b:4a:ec:43:ba:96:ab:73:2b:a6:af:68 max@notebook (ED25519)
Obtain SHA256 fingerprint of your SSH key:
max@notebook:~$ ssh-keygen -E SHA256 -l -f ~/.ssh/id_ed25519_nhr_fau
256 SHA256:mWO4eYar1/JYn8MDB0DPer+ibB/QatmhxvvngfaoMgQ max@notebook (ED25519)
Fixing error agent refused operation#
The error message

sign_and_send_pubkey: signing failed ... agent refused operation
typically means that you entered a wrong passphrase for the SSH key.
You might have to enable debug output to see this error. Add the option -v to your ssh command. The output might contain parts that look like the following:

...
debug1: Authentications that can continue: publickey,password
debug1: Next authentication method: publickey
debug1: Offering public key: /home/max/.ssh/id_rsa RSA SHA256:xCyJUQcsJldPWfZXSasoI0ZCoteKWHw1e95ylm2HK1g agent
debug1: Server accepts key: /home/max/.ssh/id_rsa RSA SHA256:xCyJUQcsJldPWfZXSasoI0ZCoteKWHw1e95ylm2HK1g agent
sign_and_send_pubkey: signing failed for RSA "/home/max/.ssh/id_rsa" from agent: agent refused operation
debug1: Next authentication method: password





Lastly, one can save the settinga nd cod on the cloud also. there has been changes with time. the linux is best for the HPC setup.

with the help of linux a user can setup its HPC environment. Also there are different procedures and ways to to setup the environment. A institudes or organiston' s site is helpfull.

kindly find the given below screenshoot:
![image](https://github.com/user-attachments/assets/4ed7555d-40bb-4a53-971a-ee81e0183852)




...
