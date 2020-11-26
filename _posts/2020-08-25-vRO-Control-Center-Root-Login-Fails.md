---
layout: post
title: "vRO Control Center Root login fails when the password is aged"
description: "Attempts to login to vRealize Orchestrator Control Center fail when the root password is aged."
date: 2020-08-25
tags: vro control-center authentication
---
## vRO Control Center Root login fails when the password is aged

Attempts to login to vRealize Orchestrator Control Center and VAMI :5480 fail when the root password is aged.
However, SSH'ing as root continues to work.

By default, the password for the root account of the vRealize Orchestrator Appliance expires after 365 days. 
This can be checked by running ``chage -l root``

You can increase the expiry for the root account by logging into the vRealize Orchestrator Appliance through an SSH client and running ``passwd -x number_of_days name_of_account.`` If you want to increase the vRealize Orchestrator Appliance root password to infinity, run ``passwd -x 99999 root.``

![Update root password age](/assets/images/vRO-Control-Center-Root-Login-Fails.png "Update root password age")

In both vRO 7.x and 8.x, the steps are the same:

1. Log in to the vRealize Orchestrator Appliance over SSH as root.
2. Run the ``passwd -x number_of_daysnumber_of_days root`` command.
3. To increase the duration of the root password indefinitely, run the ``passwd -x 99999 root`` command.

---

> vRA 7.x
>
> * Change the Root Password - [https://docs.vmware.com/en/vRealize-Orchestrator/7.6/com.vmware.vrealize.orchestrator-install-config.doc/GUID-CB6FD2E7-2B0C-4FA8-A883-C2AFB5E399DA.html](https://docs.vmware.com/en/vRealize-Orchestrator/7.6/com.vmware.vrealize.orchestrator-install-config.doc/GUID-CB6FD2E7-2B0C-4FA8-A883-C2AFB5E399DA.html)

---

> vRA 8.x
>
> * Change the Duration of the Root Password - [https://docs.vmware.com/en/vRealize-Orchestrator/8.1/com.vmware.vrealize.orchestrator-install-config.doc/GUID-CB6FD2E7-2B0C-4FA8-A883-C2AFB5E399DA.html](https://docs.vmware.com/en/vRealize-Orchestrator/8.1/com.vmware.vrealize.orchestrator-install-config.doc/GUID-CB6FD2E7-2B0C-4FA8-A883-C2AFB5E399DA.html)