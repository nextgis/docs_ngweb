.. sectionauthor:: Maxim Dubinin <maxim.dubinin@nextgis.com>

How to change Web GIS domain
============================

.. note:: 
	This functionality is available only to nextgis.com `Premium users <http://nextgis.com/nextgis-com/plans>`_.

After you created a Web GIS you get a domain at nextgis.com, for example *mywebgis.nextgis.com*. You can rename it to something else, i.e. *mywebgis2.nextgis.com* by contacting support.

You can also change it to a subdomain  under the domain of your organization, for example *gis.example.com* where example.com is the domain of your organization. We will use these two addresses in the examples below.

.. note::
	You can't use your organization domain itself (mycompany.com) as Web GIS domain.  It is impossible to add CNAME record in the DNS zone root with most of the providers.

Follow these steps to change the domain name. The described actions are done by the system administrator of your company or another authorized person who has access to DNS records.

**Step 1. Add DNS record to get HTTPS certificates**

.. code-block:: bash

   _acme-challenge.gis.example.com. CNAME _acme-challenge.nimbo.nextgis.net.
   
change *gis.example.com* in this record to the needed domain under the domain of your organization.

**Check 1**

Your system administrator needs to check that changes took effect. Use `Dig <https://toolbox.googleapps.com/apps/dig/#CNAME/>`_. 

Enter: _acme-challenge.gis.example.com. 

Expected answer: CNAME (TTL and TARGET). 
TARGET must have the following value: _acme-challenge.nimbo.nextgis.net.

Give some time for DNS changes to take effect.

**Step 2. Add DNS record 2 to redirect requests to Web GIS**

.. code-block:: bash

   gis.example.com. CNAME example.nextgis.com.

change *gis.example.com* to the needed domain under the domain of your organization, change *example.nextgis.com* to your Web GIS address at *.nextgis.com

**Check 2**

Your system administrator needs to check that changes took effect. Use `Dig <https://toolbox.googleapps.com/apps/dig/#CNAME/>`_. 

Enter: gis.example.com

Expected answer: CNAME (TTL and TARGET). 
TARGET must have the following value: mywebgis.nextgis.com.

Give some time for DNS changes to take an effect.

**Step 3. Inform us**

After both checks are successful, let us know at support@nextgis.com. Please use the following template:

1. Your Web GIS URL: *mywebgis.nextgis.com*
2. Domain you want to switch to: *gis.example.com*

We will finalize the setup and contact you when everything is ready.
