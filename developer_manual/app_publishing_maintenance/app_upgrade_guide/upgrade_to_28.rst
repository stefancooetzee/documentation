=======================
Upgrade to Nextcloud 28
=======================

General
-------

info.xml
^^^^^^^^

Make sure your ``appinfo/info.xml`` allows for Nextcloud 28.

.. code-block:: xml

    <dependencies>
        <nextcloud min-version="25" max-version="28" />
    </dependencies>

Front-end changes
-----------------

Added APIs
^^^^^^^^^^

* tbd

Changed APIs
^^^^^^^^^^^^

* tbd

Deprecated APIs
^^^^^^^^^^^^^^^

* tbd

Removed APIs
^^^^^^^^^^^^

* tbd

Back-end changes
----------------

Added APIs
^^^^^^^^^^

* tbd

Changed APIs
^^^^^^^^^^^^

* tbd

Deprecated APIs
^^^^^^^^^^^^^^^

* tbd

Removed APIs
^^^^^^^^^^^^

* `\OC::$server->createEventSource()` has been removed, use `\OC::$server->get(IEventSource::class)` instead. 

Fetching IEventSource source via dependency injection works only from Nextcloud 28. For older versions use \OC::$server->createEventSource() or `new OC_EventSource()`.

If you want to support Nextcloud 27 and Nextcloud 28:

.. code-block:: php

		// @TODO: Remove method_exists when min-version="28"
		if (method_exists(\OC::$server, 'createEventSource')) {
			$eventSource = \OC::$server->createEventSource();
		} else {
			$eventSource = \OCP\Server::get(IEventSource::class);
		}
