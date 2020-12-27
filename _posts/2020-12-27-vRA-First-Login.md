---
layout: post
title: "Logging in to vRA for the first time"
description: "Helpful hints to logging into vRA after the initial deployment"
date: 2020-12-27
tags: vra vidm iam troubleshooting
---

## Logging in to vRA after a successful deployment from vRSLCM

Congratulations you have successfully completed deploying vRA from vRSLCM, and now its time to login for the first time. For my initial deployment I deployed a single node with a self signed certificate from the vRealize Easy Installer.

![vra splash page](/assets/images/vRA-First-Login-vRA-Splash-Page.png)

When browsing to ``https://your_vRA_FQDN`` and clicking Go To Login Page, we are presented with a login prompt from the vIDM Workspace One instance vRA is registered with. Its important to ensure we are logging in to the System Domain, since vRA's Identity and Access Management still needs to be configured with the desired users/groups and roles.
![system domain login](/assets/images/vRA-First-Login-System-Domain.png)

Once logged in with the configuration admin account we can see the 6 roles assigned to the Configurationadmin user.

* Service Broker Administrator
* Orchestrator Workflow Designer
* Orchestrator Administrator
* Migration Assistant Administrator
* Code Stream Developer
* Code Stream Administrator
* Cloud Assembly Administrator

![iam roles](/assets/images/vRA-First-Login-IAM-Roles.png)

## Logging in to vRA after a successful deployment from vRSLCM when an LDAP source has already been configured in vIDM Workspace One

However if this is not the initial deployment of vRA, chances are you have already configured vIDM Workspace One with a LDAP source for authentication. When logging in to the newly deployed vRA instance for the first time, with vIDM already configured to a LDAP source, we need to change the domain back to System Domain.

| ![vIDM AD Domain](/assets/images/vRA-First-Login-AD-Domain.png) | ![vIDM Change Domain](/assets/images/vRA-First-Login-Change-Domain.png)
|:---:|:---:|
| Change to a different domain | Select System domain for the Configuration Admin User |

Once Workspace One ui notes System Domain, login with the configuration administrator credentials showing in the globalenvironment under the vIDM product in vRSLCM.

| ![vIDM Configuration Admin](/assets/images/vRA-First-Login-vRSLCM-ConfigAdmin.png) | ![vRA login](/assets/images/vRA-First-Login-ConfigAdmin-Login.png)
|:---:|:---:|
| vIDM Configuration Admin User | vRA Login with Configuration Admin User |

Now that we have completed logging in to vRA, my first step is to browse to Identity and Access Management and integrate my domain groups from vIDM. When reaching IAM we can see our configuration admin user is entitled with the 6 roles needed to make it an administrator, however none of our users from vIDM Workspace One have been assigned Organization or Service Roles.

![vRA IAM](/assets/images/vRA-First-Login-vRA-IAM-vIDM.png)

## Errors

If we attempt to login with a domain account, prior to granting the user a role in IAM, we are presented with a 403 error. Simply click Sign Out, and log back in with the configuration admin user, noted in vRSLCM, ensuring the System Domain is selected.

>
403 Error
It appears that you don't have access to VMware vRealize Automation.

![vRA 403 Error](/assets/images/vRA-First-Login-vRA-403-Error.png)
