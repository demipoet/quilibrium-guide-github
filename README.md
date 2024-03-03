 <link rel="shortcut icon" type="image/x-icon" href="favicon.png">

# Beginners' Guide - How to Setup a Quilibrium CeremonyClient node
Created by Demipoet. Let's connect at [Farcaster](https://warpcast.com/demipoet)! ... Don't have Farcaster account yet? Create your FC account [here](https://warpcast.com/~/invite-page/272717?id=1f9087d2).<br /><br />
Current Version: 1.14.0 (Sunset) as of March 3, 2024<br />
Want to refer to the old PDF Guide: [link](https://drive.google.com/file/d/1atQ2Gb8vLzqxiS2cqRAp9ojFNDJup3TU/view?usp=sharing)<br />

## Table of Contents
I. [Secure your Node hardware (VPS)](#i-secure-your-node-hardware-vps)<br />
II. [SSH for the first time to your new VPS](#ii-ssh-for-the-first-time-to-your-new-vps)<br />
III. [Prerequisite software](#iii-prerequisite-software)<br />
IV. [Configure Linux network device settings](#iv-configure-linux-network-device-settings)<br />
V. [Clone the Quilibrium CeremonyClient Repository](#v-clone-the-quilibrium-ceremonyclient-repository)<br />
VI. [Import your Voucher Hex](#vi-import-your-voucher-hex-optional)<br />
VIII. [Configure your config.yml](#viii-configure-your-configyml)<br />

## I. Secure your Node hardware (VPS)
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br />
### 1. Get a VPS running Ubuntu 22.04

#### My recommendations

Hostinger.com ([My referral link](https://hostinger.ph?REFERRALCODE=1DEMICINCO282))
I recommend getting the KVM8 for Dawn
In USD - KVM 8 costs $18.30/mo 

### 2. After buying, go to [Hostinger VPS Plans](https://hpanel.hostinger.com/servers/plans)
Select KVM 8 only. KVM 2 will not work for the Quilibrium Dawn testnet looks to be especially different from other networks so this requires KVM 8 😉 <br /> It requires if VPS at least 8vCPU Cores and 16GB  RAM

> 8 vCPU Core<br />
> 32 GB RAM<br />
> 400 GB NVMe Disk Space<br />
> 32 TB Bandwidth<br />
> 1 Snapshot<br />
> Weekly Backups<br />

### 3. You can select a plan duration (monthly/yearly) after selecting KVM 8 
Then link your card to pay or pay with crypto <br />
Note that the discounted rate may only be available on the yearly plan<br />
Then follow installation instructions<br />

### 4. Connect to your VPS 
Downloading and install PuTTY: http://putty.org/  or just use your Terminal if you are running MacOS or Linux <br />
Put your IP address into terminal client & click open Click "Accept" Type "root" & click enter Type your password & click enter <br />
SSH into your VPS <br />

## II. SSH for the first time to your new VPS
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br /><br />
To SSH into a VPS, run:
```
ssh root@<ip_address_of_VPS>
```
> The authenticity of host (<ip_address>) can't be established.<br />
> ED25519 key fingerprint is SHA256:xxxxxxx<br />
> This key is not known by another names<br />
> Are you sure you want to continue connecting (yes/no/\[fingerprint\])?

```
yes
```

> Warning: Permanently added '<ip_address>' (ED25519) to the list of known hosts.
> Connection closed by <ip_address> port 22.

Enter the root password
SSH into your VPS again:
```
ssh root@<ip_address_of_VPS>
```
Enter the root password.

## III. Prerequisite software
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br />
### 1. Use root user for this installation
Make sure you are using the root user as this guide is tailored 

### 1. Update 
Run apt update to fetch the latest version of the package list from Ubuntu's software repository
```
sudo apt -q update
```

### 2. Install Git

```
sudo  apt  install  git  -y
```
```
git --version
```
It must show:
> git version 2.34.1

### 3. Install GO

The release right now (as of 1.2.14) does not support its project builds with go1.21.x nor go1.19.x. It has to be go1.20.x.

#### Download the distribution source
```
wget  https://go.dev/dl/go1.20.14.linux-amd64.tar.gz
```
OR
```
wget  https://go.dev/dl/go1.20.14.linux-arm64.tar.gz
```
The project support both AMD and ARM.
#### Extract the zipped (gz) file
```
sudo tar -xvf go1.20.14.linux-amd64.tar.gz 
```
#### Move the GO folder to usr folder
```
sudo  mv  go  /usr/local
```
#### Delete (remove) the downloaded GO zipped file
```
sudo  rm  go1.20.14.linux-amd64.tar.gz
```
#### Permanently set GO environment variables
```
sudo vim  ~/.bashrc
```
Press `i` to start inserting text into ~/.bashrc file<br />
Scroll until the end of the file, and insert the following in new lines at the end:<br />
```
GOROOT=/usr/local/go
GOPATH=$HOME/go
PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```
Press `esc` to stop the insert-text mode<br />
Press `shift` + `:wq`, and press `enter` or `return` on the keyboard<br />
Run:
```
source  ~/.bashrc
go version
```
It must show:
> go version go1.20.14 linux/amd64

## IV. Configure Linux network device settings
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br /><br />
Purpose: to optimize throughput (bandwidth) and latency for large parallel jobs typical of networks like Q<br />

Run:
```
sudo vim  /etc/sysctl.conf
```
Press `i` to start inserting text into /etc/sysctl.conf file<br />
Scroll until the end of the file, and insert the following in new lines at the end:<br />
```
# Increase buffer sizes for better network performance
net.core.rmem_max=600000000
net.core.wmem_max=600000000
```
Press `esc` to stop the insert-text mode<br />
Press `shift` + `:wq`, and press `enter` or `return` on the keyboard<br />
Run:
```
sudo  sysctl  -p
```
Reboot your VPS
```
reboot
```
You will be disconnected from your VPS as the VPS reboots<br />
SSH into your VPS again:
```
ssh root@<ip_address_of_VPS>
```
Enter the root password<br />

## V. Clone the Quilibrium CeremonyClient Repository
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br /><br />
Run:
```
cd ~
git  clone  https://github.com/QuilibriumNetwork/ceremonyclient.git
```
Go to the ceremonyclient/node folder
```
cd ceremonyclient/node
```
## VI. Import your voucher hex (optional)
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br /><br />
<b>Note:</b> Only applicable for those who has an offline voucher (from April 2023 offline declaration for the Freedom of Internet ceremony).<br />
Run:
```
sudo vim /root/voucher.hex
```
Press `i` to start inserting text into voucher.hex file<br />
Copy-paste the 228-character voucher hex into the file<br />
Press `esc` to stop the insert-text mode<br />
Press `shift` + `:wq`, and press `enter` or `return` on the keyboard<br />
Run:
```
GOEXPERIMENT=arenas  go  run  ./...  -import-priv-key  `cat /root/voucher.hex`
```
Take note of your Peer ID. It will be one of the last lines in the response, starts with 'Qm' with a label Peer ID.

## VII. Configure your Node Network Firewall
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)<br /><br />
Run:
```
sudo ufw enable
```
Type `y` and press `enter` or `return` on keyboard
Run:
```shell
sudo ufw allow 22
sudo ufw allow 8336
sudo ufw allow 443
sudo ufw status
```

Response for the status command should be:
> To            Action            From
> --            ------            -----
> 22            ALLOW             Anywhere
> 8336          ALLOW             Anywhere
> 443           ALLOW             Anywhere
> 22 (v6)       ALLOW             Anywhere (v6)
> 8336 (v6)     ALLOW             Anywhere (v6)
> 443 (v6)      ALLOW             Anywhere (v6)


## VIII. Configure your config.yml
[Return to top](#beginners-guide---how-to-setup-a-quilibrium-ceremonyclient-node)
### Enable gRPC to enable gRPC Function Calls for your Node

<b>Note</b>: This interface, while read-only, is unauthenticated and not rate-limited. It is recommended that you only enable them if you are properly controlling access via firewall or only query via localhost (i.e. if port 8337 is used for gRPC calls, best not to allow it on your firewall configuration later and only trigger gRPC calls on localhost).<br/><br/>

Go to ceremonyclient/node folder. <br/>
```
cd ~/ceremonyclient/node
```
Run:
```shell
sudo vim  .config/config.yml
```
Press `i` to start inserting text into config.yml file<br />
On the line, right about the end of file, there is a field `listenGrpcMultiaddr: “”`, replace it with 
```shell
listenGrpcMultiaddr: /ip4/127.0.0.1/tcp/8337
```




