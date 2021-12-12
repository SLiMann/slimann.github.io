---
layout: post
title: "vRSLCM Log4J fix"
description: "Patch Log4J in vRSLCM CVE-2021-44228 - VMSA-2021-0028"
date: 2021-12-12
tags: vrslcm security cve
---

## vRSLCM Log4j

vRealize Suite Lifecycle Manager 8.x leverages Log4j in its server and blackstone spring jar's, and so with the recent vulnerability noted in CVE-2021-44228 - VMSA-2021-0028 we need to patch the affected files.

During the process the vRSLCM and Blackstone jar's will be backed up, replaced and services restarted.

### Active Requests

Since we will be restarting the vRSLCM and Blackstone service's, ensure there are no active lifecycle operation or content management requests

### Snapshots for safety

As always, prior to applying any patch it is recommended to ensure there are adequate backups and snapshots!

### Download the fix

To begin we need to first download the [log4jfix](https://kb.vmware.com/sfc/servlet.shepherd/version/download/0685G00000cPSEKQA4) patch in the KB's Attachment's box, and copied to the appliance, over SCP, into the ``/tmp`` directory

> * Workaround instructions to address CVE-2021-44228 in vRealize Suite Lifecycle Manager 8.x (87097) - [https://kb.vmware.com/s/article/87097](https://kb.vmware.com/s/article/87097)

### SSH to the vRSLCM appliance

SSH to the vRSLCM appliance and change the directory to tmp

~~~bash
root@vrslcm [ ~ ]# cd /tmp
~~~

### Set permissions on the script, so it may be executed

Use chmod to set the log4jfix script permissions, so it may be executed

~~~bash
root@vrslcm [ /tmp ]# chmod +x log4jfix.sh
~~~

Confirm permissions are set successfully

~~~bash
root@vrslcm [ /tmp ]# ls -lah log4jfix.sh
-rwx------ 1 root root 339 Dec 12 19:29 /tmp/log4jfix.sh
~~~

### Apply the patch

~~~bash
root@vrslcm [ /tmp/ ]# ./log4jfix.sh
~~~

Applying the patch to vRSLCM 8.6 took a few minuets to complete in my environment.

To see the output of applying the patch, I have captured the console output here.
[Log4jfix output log](/assets/logs/vRSLCM-Log4jfix.log)

### Additional Information

This vulnerability and its impact on VMware products are documented in the following VMware Security Advisory (VMSA)

[VMSA-2021-0028](https://www.vmware.com/security/advisories/VMSA-2021-0028.html)
