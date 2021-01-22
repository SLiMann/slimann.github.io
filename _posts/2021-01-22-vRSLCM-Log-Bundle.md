---
layout: post
title: "vRSLCM Log Bundle"
description: "Generating a log bundle from the vRSLCM shell command line"
date: 2021-01-21
tags: vrslcm troubleshooting
---

## vRSLCM Log Bundles
Previously log bundles could only be generated from the Settings page of the vRSLCM site. Now we have an additional command line option.

### Gathering a Log bundle from the vRSLCM site

Generally speaking, gathering a log bundle from vRSLCM is a straight forward process of logging into the web interface and navigating to Lifecycle Operations --> Settings --> Logs.

* Generate Log Bundle in vRealize Suite Lifecycle Manager - [https://docs.vmware.com/en/VMware-vRealize-Suite-Lifecycle-Manager/8.2/com.vmware.vrsuite.lcm.8.2.doc/GUID-963DCFB7-FBAC-4A33-B7D3-D932C28D7B8E.html](https://docs.vmware.com/en/VMware-vRealize-Suite-Lifecycle-Manager/8.2/com.vmware.vrsuite.lcm.8.2.doc/GUID-963DCFB7-FBAC-4A33-B7D3-D932C28D7B8E.html)

Unfortunately, sometimes we encounter a condition where the web interface is not available, and support has requested a log bundle to investigate the issue.

### vRSLCM Log Bundles from the shell

So, from the vRSLCM shell we may run the following command to generate a log bundle, for VMware Global Support Services (GSS) to investigate

~~~
root@vrslcm [ /var/lib/vlcm-common/ ]# vlcm-support -w /data
~~~

This will result in an zip being created in ``/data`` called ``vlcmsupport-<utc date-time>.zip``
