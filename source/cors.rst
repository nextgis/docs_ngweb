Cross-origin resource sharing (CORS)
======================================

.. note:: 
	This functionality is available only to nextgis.com `Mini and Premium users <https://nextgis.com/pricing-base/>`_.

If you’re a developer and would like to use your Web GIS as a backend for your own map or an app, you can switch on and set up :term:`CORS`. 
This mode allows to use data from Web GIS for a map or system on your ogranization's domain, while all geodata uploads and management pains will be taken care of by your Web GIS at nextgis.com.

In your Web GIS Control panel go to "Cross-origin resource sharing (CORS)" section and enter allowed origins for cross-domain requests on CORS Settings page, one origin per line. Press **Save**.

.. figure:: _static/CORS_settings_eng.png
   :name: CORS_settings_pic
   :align: center
   :width: 20cm
   
   CORS settings page
   
Please note that different protocols (HTTP and HTTPS) and subdomains (example.com and www.example.com) are different origins. Wildcards are allowed for third-level domains and higher.
