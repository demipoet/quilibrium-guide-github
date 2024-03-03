# Beginner’s Guide - How to Setup a Quilibrium CeremonyClient node - Dawn

## Secure your Node hardware (VPS)

### Get a VPS running Ubuntu 22.04

#### My recommendations

Hostinger.com (My referral link)
I recommend getting the KVM8 for Dawn
In USD - KVM 8 costs $18.30/mo 

### After buying, go to https://hpanel.hostinger.com/servers/plans 
Select KVM 8 only. KVM 2 will not work for the Quilibrium Dawn testnet looks to be especially different from other networks so this requires KVM 8 😉 <br /> It requires if VPS at least 8vCPU Cores and 16GB  RAM

> 8 vCPU Core<br />
> 32 GB RAM<br />
> 400 GB NVMe Disk Space<br />
> 32 TB Bandwidth<br />
> 1 Snapshot<br />
> Weekly Backups<br />

### You can select a plan duration (monthly/yearly) after selecting KVM 8 
Then link your card to pay or pay with crypto <br />
Note that the discounted rate may only be available on the yearly plan<br />
Then follow installation instructions<br />

### Connect to your VPS 
Downloading and install PuTTY: http://putty.org/  or just use your Terminal if you are running MacOS or Linux <br />
Put your IP address into terminal client & click open Click "Accept" Type "root" & click enter Type your password & click enter <br />
SSH into your VPS <br />

## SSH for the first time to your new VPS
To SSH into a VPS, run:
```
ssh root@<ip_address_of_VPS>
```
