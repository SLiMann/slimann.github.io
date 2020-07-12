---
layout: default
title: "vRSLCM API Authentication"
date: 2020-07-11
---

To begin working with vRSLCM to gather details leveraging the api, like the list of environments, we first need to figure out how to authenticate.
Many VMware KBs advise teh following for creating the <token>:
<token> : The Base64 encoded value of "username:password". Here username is 'admin@local', and password is admin@local user's password.

While the words make sense, they leave out exactly how one might Base64 encode the value of "username:password"

I tend to leverage sites like base64encode.org to accomplish quick conversion task's. For example lets encode:
user name: admin@local
Password: VMwareRocks

This would form the string to encode: admin@local:VMwareRocks
encoding the string results in the following value: YWRtaW5AbG9jYWw6Vk13YXJlUm9ja3M=

[![Base64Encode.org the vRSLCM password](/assets/images/vRSLCM-API-Authentication-Base64.png "Base64 Encode vRSLCM Credentials")](https://www.base64encode.org)

[![An old rock in the desert](/assets/images/vRSLCM-API-Authentication-Base64.png "Shiprock, New Mexico by Beau Rogers")](https://www.flickr.com/photos/beaurogers/31833779864/in/photolist-Qv3rFw-34mt9F-a9Cmfy-5Ha3Zi-9msKdv-o3hgjr-hWpUte-4WMsJ1-KUQ8N-deshUb-vssBD-6CQci6-8AFCiD-zsJWT-nNfsgB-dPDwZJ-bn9JGn-5HtSXY-6CUhAL-a4UTXB-ugPum-KUPSo-fBLNm-6CUmpy-4WMsc9-8a7D3T-83KJev-6CQ2bK-nNusHJ-a78rQH-nw3NvT-7aq2qf-8wwBso-3nNceh-ugSKP-4mh4kh-bbeeqH-a7biME-q3PtTf-brFpgb-cg38zw-bXMZc-nJPELD-f58Lmo-bXMYG-bz8AAi-bxNtNT-bXMYi-bXMY6-bXMYv)


![Base64Encode.org the vRSLCM password]({{ site.url }}/assets/vRSLCM-API-Authentication-Base64.png )


> Updating the <token> we can now discover our environment details with the following curl command:

~~~
{
    curl -h "Authorization: Basic <token>" -k https://lcm81.slimann.com/lcm/lcops/api/environments
    curl -h "Authorization: Basic YWRtaW5AbG9jYWw6Vk13YXJlUm9ja3M=" -k https://lcm81.slimann.com/lcm/lcops/api/environments
}
~~~