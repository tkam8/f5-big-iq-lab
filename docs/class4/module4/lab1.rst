Lab 4.1: Configure High Availability for BIG-IQ: Manual Failover
----------------------------------------------------------------
.. include:: /accesslab.rst

Tasks
^^^^^
**Add a peer BIG-IQ system for a high availability configuration**

Before you can set up F5 BIG-IQ Centralized Management in a high availability (HA) pair, you must have two licensed BIG-IQ systems.

For the high-availability pair to synchronize properly, each system must be running the same BIG-IQ version, 
and the clocks on each system must be synchronized to within 60 seconds. To make sure the clocks are in sync, 
take a look at the NTP settings on each system before you add a peer.

Configuring BIG-IQ in a high availability (HA) pair means that you can still manage your BIG-IP devices even if one BIG-IQ systems fails.

**Lab:**

1. Let's first add a BIG-IQ CM image in the blueprint.

in lab environment, on the ``F5 Products`` column, click on **ADD**

.. image:: ../pictures/module4/img_module4_lab1_1a.png
  :align: center
  :scale: 60%

|

Select approriate release of BIG-IQ (same as the existing active BIG-IQ part of the blueprint) and set the following values for CPU/Memory/Disk:

    - vCPUs: 4
    - Memory: 16 GiB
    - Disk Size: 500 GiB

Click on **CREATE**.

.. image:: ../pictures/module4/img_module4_lab1_1b.png
  :align: center
  :scale: 60%

|

After few minutes, the VM is created in lab environment. Click on the new VM, go to the Subnets tab and bind additional interfaces (External and Internal).
Use the same last digit of the management interface for the additional interfaces (in example below .12 but most probably .9).

.. image:: ../pictures/module4/img_module4_lab1_1c.png
  :align: center
  :scale: 60%

|

Finally, start the new BIG-IQ.

.. image:: ../pictures/module4/img_module4_lab1_1d.png
  :align: center
  :scale: 60%

|

Then, start the new BIG-IQ CM VM.

2. Once the VM is started, open a web shell or ssh to the new BIG-IQ CM VM, reset the admin password, enable bash shell and enable root account.

.. code::

    (tmos)# modify auth user admin password admin
    (tmos)# modify auth user admin shell bash
    (tmos)# modify /sys db systemauth.disablerootlogin value false
    (tmos)# save sys config

3. Connect via ``SSH`` to the system *Ubuntu Lamp Server*.

4. Edit the hosts file and make sure **ONLY** the ``big-iq-cm-2.example.com`` is not commented with a ``#``.

.. warning:: Double check the IP addresses of the new secondary BIG-IQ and update it if necessary. In below example 10.1.1.9 and 10.1.10.9 are used.

.. code::

    # cd /home/f5/f5-ansible-bigiq-onboarding 
    # vi hosts

    [f5_bigiq_cm]
    #big-iq-cm-1.example.com ansible_host=10.1.1.4 discoveryip=10.1.10.4/24 ...
    big-iq-cm-2.example.com ansible_host=10.1.1.9 discoveryip=10.1.10.9/24 ...



5. Once the new VE is full up and running, execute the following script to onboard this new secondary BIG-IQ CM.

    ::

        # cd /home/f5/f5-ansible-bigiq-onboarding
        # sudo docker build -t f5-big-iq-onboarding .
        # ./ansible_helper ansible-playbook /ansible/bigiq_onboard.yml -i /ansible/hosts

6. Verify the new secondary BIG-IQ CM has been correclty configured (check hostname, self IP, VLAN, NTP, DNS, license)

.. image:: ../pictures/module4/img_module4_lab1_3.png
  :align: center
  :scale: 50%

|

.. warning:: If you are doing lab 2, stop here and go back to `Lab 4.2`_

.. _Lab 4.2: ./lab2.html

7. In BIG-IQ HA Settings click **Reset to Standalone**.

8. Open active BIG-IQ, go to System > BIG-IQ HA and Click the Add Secondary button.

.. warning:: Double check the IP addresses of the new secondary BIG-IQ and update it if necessary. In below example 10.1.1.9 and 10.1.10.9 are used.

*Properties*
 * IP Address =	10.1.10.9
 * Username = admin
 * Password = purple123
 * Root Password = purple123

.. image:: ../pictures/module4/img_module4_lab1_4.png
  :align: center
  :scale: 60%

|

9. Type the properties for the BIG-IQ system that you are adding and click the Add button at the bottom of the screen.

- In the IP Address field, type the IP address for the secondary BIG-IQ system.
- In the Username and Password fields, type the administrator's user name and password for the new BIG-IQ system.
- In the Root Password field, type the root password for the new BIG-IQ system.

.. image:: ../pictures/module4/img_module4_lab1_5.png
  :align: center
  :scale: 60%

|

Then, click OK.

.. image:: ../pictures/module4/img_module4_lab1_6.png
  :align: center
  :scale: 100%

|

The BIG-IQ system synchronize. Once they are finished, both appear as ready (green).
