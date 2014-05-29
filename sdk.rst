Software Development Kits
=========================

The are several Software Development Kits (SDKs) with support for OpenStack.
Below are specifics for using some SDKs with the OSDC.

Libcloud
------------------------
`Apache Libcloud <http://libcloud.apache.org/>`_ is a Python library that provides support for multiple cloud providers.
The OpenStack examples on the Libcloud site need to be slightly modified to work on the OSDC.
See the example below.

.. code-block:: python

    import os

    from libcloud.compute.types import Provider
    from libcloud.compute.providers import get_driver

    OpenStack = get_driver(Provider.OPENSTACK)

    driver = OpenStack(os.environ['OS_USERNAME'], os.environ['OS_PASSWORD'],
                         ex_force_auth_url=os.environ['OS_AUTH_URL'] + 'tokens',
                         ex_force_auth_version='2.0_password',
                         ex_force_service_name='Compute Service',
                         ex_force_service_type='compute',
                         ex_tenant_name=os.environ['OS_TENANT_NAME'])

    print driver.list_nodes()


.. _libcloud-example:

pyrax
------------------------
`Pyrax <https://github.com/rackspace/pyrax>`_ is a Python SDK for OpenStack/Rackspace APIs.

Pyrax requires a pyrax_creds file.
This file needs an OpenStack tenant_id which you can obtain with `keystone token-get`.
An example pyrax_creds file would look like::

    #pyrax_creds:
    [keystone]
    identity_type = keystone
    tenant_id = b61bf0683335448e8f3f778dec2949be
    username = demo
    password = *****************
    auth_endpoint = https://api.opensciencedatacloud.org:5000/sullivan/v2.0/

Then reference that file from your Python script:

.. code-block:: python

    import os
    import pyrax

    pyrax.set_setting("identity_type", "keystone")
    pyrax.set_setting("auth_endpoint", os.environ['OS_AUTH_URL'])
    pyrax.set_credential_file("pyrax_creds")
