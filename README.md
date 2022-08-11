# vCenter 8 with AVI (DVS) and multi AZ (Zones/vCenter Clusters) without Arcas (Service Installer)

Versions used in this nested environment:
```
Base System ESXi 6.7u3
Base vCenter 7.0.3
Nested ESXi 7.0.3d
Nested vCenter 8.0.0 Build 20183400 (some sort of alpha/beta version) (sorry internal right now only on build web)
AVI 22.1.1-9052 (https://portal.avipulse.vmware.com/software/vantage)
```

## What is covered in this write up:
```
The nested test environment is built via William Lam (https://williamlam.com/nested-virtualization) Power Shell script
  Deploys one vCenter
  Deploys 9 ESXi server into 3 vCenter clusters with vSAN and Networks
Deploy AVI by hand
AVI set up 
  Done by hand 
WCP enablement 
  Done by hand
Workload cluster
  Done by hand 
```

## Network layout
```
DNS = "10.197.96.7"
NTP = "10.128.152.81"

AVI Management Network "192.168.1.1/24" "192.168.1.60-192.168.70"
AVI Frontend Network "192.168.4.1/24" "192.168.4.70-192.168.4.100"
AVI Route workload to frontend network 192.168.5.0/24 -> 192.168.4.1

WCP Management Network 192.168.1.80 <--- Start
WCP Workload Network "192.168.5.1/24" "192.168.5.120-192.168.5.140"

vCenter = 192.168.1.50
AVI = 192.168.1.40
ESXi = See William Lams script
```

## The Nested deploy via William Lams Power Shell Script

Here is the script modified for 3 vCenter clusters and 9 ESXi hosts

  https://github.com/ogelbric/vSphere8_Avi_AZ/blob/main/PSAutomationScript


Script kick off: 

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ/blob/main/kickoff1.png)
![Version](https://github.com/ogelbric/vSphere8_Avi_AZ/blob/main/Kickoff2.png)

Result:

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ/blob/main/vCenterOutcome1.png)

### Make sure datastores are tagged

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ/blob/main/tag1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ/blob/main/tag2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ/blob/main/tag2.png)

### Make sure Storage police has consumption / zonal enabled

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/con1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/con2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/con3.png)

### Make sure zones for the 3 vCenter clusters is enabled

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/zone1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/zone2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/zone3.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/zone4.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/zone5.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/zone6.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/zone7.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/zone8.png)

### Makre sure the is a content Lib (empty) for the AVI SE's (new) (AVI will push SE's to the lib from the cloud setup

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/contentlib1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/contentlib2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/contentlib3.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/contentlib4.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/contentlib5.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/contentlib6.png)

### Makre sure the is a content Lib for the k8 images (subscribed)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/k8lib1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/k8lib2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/k8lib3.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/k8lib4.png)

## Deploy and set up AVI

### Deploy AVI OVA (Management Network 192.168.1.40)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/depavi1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/depavi2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/depavi3.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/depavi4.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/depavi5.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/adepavi6.png)


### Set up AVI

#### Set Up AVI admin account / DNS

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avilic1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avilic2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avilic3.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avilic4.png)

#### Set Up AVI License (new)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avilic5.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avilic6.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avilic7.png)

#### Set Up AVI - Convert default Cloud to vCenter (new)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avidefcl1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avidefcl2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avidefcl3.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avidefcl4.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avidefcl5.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avidefcl6.png)

#### Set Up AVI - Networks

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avinet1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avinet2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avinet3.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avinet4.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avinet5.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avinet6.png)

#### Set Up AVI - Route (new location)

Route from 192.168.5.0/24 (workload network to frontent) -> 192.168.4.1

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avirout1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avirout2.png)

#### Set Up AVI - Default Cloud is green (FYI the AKO POD in TKGs/u is lookign for the DefaultCloud in AVI!)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/aviclgreen1.png)


#### Set Up AVI - Cert 

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avicert1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avicert2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avicert3.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avicert4.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avicert5.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avicert6.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avicert7.png)

#### Set Up AVI - Cert Location

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avicertloc1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avicertloc2.png)

#### Set Up AVI - Service Engine (SE) group needs the zone clusters to deploy to

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avisegroup1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avisegroup2.png)


## Set up WCP enablement 

## Trouble shooting

..
