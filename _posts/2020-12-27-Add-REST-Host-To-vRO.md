---
layout: post
title: "Add a REST host to Orchestrator and Invoke a REST Operation"
description: "How to add a REST host to Orchestrator and Invoke a REST operation"
date: 2020-12-27
tags: vro vrslcm rest
---

## VRSLCM API URL

For this demo, I find it's easiest to leverage the vRealize Suite Lifecycle Manager appliance. So lets begin by logging in to vRSLCM and noting our endpoint URL

I launched the vRSLCM Swagger API interface ``https://vRSLCM_appliance_FQDN/api/swagger-ui.html``, and copied the public API url ``https://vRSLCM_appliance_FQDN/api/v2/api-docs?group=1.public-api`` to be used in the next step when we run the ``Add a REST host by Swagger spec from a URL`` workflow.

![vRSLCM Swagger API](/assets/images/Add-REST-Host-To-vRO-vRSLCM-API.png)

---

## Adding a REST host to Orchestrator

With the endpoint url noted from the vRSLCM Swagger API, login to the vRealize Orchestrator client at:``https://vRO_appliance_FQDN/orchestration-ui``

Expand "Library" on the left navigation bar and select "Workflows". Then, filter workflows on ``HTTP-REST`` and click Run on the ``Add a REST host by Swagger spec from a URL`` workflow.

![vRO Add REST HOST WF Tile](/assets/images/Add-REST-Host-To-vRO-Add-Host-Workflow-Tile.png)

## Add a REST host workflow Properties

Populate the new endpoints Name, I have chosen ``vRSLCM 8.2 Public API``. For the url, we will use the vRSLCM public API url we noted above in our first step. ``https://vRSLCM_appliance_FQDN/api/v2/api-docs?group=1.public-api`` Select HTTPS as the Preferred communication protocol.check the option to silently accept the certificate and add it to the trusted store.

![vRO Add REST HOST WF Properties](/assets/images/Add-REST-Host-To-vRO-Add-Host-Workflow.png)

## Add a REST host Spec Authentication

vRSLCM API URL is not protected by authentication, so this may be left blank

![vRO Add REST HOST WF Spec Auth](/assets/images/Add-REST-Host-To-vRO-Add-Host-Workflow-Spec-Auth.png)

## Add a REST host Authentication

vRSLCM accepts Basic Authentication, so we will select it from the list of options.

![vRO Add REST HOST WF Auth](/assets/images/Add-REST-Host-To-vRO-Add-Host-Workflow-Auth.png)

## Add a REST host User Credentials

Select "Shared Session", so all vRO users will leverage this account to connect when using this REST host. Then, supply the vRSLCM admin user name ``admin@local`` and password.

![vRO Add REST HOST WF Credentials](/assets/images/Add-REST-Host-To-vRO-Add-Host-Workflow-Credentials.png)

<!-- ## Add a REST host Proxy Settings

Define a proxy as needed to reach the REST host from vRO. Generally this option is unchecked. I have checked so you may see the options.

![vRO Add REST HOST WF Proxy](/assets/images/Add-REST-Host-To-vRO-Add-Host-Workflow-Proxy.png) -->

## Add a REST host SSL

Verify whether the target hostname matches the names stored inside the server's X.509 certificate , is checked by default. This means vRO will confirm the certificate presented has the host name we have requested. Additionally, we can supply the private key of the certificate for hosts which require clients present a certificate.

![vRO Add REST HOST WF SSL](/assets/images/Add-REST-Host-To-vRO-Add-Host-Workflow-SSL.png)

## Run the Add a REST host workflow

If the values supplied are correct we will receive a completed result for the workflow run. If there is a failure we can re-run the workflow by clicking the Run Again button and updating the supplied values.

![vRO Add REST HOST WF](/assets/images/Add-REST-Host-To-vRO-Add-Host-Workflow-Run.png)

Close the workflow Run to return to the HTTP-REST Workflows.

## Confirm REST host has been added to the Inventory

Expand "Administration" on the left navigation bar and select "Inventory". Then, Expand HTTP-REST and ensure the new end point has been added successfully.

![Inventory HTTP-REST](/assets/images/Add-REST-Host-To-vRO-Inventory-HTTP-REST.png)

Clicking on the host and reviewing the details, we can see exactly the options leveraged when populating the workflow run. Additionally when we expand the host we can see REST operations imported via the Swagger spec.

![Inventory HTTP-REST details](/assets/images/Add-REST-Host-To-vRO-Inventory-HTTP-REST-Details.png)

---

## Invoke a REST Operation

Run the ``Invoke a REST operation`` workflow, against our new vRSLCM endpoint

![vRO Invoke A REST Operation WF Tile](/assets/images/Add-REST-Host-To-vRO-Invoke-A-REST-Operation-Tile.png)

![vRO Invoke A REST Operation WF](/assets/images/Add-REST-Host-To-vRO-Invoke-A-REST-Operation.png)

Clicking the line for REST operation to be used for the call, pop-ups a dialogue to select the REST host and the API call to make.

![vRO Invoke A REST Operation WF Select](/assets/images/Add-REST-Host-To-vRO-Invoke-A-REST-Operation-Select.png)

For this demo lets scroll down the list and select the ``get_getAllEnvironmentsV2UsingGET`` operation

![vRO Invoke A REST Operation WF Select getAllEnvironments](/assets/images/Add-REST-Host-To-vRO-Invoke-A-REST-Operation-Select-getAllEnvironments.png)

Next switch to the Input Parameters tab and clear the Parameter 1 value ``<status>`` , to retrieve all environments regardless of status. Or, set Parameter 1 to ``COMPLETED`` to see completed environment requests. then click Run.

![vRO Invoke A REST Operation WF Input Parameters](/assets/images/Add-REST-Host-To-vRO-Invoke-A-REST-Operation-Input-Parameters.png)

After clicking Run, the workflow will return the environments in the logs window as an multi-dimensional array.

![vRO Invoke A REST Operation WF Run](/assets/images/Add-REST-Host-To-vRO-Invoke-A-REST-Operation-Run.png)

---

You have now completed adding a HTTP-REST host to vRealize Orchestrator with a Swagger spec. Then, we invoked a REST operation, which was imported from the Swagger spec, and reviewed the output in the logs window of the ``Invoke a REST operation`` workflow run.
