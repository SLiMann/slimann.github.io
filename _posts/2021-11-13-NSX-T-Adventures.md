---
layout: default
title: "NSX-T Adventures"
description: "Adventures in NSX-T"
date: 2021-11-13
tags: troubleshooting nsx-t
---
## NSX-T Adventures

With vRealize Automation, vCloud Foundation, and VMware Tanzu all leveraging NST-T, I wanted to learn all about it. So, I reached out to one of the best VMware Instructors I know, [R. Damian Koziel](https://www.linkedin.com/in/rdkoziel/), and attended his Instructor Lead Training of VMware NSX-T Data Center: Install, Configure, Manager [V3.0] class.

Since attending the NSX-T ICM class, I have been busy working to integrate NSX-T into my home lab. I began by deciding I wanted the NSX-T deployment to live behind my pfSense lab networks, and gave NSX-t Overlays vLAN1025 and the vLANS had access to the entire trunk of vLAN's hosted by pfSense.

Now on to deploying Managers and Edges

With my lab's management cluster being low on resources, I proceeded to deploy the small nodes. Unfortunately, I quickly came to learn the Small node size is best suited for Global Manager use cases. So, I deployed a medium sized Manager. With the medium nodes, increased 24GB of memory up from 16GB of the small node, the ui performed noticeably better as a result.

* Deploy NSX Manager Nodes to Form a Cluster from the UI - https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/installation/GUID-B89F5831-62E4-4841-BFE2-3F06542D5BF5.html

[![NSX-T Appliance Node Size](/assets/images/NSX-T-Adventures-Add-Appliance-Node-Size.png "NSX-T Appliance Node Size")](/assets/images/NSX-T-Adventures-Add-Appliance-Node-Size.png)

However, when I went to delete my initial small Manager, I was unable to. Fortunately, I found this blog on, [VMware Arena](http://www.vmwarearena.com/remove-nsx-t-manager-from-nsx-t-manager-cluster/ "VMware Arena - Remove NSX-T Manager From NSX-T Manager Cluster") which notes the steps to remove the initial Manager.

``> get cluster status``

``> detach node <NSX-T Manager UUID>``

* Remove NSX-T Manager from NSX-T Manager Cluster - http://www.vmwarearena.com/remove-nsx-t-manager-from-nsx-t-manager-cluster/

With the initial Manager removed, the remaining manager is able to be deleted as well. However this can be dangerous as this allows you to delete all of the managers, which could destroy the NSX-T deployment. So, as always ensure you have setup NSX-T backups.

[![NSX-T Appliance Delete Node](/assets/images/NSX-T-Adventures-Delete.png "NSX-T Appliance Delete Node")](/assets/images/NSX-T-Adventures-Delete.png)
