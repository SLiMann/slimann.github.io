---
layout: post
title: "Logging in to the vRealize Automation 8.x Embedded Orchestrator Control Center"
description: "vRealize Automation 8.x Embedded Orchestrator no longer has a page noting the Control Center URL"
date: 2020-11-26
tags: vra vro control-center troubleshooting
---

## Logging in to the vRealize Automation 8.x Embedded Orchestrator Control Center

Beginning with the 8.x branch of vRealize Orchestrator and Automation, the virtual machine interface VAMI running on port 5480 has been depreciated. Along with the VAMI being deprecated, the tab for controlling the Orchestrator server and control center are no longer available.

| ![vRA](/assets/images/vRO-Control-Center-Login-vRA-Console-Login.png) | ![vRO](/assets/images/vRO-Control-Center-Login-Console-Login.png)
|:---:|:---:|
| vRA Console | vRO Console |

Fortunately, the Control Center URL is noted in the following documentation

---

> vRA 8.x with embedded vRealize Orchestrator instance
>
> You can access the Control Center of the embedded vRealize Orchestrator by navigating to ``https://your_vRA_FQDN/vco-controlcenter`` and logging in as root.
>
> * Using the Embedded vRealize Orchestrator Client - [https://docs.vmware.com/en/vRealize-Automation/8.0/vmware-vrealize-orchestrator-embedded/GUID-F1AF994C-FCE8-4E40-81F1-285E469CC95F.html](https://docs.vmware.com/en/vRealize-Automation/8.0/vmware-vrealize-orchestrator-embedded/GUID-F1AF994C-FCE8-4E40-81F1-285E469CC95F.html)

---

> vRO 8.x
>
> You can access the vRealize Orchestrator client and Control Center services at the following endpoints:
>
> * vRealize Orchestrator Network Ports and Endpoints - [https://docs.vmware.com/en/vRealize-Orchestrator/8.0/com.vmware.vrealize.orchestrator-install-config.doc/GUID4CE80CCD-CBDF-49BA-A87C-B63BAE3C776F.html](https://docs.vmware.com/en/vRealize-Orchestrator/8.0/com.vmware.vrealize.orchestrator-install-config.doc/GUID4CE80CCD-CBDF-49BA-A87C-B63BAE3C776F.html)
>

---

>
    ~~~ vRO Client
    https://appliance_FQDN/orchestration-ui
    ~~~
>
    ~~~ vRO Control Center
    https://appliance_FQDN/vco-controlcenter
    ~~~
