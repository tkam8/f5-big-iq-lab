Lab 1.6: Run Bash Scripts on Devices that BIG-IQ Manages 
--------------------------------------------------------

.. note:: Estimated time to complete: **5 minutes**

This lab will show you how to create and deploy/run scripts on a managed device from BIG-IQ.

This feature can be used for various purposes, including deploying or modifying BIG-IP configuration that is not natively manageable by the BIG-IQ
or running a standard device startup/provisioning script prior to import into BIG-IQ. Scripts can be bash or TMSH commands.

Official documentation about BIG-IQ Script Management can be found on the `F5 Knowledge Center`_.

.. _F5 Knowledge Center: https://techdocs.f5.com/en-us/bigiq-7-1-0/managing-big-ip-devices-from-big-iq/script-management.html

.. raw:: html

    <iframe width="560" height="315" src="https://www.youtube.com/embed/PmaOq1bK4YU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

|

.. include:: /accesslab.rst

Tasks
^^^^^

1. Login to BIG-IQ as **david**.

2. Navigate to the DEVICE tab, Script Management, Scripts. Click on Add.

.. image:: ./media/lab-6-1.png
  :scale: 40%
  :align: center

3. Type a name (e.g. ``CVE-2020-5902``), and copy the code below or copy the |location_link| and replace the credentials with ``CREDS=admin:purple123`` instead of ``CREDS=<username><password>``.

.. |location_link| raw:: html

   <a href="https://raw.githubusercontent.com/usrlocalbins/Big-IQ-scripts/master/CVE-Bash%20Script" target="_blank">TMUI RCE vulnerability CVE-2020-5902 bash script</a>

.. code-block:: bash

   #!/bin/bash
   
   # TMUI RCE vulnerability CVE-2020-5902
   # Security Advisory Description
   # The Traffic Management User Interface (TMUI), also referred to as the Configuration utility, 
   # has a Remote Code Execution (RCE) vulnerability in undisclosed pages. (CVE-2020-5902)

   CREDS=admin:purple123

   IP=localhost

   curl -u $CREDS -k https://$IP/mgmt/tm/sys/httpd -X PATCH -d '{"include":"\n <LocationMatch \\\";\\\">\n  \
   Redirect 404 /\n </LocationMatch>\n <LocationMatch \\\"hsqldb\\\">\n Redirect 404 /\n </LocationMatch>\n "}'  \
   -H content-type:application/json

   sleep 10

   curl -k -u $CREDS -H "Content-Type: application/json" -d '{"command":"save"}' https://$IP/mgmt/tm/sys/config

   sleep 10

   Device="$(uname -n)"
   echo HOSTNAME:${Device/.*/}

   URL="https://localhost/tmui/login.jsp/..;/login.jsp"

   response=$(curl -k -s -w "%{http_code}" $URL)

   http_code=$(tail -n1 <<< "$response")  # get the last line
   content=$(sed '$ d' <<< "$response")   # get all but the last line which contains the status code 

   echo "$http_code"
   #echo "$content"
 
   echo "done"

|

.. image:: ./media/lab-6-2.png
  :scale: 40%
  :align: center

**Save & Close**.

4. After the script is saved, select it and click on **Run**.

.. image:: ./media/lab-6-3.png
  :scale: 40%
  :align: center

5. The script task properties opens, type a name (e.g. ``execution-script-20202707``), and select the SEA-vBIGIP01.termmarc.com and SJC-vBIGIP01.termmarc.com BIG-IPs.

.. image:: ./media/lab-6-4.png
  :scale: 40%
  :align: center

6. Click on **Run**. The following window opens. Click on *Script Logs*.

.. image:: ./media/lab-6-5.png
  :scale: 40%
  :align: center

7. The link takes you to the script logs window where you can see the task running...

.. image:: ./media/lab-6-6.png
  :scale: 60%
  :align: center

8. When the task if completed, the status will show *Finished*.

.. image:: ./media/lab-6-7.png
  :scale: 40%
  :align: center

9. You can see the details of the task by clicking on it.

.. image:: ./media/lab-6-8.png
  :scale: 40%
  :align: center

10. View Output will also show you the output of the script for each devices.

.. image:: ./media/lab-6-9.png
  :scale: 40%
  :align: center