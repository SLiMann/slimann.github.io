---
layout: post
title: "vRSLCM Patched Product Binaries"
description: "Map vRA 8.1 patch binary in vRSLCM 8.1"
date: 2020-09-01
tags: vrslcm binary patch scale-out
---
# vRSLCM Scale-Out Patched Product Binaries

vRSLCM 8.1 resolves the issue of scaling out a patched product in vRealize Suite Lifecycle Manager with the introduction of patched product binaries. For example, When a single node vRA instance is deployed and then patched in a day-2 operation from vRSLCM, it was previously not possiable to join another node to the cluster. This was a result of the way patches had been delivered.

VMware has begun releasing patches + the product binary slipstreamed into a single file called a patched product binary.

To download a patched product binary, we first need to login to the MyVMware Prodcut Patches site. This site is also helpfull when vRSLCM is not connected to the internet, since all of the patches are posted here and may be import to vRSLCM.

---

> * Product Patches - [https://my.vmware.com/group/vmware/patch#search](https://my.vmware.com/group/vmware/patch#search)

---

Select "vRealize Suite Lifecycle Manager" in the Select a Product menu. Then select the corresponding version of vRA (VRA-8.1.0) from the Select Version menu and click Search under the filters.

[![Product Patches](/assets/images/vRSLCM Scale-Out Patched Product Binaries.png "Select "vRealize Suite Lifecycle Manager" and the corresponding version of vRA (VRA-8.1.0)")](https://my.vmware.com/group/vmware/patch#search)

Then select the newest vrlcm-vra-8.x.x-PatchX ovabundle to download. Patches are culumative, so we always want to apply the newest.

[![Product Patches](/assets/images/vRSLCM Scale-Out Patched Product Binaries vRA Patches.png "Select the newest vrlcm-vra-8.x.x-PatchX ovabundle to download")](https://my.vmware.com/group/vmware/patch#search)

See KB 79105 for mapping a patched product binary

---

> * Mapping binaries for the scale out of patched products in vRealize Suite Lifecycle Manager (79105) - [https://kb.vmware.com/s/article/79105](https://kb.vmware.com/s/article/79105)
