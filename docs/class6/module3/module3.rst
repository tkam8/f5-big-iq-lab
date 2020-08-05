Module 3: Version Specific Objects
==================================

The Version Specific feature added in 6.1 is meant to address issues with changes to default profiles and 
monitors across versions. This feature is still there, and should continue to be used for that use case.
 
The new `v7.1 Silo feature`_ is intended to solve a different problem of shared object collisions that are not 
related to Version Specific defaults.

.. _`v7.1 Silo feature`: ../module5/module5.html

When adding multiple BIG-IP devices and discovering and importing their services at the same time,
you can specify a conflict resolution policy if BIG-IQ finds any default monitors or LTM profiles with different parameters.

The options are:

- Use BIG-IQ, BIG-IQ replaces conflicting shared objects with the object that exists on this BIG-IQ.
- Use BIG-IP, BIG-IQ replaces any conflicting shared objects with the objects it's importing from the BIG-IP device.
- Create Version, BIG-IQ creates an instance of the object that is specific to the software version running on the BIG-IP device you are importing.

.. image:: ../pictures/img_module3_1.png
   :align: left
   :scale: 60%


BIG-IQ creates and stores a copy of the BIG-IP device's LTM monitor or profile object(s), specific to the software version running on that BIG-IP device.
If you select this option, BIG-IQ replaces that object for all the managed BIG-IP devices running that version, the next time it deploys a configuration.
You can store multiple versions of LTM monitors or profiles. BIG-IQ deploys the appropriate stored version to your managed devices.
BIG-IQ automatically resolves conflicts against the appropriate version the next time it imports services that contain LTM monitors or profiles.

Navigate to the Configuration tab, under Local Traffic left-hand menu to see objects which have different version available.

.. image:: ../pictures/img_module3_2.png
   :align: left
   :scale: 60%


If you select one object with different version, you can see the detail of that object for each version available.


.. image:: ../pictures/img_module3_3.png
   :align: left
   :scale: 60%
