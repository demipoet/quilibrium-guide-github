 <link rel="shortcut icon" type="image/x-icon" href="favicon.png">

# Beginner’s Guide - How to Setup a Quilibrium CeremonyClient node - Dawn

## Secure your Node hardware (VPS)

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

## SSH for the first time to your new VPS
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

## Prerequisite software

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

## Configure Linux network device settings

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
## Clone the Quilibrium CeremonyClient Repository
Run:
```
cd ~
git  clone  https://github.com/QuilibriumNetwork/ceremonyclient.git
```
Go to the ceremonyclient/node folder
```
cd ceremonyclient/node
```
## Import your voucher hex
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

