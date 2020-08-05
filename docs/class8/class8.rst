Class 8: BIG-IQ Application Security Manager
============================================

Overview
^^^^^^^^

BIG-IQ ASM enables enterprise-wide management and configuration of multiple BIG-IP devices from a central 
management platform. 
You can centrally manage BIG-IP devices and security policies, and import policies from files on those devices.

For each device discovered, an additional virtual server is created to hold all security policies
that are not related to any virtual server on the device. To deploy a policy to a device, the policy 
must be attached to one of the device's virtual servers. Policies can be deployed to a device that already has 
the policy by overwriting it. If the policy does not yet exist on the device, you have the option to deploy 
it as a new policy attached to an available virtual server or as an inactive policy.

`K07359270 Succeeding with application security`_

.. _K07359270 Succeeding with application security: https://support.f5.com/csp/article/K07359270

See `Class 1 Module 3`_ for more details on BIG-IQ and AS3 WAF integration.

.. _Class 1 Module 3: ../../class1/module3/module3.html


Modules/Labs
^^^^^^^^^^^^

.. toctree::
   :maxdepth: 1
   :glob:

   module*/module*

------------

.. include:: ../lab.rst