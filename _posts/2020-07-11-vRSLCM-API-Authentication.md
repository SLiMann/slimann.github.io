---
layout: post
title: "vRSLCM API Authentication"
description: "Review generating a vRealize Suite Lifecycle Manager Authentication token"
date: 2020-07-11
tags: vrslcm authentication base64
---
# vRSLCM API Authentication

To begin working with vRealize Suite Lifecycle Manager (vRSLCM) to leverage the api, we first need to to authenticate.

Many VMware KBs advise the following for creating the `<token>` The Base64 encoded value of 'username:password'. In this example we use the default username 'admin@local', and password.

While the words make sense, lets explore how one might Base64 encode the value of 'username:password'

I tend to leverage sites like base64encode.org to accomplish quick conversion task's.

For example lets encode:
username: admin@local
password: VMwareRocks

This would form the string to encode: admin@local:VMwareRocks

Encoding the string results in the following value: YWRtaW5AbG9jYWw6Vk13YXJlUm9ja3M=

[![Base64Encode.org the vRSLCM password](/assets/images/vRSLCM-API-Authentication-Base64.png "Base64 Encode vRSLCM Credentials")](https://www.base64encode.org)


Updating the `<token>` we can now discover our environment details with the following curl command:

~~~
   root@vrslcm:~$ curl -h "Authorization: Basic <token>" -k https://lcm81.corp.com/lcm/lcops/api/environments
   
   root@vrslcm:~$ curl -h "Authorization: Basic YWRtaW5AbG9jYWw6Vk13YXJlUm9ja3M=" -k https://lcm81.corp.com/lcm/lcops/api/environments
~~~