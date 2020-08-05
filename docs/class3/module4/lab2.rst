Lab 4.2: Generate Reports in CSV using the API 
----------------------------------------------

.. include:: /accesslab.rst

Tasks
^^^^^
1. From the lab environment, launch a remote desktop session to have access to the Ubuntu Desktop. 

2. Open a terminal and execute the following script to export the analytics of ``site16_boston`` into a CSV::

    cd f5-demo-bigiq-analytics-export-restapi
    ./export_app_http_stats_csv_demo.sh /security/site16_boston/serviceMain -1h now 60 -1h now 60


.. image:: ../pictures/module4/img_lab2_1.png
  :align: center
  :scale: 40%

The `script`_ is getting the Analytics using BIG-IQ API and exports it to a JSON file.
Then, the JSON output is converted to a CSV file using a json2csv tool.

.. _script: https://github.com/f5devcentral/f5-big-iq-lab/tree/develop/lab/f5-demo-bigiq-analytics-export-restapi

.. image:: ../pictures/module4/img_lab2_2.png
  :align: center
  :scale: 40%

3. Navigate in the ``f5-demo-bigiq-analytics-export-restapi`` folder and open the ``output.csv`` file.

.. image:: ../pictures/module4/img_lab2_3.png
  :align: center
  :scale: 40%

The data exported correspond to the HTTP Transactions for ``site16_boston`` application services.

.. image:: ../pictures/module4/img_lab2_4.png
  :align: center
  :scale: 40%