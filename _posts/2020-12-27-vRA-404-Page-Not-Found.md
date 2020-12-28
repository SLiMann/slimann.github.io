---
layout: post
title: "Browsing to vRA returns a 404"
description: "When browsing to vRA by anything then its FQDN a 404 is returned"
date: 2020-12-27
tags: vra troubleshooting
---

## When browsing to vRA by anything then its FQDN a 404 is returned

Assuming all of the vRA services are running as expected and deploy.sh has finished successfully, noted at the end of ``/var/log/deploy.log``, vRA will only respond via an FQDN.

| ![Short Name](/assets/images/vRA-404-Page-Not-Found-Short-Name.png) | ![Fully Qualified Domain Name](/assets/images/vRA-404-Page-Not-Found-Fully-Qualified-Domain-Name.png)
|:---:|:---:|
| Short Name | Fully Qualified Domain Name |

## Confirm the FQDN

Additionally you may review ``/var/log/deploy.log`` to confirm the FQDN

![Multi Tenancy requirements](/assets/images/vRA-404-Page-Not-Found-Deploy-Log-FQDN.png)

## Thanks SNI :)

This is due to vRA's use of [SNI](https://en.wikipedia.org/wiki/Server_Name_Indication) to route the request to the appropriate internal virtual server. With vRA's multi tenancy ability this becomes a requirement. When Enabling Multi-Tenancy CNAME DNS Records are created for each tenant and all pointed at the vRA node/cluster lb. Having SNI setup in the vRA appliance allows it to respond to the request for each tenant by examining the fqdn the client browser requested. So, with out a FQDN being leveraged in the client browser the vRA appliance is un able to determine which internal virtual server the request should be routed too.

* Multi-Tenancy Model - [https://docs.vmware.com/en/VMware-vRealize-Suite-Lifecycle-Manager/8.2/com.vmware.vrsuite.lcm.8.2.doc/GUID-05B4DCAA-546D-4A13-88C1-852AAA903F1E.html](https://docs.vmware.com/en/VMware-vRealize-Suite-Lifecycle-Manager/8.2/com.vmware.vrsuite.lcm.8.2.doc/GUID-05B4DCAA-546D-4A13-88C1-852AAA903F1E.html)

![Multi Tenancy requirements](/assets/images/vRA-404-Page-Not-Found-Multi-Tenancy-Requirements.png){: style="background-color: white;"}
