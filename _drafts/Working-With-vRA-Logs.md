---
layout: post
title: "Working with vRA Logs"
description: "Helpful commands for working with vRA logs"
date: 2020-07-11
tags: vra troubleshooting
---

## Working with vRA 8.x logs

With the release of vRA 8 we are no longer able to simply tail a single vcac log file during investigations. We now need to work with Kubernetes and how containers handle logging.

Kubernetes comes with a tool called ``kubectl``, which can be used to follow logs. The ``kubectl logs`` command can be used with the ``--follow`` parameter to tail the pods logs.

For example, to follow the postgres pods logs we can use ``kubectl logs --namespace [vra namespace] --follow [pod name]``

``kubectl logs --namespace prelude --follow postgres-0``

Since this is a kubectl command it works across nodes in a clustered environment. Regardless if the pod is running locally or on another node in the cluster. Unfortunately, there are some drawbacks when leveraging this approach.

For example, one of my recent cases, 1/3 nodes had a networking issues which brought down the postgres pod. This meant we were unable to review the logs with ``kubectl``

Fortunately the pods running on a node store their logs locally in ``/services-logs/prelude/``
So, we can simply tail all the log files in the folder with ``tail -f /services-logs/prelude/postgres-0/*.log``

To further expand on this, lets say we want to tail all of the logs for all of the pods in the prelude namespace on a node.
We can leverage ``find /services-logs/prelude/ -name "*.log" | xargs tail -f``

---

> vRA 8.x
>
> *  How do I work with logs and log bundles in vRealize Automation - [https://docs.vmware.com/en/vRealize-Automation/8.0/Administering/GUID-EB314825-845A-4F31-8155-53F99F0B7B3C.html](https://docs.vmware.com/en/vRealize-Automation/8.0/Administering/GUID-EB314825-845A-4F31-8155-53F99F0B7B3C.html)

---

> vRA 8.2 has a new section regarding (Legacy) Pod logs
>
> * How do I work with logs and log bundles in vRealize Automation - [https://docs.vmware.com/en/vRealize-Automation/8.2/Administering/GUID-EB314825-845A-4F31-8155-53F99F0B7B3C.html](https://docs.vmware.com/en/vRealize-Automation/8.2/Administering/GUID-EB314825-845A-4F31-8155-53F99F0B7B3C.html)

Happy Logging :)
