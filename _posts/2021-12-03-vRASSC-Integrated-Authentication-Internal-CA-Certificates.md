---
layout: post
title: "Deploying vRealize Automation SaltStack Config to Trust Internal CA Signed Certificates"
description: "Deploying vRealize Automation SaltStack Config to trust the root certificate applied to vRealize Automation"
date: 2021-12-03
tags: troubleshooting vrassc vrslcm
---
## Deploying vRealize Automation SaltStack Config to trust the root certificate applied to vRealize Automation

I started deploying vRealize Automation SaltStack Config to check out its features by adding it to my existing vRealize Automation deployment, in vRealize Suite Lifecycle Manager.

[![vRASSC Deployment.](/assets/images/vRASSC-Integrated-Authentication-Internal-CA-Certificates-Select-Product.png "vRASSC Deployment")](/assets/images/vRASSC-Integrated-Authentication-Internal-CA-Certificates-Select-Product.png)

Followed by selecting a certificate issued by the same Internal CA vRA is leveraging.

[![Internal CA Certificate.](/assets/images/vRASSC-Integrated-Authentication-Internal-CA-Certificates-Certificate.png "Internal CA Certificate")](/assets/images/vRASSC-Integrated-Authentication-Internal-CA-Certificates-Certificate.png)

Then I ensured the slider to Integrate with Identity Manager was enababled.

[![Integrate with Identity Manager.](/assets/images/vRASSC-Integrated-Authentication-Internal-CA-Certificates-Integrate-with-Identity-Manager.png "Integrate with Identity Manager.")](/assets/images/vRASSC-Integrated-Authentication-Internal-CA-Certificates-Integrate-with-Identity-Manager.png)

After successfully deploying vRASSC from vRSLCM, integrated with vRA authentication, I was greeted by a friendly 403 error page.

[![VMW SaltStack Config 403 Forbidden It appears you do not have access to this resource.](/assets/images/vRASSC-Integrated-Authentication-Internal-CA-Certificates-403-Forbidden.png "VMW SaltStack Config 403 Forbidden It appears you do not have access to this resource.")](/assets/images/vRASSC-Integrated-Authentication-Internal-CA-Certificates-403-Forbidden.png)

In checking the RaaS logs (/var/log/raas/raas) we observe SSL certificate verification failure messages. These are due to the certificate applied to vRA, being either self-signed or signed by an internal certificate authority (vRSLCM Locker, AD CA, etc)

``2021-12-02 22:53:05,915 [var.lib.raas.unpack._MEIABoEDF.raas.mods.vra.params][ERROR:252 ][Webserver:16730] SSL Exception - https://vranode1.lab.slimann.com/csp/gateway/am/api/auth/discovery may be using a self-signed certificate HTTPSConnectionPool(host='vranode1.lab.slimann.com', port=443): Max retries exceeded with url: /csp/gateway/am/api/auth/discovery?username=service_type&state=aHR0cHM6Ly92cmFzc2MubGFiLnNsaW1hbm4uY29tL2lkZW50aXR5L2FwaS9jb3JlL2F1dGhuL2NzcA%3D%3D& redirect_uri=https%3A%2F%2Fvrassc.lab.slimann.com%2Fidentity%2Fapi%2Fcore%2Fauthn%2Fcsp&client_id=ssc-r9b5DnV1Iy (Caused by SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: self signed certificate in certificate chain (_ssl.c:1076)')))
2021-12-02 22:53:05,965 [tornado.application][ERROR:1792][Webserver:16730] Uncaught exception GET /csp/gateway/am/api/loggedin/user/profile (10.1.20.35)
HTTPServerRequest(protocol='https', host='vrassc.lab.slimann.com', method='GET', uri='/csp/gateway/am/api/loggedin/user/profile', version='HTTP/1.1', remote_ip='10.1.20.35')``

To allow vRASSC to trust the certificate applied to the authentication provider (vRA), we need to first capture the root certificate, followed by appending it to /etc/pki/tls/certs/ca-bundle.crt, and restarting the RaaS service. This is noted in the docs under the "Troubleshooting SaltStack Config environments with vRealize Automation that use self-signed certificates" section.

While the documentation outlines this process, I wanted to streamline the procedure. So, I reached out to my colleague [Sean Jones](https://www.linkedin.com/in/seandylanjones/) for guidance in working with vRASSC. [Sean](https://www.linkedin.com/in/seandylanjones/) kindly assisted in crafting the following commands, and we have sequenced them together to help ease this process, once the new appliance is deployed (shared at the end of the post). But to see how we got there keep reading

> * Set up SSL certificates - [https://docs.vmware.com/en/VMware-vRealize-Automation-SaltStack-Config/8.6/install-configure-saltstack-config/GUID-21A87CE2-8184-4F41-B71B-0FCBB93F21FC.html#GUID-21A87CE2-8184-4F41-B71B-0FCBB93F21FC__SECTION_05AB07CE-D60B-4621-BEDC-680CBC14A46E](https://docs.vmware.com/en/VMware-vRealize-Automation-SaltStack-Config/8.6/install-configure-saltstack-config/GUID-21A87CE2-8184-4F41-B71B-0FCBB93F21FC.html#GUID-21A87CE2-8184-4F41-B71B-0FCBB93F21FC__SECTION_05AB07CE-D60B-4621-BEDC-680CBC14A46E)

* Troubleshooting SaltStack Config environments with vRealize Automation that use self-signed certificates

In reaching step 4, after sshing to the vRASSC appliance, we can get the certificate from vRA and update the ca-bundle.crt file
For example my vRA host is vranode1.lab.slimann.com. Update this value in the following command to your vRA fqdn noted in the RaaS error logs.

``root@vrassc [ ~ ]# echo -n | openssl s_client -connect vranode1.lab.slimann.com:443 -showcerts | tac | sed -ne '1,/-BEGIN CERTIFICATE-/p' | tac | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' | tee -a /etc/pki/tls/certs/ca-bundle.crt``

After running the above command we can confirm our root certificate is added to the end of /etc/pki/tls/certs/ca-bundle.crt

In step 5 we need to add a line to the raas.service file, in a specific location, so we thought it be best to again script this portion.

``root@vrassc [ ~ ]# sed -i.bak '/ExecStart\=/iEnvironment=REQUESTS_CA_BUNDLE=/etc/pki/tls/certs/ca-bundle.crt' /usr/lib/systemd/system/raas.service``

Now we have a raas.service.bak of the original and updated raas.service file in /usr/lib/systemd/system/.

With the root certificate of vRA added to a-bundle.crt, and the path set in raas.service, its time to restart the services and refresh the site to confirm resolution in step 6.

``root@vrassc [ ~ ]# systemctl daemon-reload``

``root@vrassc [ ~ ]# systemctl stop raas``

``root@vrassc [ ~ ]# rm /var/log/raas/raas``

``root@vrassc [ ~ ]# systemctl start raas``

``root@vrassc [ ~ ]# tail -f /var/log/raas/raas``

With all of these commands, we again think there is an easier way. So we have put together the commands with the following extensive "one-liner"

... just update the vRA host name from vranode1.lab.slimann.com to your vRA fqdn in the beginning of the following command

``root@vrassc [ ~ ]# echo -n | openssl s_client -connect vranode1.lab.slimann.com:443 -showcerts | tac | sed -ne '1,/-BEGIN CERTIFICATE-/p' | tac | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' | tee -a /etc/pki/tls/certs/ca-bundle.crt && sed -i.bak '/ExecStart\=/iEnvironment=REQUESTS_CA_BUNDLE=/etc/pki/tls/certs/ca-bundle.crt' /usr/lib/systemd/system/raas.service && systemctl daemon-reload && systemctl stop raas && rm /var/log/raas/raas && systemctl start raas && echo -e 'Starting RaaS Service and tailing the log to see if errors persist. \n Please Wait' && sleep 10 && echo -e 'Relaunch the web browser and navigate to the vRASSC login page and monitor this screen for further errors.\n CTRL+C to exit the tail' && tail -f /var/log/raas/raas``
