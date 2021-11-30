---
layout: post
title: "NSX-T URL Filtering"
description: "URL Filtering in NSX-T"
date: 2021-11-30
tags: troubleshooting nsx-t
---
## URL Filtering in NSX-T

After setting up URL Filtering in the NSX-T ICM class, I thought it would be fun to filter my IoT network and see what the devices are up too.

[![NSX-T URL Analysis](/assets/images/NSX-T-URL-Filtering-URL-Analysis.png "NSX-T URL Analysis")](/assets/images/NSX-T-URL-Filtering-URL-Analysis.png)

So, I started by enabling the URL Analysis service under North South Security in the Security tab of the NSX Manager.

[![NSX-T URL Analysis Settings](/assets/images/NSX-T-URL-Filtering-URL-Analysis-Settings.png "NSX-T URL Analysis Settings")](/assets/images/NSX-T-URL-Filtering-URL-Analysis-Settings.png)

Then I proceed setting up the firewall rule, as we had done in class.

Under North South Security, in the Security tab, select Gateway Firewall. Then, select Gateway Specific Rules. Change the Gateway dropdown to the T1 gateway, per the requirements in the Layer 7 Context Profile doc.

> * Layer 7 Context Profile - [https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/administration/GUID-5A73D94A-1B37-4D3C-9E09-E6936F39AEE3.html](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/administration/GUID-5A73D94A-1B37-4D3C-9E09-E6936F39AEE3.html)

With the T1-GW-01 gateway selected, create the new Policy and Rule. In my case I called these ``URL Inspection Policy`` and ``URL Inspection Rule``.
With the Rule set to ``Any`` for the Sources and Destinations, set the Services to ``DNS`` and ``DNS-UDP``, set the Context Profiles to ``DNS`` and Applied To ``T1-GW-01``. This will allow filtering of all dns requests passing the T1 gateway.

[![NSX-T URL Analysis T1 Gateway Firewall Rule](/assets/images/NSX-T-URL-Filtering-T1-Gateway-Firewall-Rule.png "NSX-T URL Analysis T1 Gateway Firewall Rule")](/assets/images/NSX-T-URL-Filtering-T1-Gateway-Firewall-Rule.png)

However, I had missed the note regarding a medium sized edge node or higher is required for Layer 7 rules. Which is why I am sharing this post. Since, during my investigation of public knowledge, I found little regarding the error messages presented. So, I wanted to share my observations.

* Prerequisites - A medium-sized edge node (or higher), or a physical form factor edge.

> * Create a Layer 7 DNS Rule - [https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/administration/GUID-02A1D4FD-7A43-42E7-B228-EB5DE27F8A15.html?hWord=N4IghgNiBcILYFMAmBLArnEBfIA](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/administration/GUID-02A1D4FD-7A43-42E7-B228-EB5DE27F8A15.html?hWord=N4IghgNiBcILYFMAmBLArnEBfIA)

Seeing the ui message I started hunting in the logs to see what is generating the Internal Error when applying the Context Profile to the rule. As I had found out over troubleshooting with out the context Profile set to DNS, the rule would publish successfully. Further more I found it was any Context Profile which would seem to generate this error. Which was pointing to a L7 issue rather then a specific DNS rule problem.

> * Log Messages and Error Codes - [https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/administration/GUID-406AF9C3-E8F7-447A-8E3D-92AFB9D5E973.html](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/administration/GUID-406AF9C3-E8F7-447A-8E3D-92AFB9D5E973.html)

Following the edge syslog, show's the following, No Service Cores Configured

`` > get log-file syslog -follow ``

UI notes: ``Internal error(1401) occurred on transport node <UUID>.``

Edge syslog notes: ``[ERROR] No Service Cores Configured.. Cannot configure L7 Rule``

In researching the above edge log error message, No Service Cores configured, I came to learn Service Cores are only part of medium-sized edge node (or higher) edge deployments

Seeing version note 0.0.0 on both of my edge nodes got me thinking. If the service is reaching out to the internet for its definitions let me check DNS on the nodes. Yup, my configured DNS server the edges were leveraging was not responding. Its now been 0 day's since it was DNS.

![image alt <>](/assets/images/Zero-Days-Since-It-Was-DNS.png)

Since fixing the edge's DNS resolution, it was able to retrieve the latest URL Data

[![NSX-T URL Analysis Settings URL Data Version](/assets/images/NSX-T-URL-Filtering-URL-Analysis-Settings-URL-Data-Version.png "NSX-T URL Analysis Settings URL Data Version")](/assets/images/NSX-T-URL-Filtering-URL-Analysis-Settings-URL-Data-Version.png)

Now to start investigating what the filters find.
