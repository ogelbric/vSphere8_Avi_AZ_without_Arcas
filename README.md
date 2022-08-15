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

## Nested deploy of lab via William Lams Power Shell Script

The script modified for 3 vCenter clusters and 9 ESXi hosts

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

### Before we start current picture of vCenter and its Zones 1-3

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcpenab1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp3.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp4.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp5.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp6.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp7.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp8.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp9.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp10.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp11.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp12.png)

### Observe the deployment

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp13.png)

### Observe the deployment of the supervisor VM's into 3 different vCenter clusters

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp14.png)

## Trouble shooting

```
The 3 Supervisor VM's have 4-4-5 IP's - that is correct

Logging onto vCenter to jump to Supervisor to check on AKO
ssh root@192.168.1.50 # my vcenter
shell
/usr/lib/vmware-wcp/decryptK8Pwd.py
ssh root@192.168.1.80 # and use password from above command

k get pods -A  # looks like AKO is in some crash loop
k logs -n vmware-system-ako vmware-system-ako-ako-controller-manager-58fbd65b89-2hzpk # and the log tells me my IPAM has issues on my cloud...

2022-08-11T17:41:36.360Z        INFO    cache/controller_obj_cache.go:2633      Skipping the check for SE group labels 
2022-08-11T17:41:36.360Z        ERROR   k8s/ako_init.go:274     Error while validating input: Cloud does not have a ipam_provider_ref configured
2022-08-11T17:41:36.360Z        ERROR   ako-main/main.go:228    Handle configmap error during reboot, shutting down AKO. 

Looking at AVI the default cloud sure thing it "forgot" about my front end network IPAM 

k get pods -A |grep -i ako # get the pods again
k delete -n vmware-system-ako pod vmware-system-ako-ako-controller-manager-58fbd65b89-2hzpk # delete the POD 
k get pods -A |grep -i ako # get the new pod name and it is running now=
k logs -n vmware-system-ako  vmware-system-ako-ako-controller-manager-58fbd65b89-nc8px -f. #follow the log looks like it is working now

```

### AVI SE's are deploying

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp15.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/avisecreating1.png)


### Things are starting to look green

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcp16.png)

## WCP - Create Namespace and Permissions

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcpns1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcpns2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcpnsperm1.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcpnsperm2.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcpnsperm3.png)

![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcpnsperm4.png)


### Log onto supervisor API end point 
```
/usr/local/bin/kubectl-vsphere login --vsphere-username administrator@vsphere.local --server=https://192.168.4.70 --insecure-skip-tls-verify
kubectl config use-context namespace1000
```

### Create Workload cluster accross 3 zones

https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/tkrcluster.yaml

```
k apply -f ./tkrcluster.yaml

[root@centosrouter 8u0]# k get tkc
NAME                  CONTROL PLANE   WORKER   TKR NAME                           AGE    READY   TKR COMPATIBLE   UPDATES AVAILABLE
tkr-zoned-cluster01   3               3        v1.22.9---vmware.1-tkg.1.cc71bc8   5m9s   False   True             
[root@centosrouter 8u0]# k get tkr
NAME                                VERSION                           READY   COMPATIBLE   CREATED
v1.16.12---vmware.1-tkg.1.da7afe7   v1.16.12+vmware.1-tkg.1.da7afe7   False   False        92m
v1.16.14---vmware.1-tkg.1.ada4837   v1.16.14+vmware.1-tkg.1.ada4837   False   False        96m
v1.16.8---vmware.1-tkg.3.60d2ffd    v1.16.8+vmware.1-tkg.3.60d2ffd    False   False        95m
v1.17.11---vmware.1-tkg.1.15f1e18   v1.17.11+vmware.1-tkg.1.15f1e18   False   False        98m
v1.17.11---vmware.1-tkg.2.ad3d374   v1.17.11+vmware.1-tkg.2.ad3d374   False   False        95m
v1.17.13---vmware.1-tkg.2.2c133ed   v1.17.13+vmware.1-tkg.2.2c133ed   False   False        98m
v1.17.17---vmware.1-tkg.1.d44d45a   v1.17.17+vmware.1-tkg.1.d44d45a   False   False        98m
v1.17.7---vmware.1-tkg.1.154236c    v1.17.7+vmware.1-tkg.1.154236c    False   False        94m
v1.17.8---vmware.1-tkg.1.5417466    v1.17.8+vmware.1-tkg.1.5417466    False   False        92m
v1.18.10---vmware.1-tkg.1.3a6cd48   v1.18.10+vmware.1-tkg.1.3a6cd48   False   False        96m
v1.18.15---vmware.1-tkg.1.600e412   v1.18.15+vmware.1-tkg.1.600e412   False   False        97m
v1.18.15---vmware.1-tkg.2.ebf6117   v1.18.15+vmware.1-tkg.2.ebf6117   False   False        98m
v1.18.19---vmware.1-tkg.1.17af790   v1.18.19+vmware.1-tkg.1.17af790   False   False        91m
v1.18.5---vmware.1-tkg.1.c40d30d    v1.18.5+vmware.1-tkg.1.c40d30d    False   False        96m
v1.19.11---vmware.1-tkg.1.9d9b236   v1.19.11+vmware.1-tkg.1.9d9b236   False   False        98m
v1.19.14---vmware.1-tkg.1.8753786   v1.19.14+vmware.1-tkg.1.8753786   False   False        94m
v1.19.16---vmware.1-tkg.1.df910e2   v1.19.16+vmware.1-tkg.1.df910e2   False   False        98m
v1.19.7---vmware.1-tkg.1.fc82c41    v1.19.7+vmware.1-tkg.1.fc82c41    False   False        94m
v1.19.7---vmware.1-tkg.2.f52f85a    v1.19.7+vmware.1-tkg.2.f52f85a    False   False        95m
v1.20.12---vmware.1-tkg.1.b9a42f3   v1.20.12+vmware.1-tkg.1.b9a42f3   True    True         94m
v1.20.2---vmware.1-tkg.1.1d4f79a    v1.20.2+vmware.1-tkg.1.1d4f79a    True    True         92m
v1.20.2---vmware.1-tkg.2.3e10706    v1.20.2+vmware.1-tkg.2.3e10706    True    True         97m
v1.20.7---vmware.1-tkg.1.7fb9067    v1.20.7+vmware.1-tkg.1.7fb9067    True    True         95m
v1.20.8---vmware.1-tkg.2            v1.20.8+vmware.1-tkg.2            True    True         96m
v1.20.9---vmware.1-tkg.1.a4cee5b    v1.20.9+vmware.1-tkg.1.a4cee5b    True    True         94m
v1.21.2---vmware.1-tkg.1.ee25d55    v1.21.2+vmware.1-tkg.1.ee25d55    True    True         98m
v1.21.6---vmware.1-tkg.1            v1.21.6+vmware.1-tkg.1            True    True         98m
v1.21.6---vmware.1-tkg.1.b3d708a    v1.21.6+vmware.1-tkg.1.b3d708a    True    True         97m
v1.22.9---vmware.1-tkg.1.cc71bc8    v1.22.9+vmware.1-tkg.1.cc71bc8    True    True         94m
```

### Cluster is creating 
![Version](https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/wcpwlcluster1.png)

```
Log onto workload cluster
kubectl vsphere login --server 192.168.4.70 --vsphere-username administrator@vsphere.local --managed-cluster-namespace namespace1000 --managed-cluster-name tkr-zoned-cluster01 --insecure-skip-tls-verify
kubectl config use-context tkr-zoned-cluster01

k get nodes
NAME                                                      STATUS   ROLES                  AGE    VERSION
tkr-zoned-cluster01-workerpool-1-ddn8z-d58f8694f-5bjct    Ready    <none>                 173m   v1.22.9+vmware.1
tkr-zoned-cluster01-workerpool-2-4r728-68dd78b9-bszlt     Ready    <none>                 167m   v1.22.9+vmware.1
tkr-zoned-cluster01-workerpool-3-89dcs-7576c89bb5-5xfhd   Ready    <none>                 167m   v1.22.9+vmware.1
tkr-zoned-cluster01-xfvvq-gwgnj                           Ready    control-plane,master   163m   v1.22.9+vmware.1
tkr-zoned-cluster01-xfvvq-w78mh                           Ready    control-plane,master   148m   v1.22.9+vmware.1
tkr-zoned-cluster01-xfvvq-wscsf                           Ready    control-plane,master   3h4m   v1.22.9+vmware.1

```
### Deploy sample cluster Auth

https://github.com/ogelbric/vSphere8_Avi_AZ_without_Arcas/blob/main/authorize-psp-for-gc-service-accounts.yaml

### Deploy sample 3 POD nginx 


...
