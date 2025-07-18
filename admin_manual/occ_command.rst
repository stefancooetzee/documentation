.. _occ:

=====================
Using the occ command
=====================

Nextcloud's ``occ`` command (origins from "ownCloud Console") is Nextcloud's command-line
interface. You can perform many common server operations with ``occ``, such as
installing and upgrading Nextcloud, manage users, encryption, passwords, LDAP
setting, and more.

``occ`` is in the :file:`nextcloud/` directory; for example
:file:`/var/www/nextcloud` on Ubuntu Linux. ``occ`` is a PHP script. **You must
run it as your HTTP user** to ensure that the correct permissions are maintained
on your Nextcloud files and directories.

occ command Directory
---------------------

* :ref:`http_user_label`
* :ref:`run_commands_in_maintenance_mode`
* :ref:`apps_commands_label`
* :ref:`background_jobs_selector_label`
* :ref:`config_commands_label`
* :ref:`dav_label`
* :ref:`database_conversion_label`
* :ref:`database_add_indices_label`
* :ref:`encryption_label`
* :ref:`federation_sync_label`
* :ref:`file_operations_label`
* :ref:`files_external_label`
* :ref:`integrity_check_label`
* :ref:`create_javascript_translation_files_label`
* :ref:`ldap_commands_label`
* :ref:`logging_commands_label`
* :ref:`maintenance_commands_label`
* :ref:`security_commands_label`
* :ref:`trashbin_label`
* :ref:`user_commands_label`
* :ref:`group_commands_label`
* :ref:`versions_label`
* :ref:`command_line_installation_label`
* :ref:`command_line_upgrade_label`
* :ref:`two_factor_auth_label`
* :ref:`disable_user_label`
* :ref:`system_tags_commands_label`
* :ref:`antivirus_commands_label`
* :ref:`setupchecks_commands_label`
* :ref:`share_operations_label`
* `Debugging`_

.. _http_user_label:

Run occ as your HTTP user
-------------------------

The HTTP user is different on the various Linux distributions:

* The HTTP user and group in Debian/Ubuntu is www-data.
* The HTTP user and group in Fedora/CentOS is apache.
* The HTTP user and group in Arch Linux is http.
* The HTTP user in openSUSE is wwwrun, and the HTTP group is www.

If your HTTP server is configured to use a different PHP version than the
default (/usr/bin/php), ``occ`` should be run with the same version. For
example, in CentOS 6.5 with SCL-PHP70 installed, the command looks like this::

  sudo -u apache /opt/rh/php70/root/usr/bin/php /var/www/html/nextcloud/occ

.. note:: Although the following examples make use of the ``sudo -u ... /path/to/php /path/to/occ`` method, your environment may require use of a different wrapper utility than ``sudo`` to execute the command as the appropriate user. Other common wrappers:

  * ``su --command '/path/to/php ...' username`` -- Note here that the target user specification comes at the end, and the command to execute is specified first.
  * ``runuser --user username -- /path/to/php ...`` -- This wrapper might be used in container contexts (ex: Docker / ``arm32v7/nextcloud``) where both ``sudo`` and ``su`` wrapper utilities cannot be used.

Running ``occ`` with no options lists all commands and options, like this
example on Ubuntu::

 sudo -E -u www-data php occ
 Nextcloud version 19.0.0

 Usage:
  command [options] [arguments]

 Options:
  -h, --help            Display this help message
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi            Force ANSI output
      --no-ansi         Disable ANSI output
  -n, --no-interaction  Do not ask any interactive question
      --no-warnings     Skip global warnings, show command output only
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output,
                        2 for more verbose output and 3 for debug

 Available commands:
  check                 check dependencies of the server
                        environment
  help                  Displays help for a command
  list                  Lists commands
  status                show some status information
  upgrade               run upgrade routines after installation of
                        a new release. The release has to be
                        installed before.

This is the same as ``sudo -E -u www-data php occ list``.

Run it with the ``-h`` option for syntax help::

 sudo -E -u www-data php occ -h

Display your Nextcloud version::

 sudo -E -u www-data php occ -V
   Nextcloud version 19.0.0

Query your Nextcloud server status::

 sudo -E -u www-data php occ status
   - installed: true
   - version: 19.0.0.12
   - versionstring: 19.0.0
   - edition:

``occ`` has options, commands, and arguments. Options and arguments are
optional, while commands are required. The syntax is::

 occ [options] command [arguments]

Get detailed information on individual commands with the ``help`` command, like
this example for the ``maintenance:mode`` command::

 sudo -E -u www-data php occ help maintenance:mode
 Usage:
  maintenance:mode [options]

 Options:
      --on              enable maintenance mode
      --off             disable maintenance mode
  -h, --help            Display this help message
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi            Force ANSI output
      --no-ansi         Disable ANSI output
  -n, --no-interaction  Do not ask any interactive question
      --no-warnings     Skip global warnings, show command output only
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output,
                        2 for more verbose output and 3 for debug

The ``status`` command from above has an option to define the output format.
The default is plain text, but it can also be ``json``::

 sudo -E -u www-data php occ status --output=json
 {"installed":true,"version":"19.0.0.9","versionstring":"19.0.0","edition":""}

or ``json_pretty``::

 sudo -E -u www-data php occ status --output=json_pretty
 {
    "installed": true,
    "version": "19.0.0.12",
    "versionstring": "19.0.0",
    "edition": ""
 }

This output option is available on all list and list-like commands:
``status``, ``check``, ``app:list``, ``config:list``, ``encryption:status``
and ``encryption:list-modules``

Environment variables
^^^^^^^^^^^^^^^^^^^^^

``sudo`` does not forward environment variables by default. Put the variables before the ``php`` command::

  NC_debug=true sudo -E -u www-data php occ status

Alternatively, you can ``export`` the variable or use the ``-E`` switch for ``sudo``::

  NC_debug=true sudo -E -u www-data php occ status

Enabling autocompletion
-----------------------

.. note:: Command autocompletion currently only works if the user you use to execute the occ commands has a profile.
  ``www-data`` in most cases is ``nologon`` and therefore **cannot** use this feature.

Autocompletion is available for bash (and bash based consoles).
To enable it, you have to run **one** of the following commands::

 # BASH ~4.x, ZSH
 source <(/var/www/html/nextcloud/occ _completion --generate-hook)

 # BASH ~3.x, ZSH
 /var/www/html/nextcloud/occ _completion --generate-hook | source /dev/stdin

 # BASH (any version)
 eval $(/var/www/html/nextcloud/occ _completion --generate-hook)

This will allow you to use autocompletion with the full path ``/var/www/html/nextcloud/occ <tab>``.

If you also want to use autocompletion on occ from within the directory without using the full path,
you need to specify ``--program occ`` after the ``--generate-hook``.

If you want the completion to apply automatically for all new shell sessions, add the command to your
shell's profile (eg. ``~/.bash_profile`` or ``~/.zshrc``).

.. _run_commands_in_maintenance_mode:

Run commands in maintenance mode
--------------------------------

In maintenance mode, apps are not loaded [1]_, so commands from apps are unavailable. Commands integrated into Nextcloud server are available in maintenance mode.

We discourage the use of maintenance mode unless the command explicitly prompts you to do so or unless the commands' documentation explicitly states that maintenance mode should be used.

A command may use events to communicate with other apps. An app can only react to an event when loaded. Example: The command user:delete deletes a user account. UserDeletedEvent is emitted. Calendar app implements an event listener to delete user data [2]_. In maintenance mode, the Calendar app is not loaded, and hence the user data not deleted.

.. [1] Exception: `The settings app is loaded <https://github.com/nextcloud/server/blob/75f17b60945e15effc3eea41393eef2b13937226/lib/base.php#L780>`_
.. [2] `Calendar app event listener for UserDeletedEvent <https://github.com/nextcloud/calendar/blob/87e8586971a8676dc15a90f0cd969274678b7009/lib/Listener/UserDeletedListener.php>`_

.. _apps_commands_label:

Apps commands
-------------

The ``app`` commands list, enable, and disable apps::

 app
  app:install      install selected app
  app:disable      disable an app
  app:enable       enable an app
  app:getpath      get an absolute path to the app directory
  app:list         list all available apps
  app:update       update an app or all apps
  app:remove       disable and remove an app

Download and install an app::

 sudo -E -u www-data php occ app:install twofactor_totp

Install but don't enable::

 sudo -E -u www-data php occ app:install --keep-disabled twofactor_totp

Install regardless of the Nextcloud version requirement::

 sudo -E -u www-data php occ app:install --force twofactor_totp

List all of your installed apps, and show whether they are
enabled or disabled::

 sudo -E -u www-data php occ app:list

List all of your installed and enabled (flag `--enabled`) or disabled (flag `--disabled`) apps::

 sudo -E -u www-data php occ app:list --enabled

List non-shipped installed apps only::

 sudo -E -u www-data php occ app:list --shipped false

Enable an app, for example the External Storage Support app::

 sudo -E -u www-data php occ app:enable files_external
 files_external enabled

Enable an app regardless of the Nextcloud version requirement::

 sudo -E -u www-data php occ app:enable --force files_external
 files_external enabled

Enable an app for specific groups of users::

 sudo -E -u www-data php occ app:enable --groups admin --groups sales files_external
 files_external enabled for groups: admin, sales

Enable multiple apps simultaneously::

 sudo -E -u www-data php occ app:enable app1 app2 app3
 app1 enabled
 app2 enabled
 app3 enabled

Disable an app::

 sudo -E -u www-data php occ app:disable files_external
 files_external disabled

Disable and remove an app::

 sudo -E -u www-data php occ app:remove files_external
 files_external disabled
 files_external 1.21.0 removed

Remove an app, but keep the app data::

 sudo -E -u www-data php occ app:remove --keep-data files_external
 files_external 1.21.0 removed

You can get the full filepath to an app::

    sudo -E -u www-data php occ app:getpath notifications
    /var/www/nextcloud/apps/notifications

To update an app, for instance Contacts::

    sudo -E -u www-data php occ app:update contacts

To update all apps::

    sudo -E -u www-data php occ app:update --all

To show available update(s) without updating::

    sudo -E -u www-data php occ app:update --showonly

To update an app to an unstable release, for instance News::

    sudo -E -u www-data php occ app:update --allow-unstable news

.. _background_jobs_selector_label:

Background jobs selector
------------------------

Use the ``background`` command to select which scheduler you want to use for
controlling background jobs, Ajax, Webcron, or Cron. This is the same as using
the **Cron** section on your Nextcloud Admin page::

 background
  background:ajax       Use ajax to run background jobs
  background:cron       Use cron to run background jobs
  background:webcron    Use webcron to run background jobs

This example selects Ajax::

 sudo -E -u www-data php occ background:ajax
   Set mode for background jobs to 'ajax'

The other two commands are:

* ``background:cron``
* ``background:webcron``

See :doc:`configuration_server/background_jobs_configuration` to learn more.

.. _config_commands_label:

Config commands
---------------

The ``config`` commands are used to configure the Nextcloud server::

 config
  config:app:delete      Delete an app config value
  config:app:get         Get an app config value
  config:app:set         Set an app config value
  config:import          Import a list of configs
  config:list            List all configs
  config:system:delete   Delete a system config value
  config:system:get      Get a system config value
  config:system:set      Set a system config value


While setting a configuration value, multiple options are available:

     - ``--value=VALUE`` change the configuration value
     - ``--type=TYPE`` change the type of the value. Use carefully; can break your instance
     - ``--lazy|--no-lazy`` set value as `lazy`
     - ``--sensitive|--no-sensitive`` set value as `sensitive`
     - ``--update-only`` only updates if a value is already stored

.. note::
	See `Appconfig Concepts`_ to learn more about `typed value`, `lazy` and `sensitive` flag.

.. _Appconfig Concepts: https://docs.nextcloud.com/server/latest/developer_manual/digging_deeper/config/appconfig.html#concept-overview

.. TODO ON RELEASE: Update version number above on release

You can list all configuration values with one command::

 sudo -E -u www-data php occ config:list

By default, passwords and other sensitive data are omitted from the report, so
the output can be posted publicly (e.g. as part of a bug report). In order to
generate a full backport of all configuration values the ``--private`` flag
needs to be set::

 sudo -E -u www-data php occ config:list --private

The exported content can also be imported again to allow the fast setup of
similar instances. The import command will only add or update values. Values
that exist in the current configuration, but not in the one that is being
imported are left untouched::

 sudo -E -u www-data php occ config:import filename.json

It is also possible to import remote files, by piping the input::

 sudo -E -u www-data php occ config:import < local-backup.json

.. note::

  While it is possible to update/set/delete the versions and installation
  statuses of apps and Nextcloud itself, it is **not** recommended to do this
  directly. Use the ``occ app:enable``, ``occ app:disable`` and ``occ app:update``
  commands instead.

Getting a single configuration value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

These commands get the value of a single app or system configuration::

  sudo -E -u www-data php occ config:system:get version
  19.0.0.12

  sudo -E -u www-data php occ config:app:get activity installed_version
  2.2.1

Setting a single configuration value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

These commands set the value of a single app or system configuration::

  sudo -E -u www-data php occ config:system:set logtimezone
  --value="Europe/Berlin"
  System config value logtimezone set to Europe/Berlin

  sudo -E -u www-data php occ config:app:set files_sharing
  incoming_server2server_share_enabled --value="yes"
  Config value incoming_server2server_share_enabled for app files_sharing set to yes

The ``config:system:set`` command creates the value, if it does not already
exist. To update an existing value,  set ``--update-only``::

  sudo -E -u www-data php occ config:system:set doesnotexist --value="true"
  --type=boolean --update-only
  Value not updated, as it has not been set before.

Note that in order to write a Boolean, float, or integer value to the
configuration file, you need to specify the type on your command. This
applies only to the ``config:system:set`` command. The following values are
known:

* ``boolean``
* ``float``
* ``integer``
* ``json``
* ``null``
* ``string`` (default)

When you want to e.g. disable the maintenance mode run the following command::

  sudo -E -u www-data php occ config:system:set maintenance --value=false --type=boolean
  Nextcloud is in maintenance mode - no app have been loaded
  System config value maintenance set to boolean false

Setting an array configuration value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Some configurations (e.g. the trusted domain setting) are an array of data.
In this case, ``config:system:get`` for this key will return multiple values::

  sudo -E -u www-data php occ config:system:get trusted_domains
  localhost
  nextcloud.local
  sample.tld

To set one of multiple values, you need to specify the array index as the
second ``name`` in the ``config:system:set`` command, separated by a
space. For example, to replace ``sample.tld`` with ``example.com``,
``trusted_domains => 2`` needs to be set::

  sudo -E -u www-data php occ config:system:set trusted_domains 2 --value=example.com
  System config value trusted_domains => 2 set to string example.com

  sudo -E -u www-data php occ config:system:get trusted_domains
  localhost
  nextcloud.local
  example.com

Alternatively, you can set the entry array at once by using the ``json`` type::

  sudo -E -u www-data php occ config:system:set trusted_domains --type json --value '["nextcloud.local","example.com"]'
  System config value trusted_domains set to json ["nextcloud.local","example.com"]

  sudo -E -u www-data php occ config:system:get trusted_domains
  nextcloud.local
  example.com

Setting a hierarchical configuration value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Some configurations use hierarchical data. For example, the settings
for the Redis cache would look like this in the ``config.php`` file::

  'redis' => array(
    'host' => '/var/run/redis/redis.sock',
    'port' => 0,
    'dbindex' => 0,
    'password' => 'secret',
    'timeout' => 1.5,
  )

Setting such hierarchical values works similarly to setting an array
value above. For this Redis example, use the following commands::

  sudo -E -u www-data php occ config:system:set redis host \
  --value=/var/run/redis/redis.sock
  sudo -E -u www-data php occ config:system:set redis port --value=0
  sudo -E -u www-data php occ config:system:set redis dbindex --value=0
  sudo -E -u www-data php occ config:system:set redis password --value=secret
  sudo -E -u www-data php occ config:system:set redis timeout --value=1.5

Alternatively, you can set the entry configuration at once by using the ``json`` type::

  sudo -E -u www-data php occ config:system:set redis --type json --value '{"host":"/var/run/redis/redis.sock","port":0,"dbindex":0,"password":"secret","timeout":1.5}'


Deleting a single configuration value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

These commands delete the configuration of an app or system configuration::

  sudo -E -u www-data php occ config:system:delete maintenance:mode
  System config value maintenance:mode deleted

  sudo -E -u www-data php occ config:app:delete appname provisioning_api
  Config value provisioning_api of app appname deleted

The delete command will by default not complain if the configuration was not set
before. If you want to be notified in that case, set the
``--error-if-not-exists`` flag::

  sudo -E -u www-data php occ config:system:delete doesnotexist
  --error-if-not-exists
  Config provisioning_api of app appname could not be deleted because it did not
  exist

.. _dav_label:

Dav commands
------------

Manage addressbooks and calendars::

 dav
  dav:create-addressbook          Create a dav addressbook
  dav:create-calendar             Create a dav calendar
  dav:create-subscription         Create a dav subscription
  dav:delete-calendar             Delete a dav calendar
  dav:delete-subscription         Delete a calendar subscription for a user
  dav:fix-missing-caldav-changes  Insert missing calendarchanges rows for existing events
  dav:list-addressbooks           List all addressbooks of a user
  dav:list-calendars              List all calendars of a user
  dav:list-subscriptions          List all calendar subscriptions for a user
  dav:move-calendar               Move a calendar from an user to another
  dav:remove-invalid-shares       Remove invalid dav shares
  dav:retention:clean-up
  dav:send-event-reminders        Sends event reminders
  dav:sync-birthday-calendar      Synchronizes the birthday calendar
  dav:sync-system-addressbook     Synchronizes users to the system addressbook


Manage addressbooks
^^^^^^^^^^^^^^^^^^^

List all addressbooks of a user
"""""""""""""""""""""""""""""""

``dav:list-addressbooks <uid>``

This example will list all addressbooks for user annie: ::

 sudo -E -u www-data php occ dav:list-addressbooks annie

Create a addressbook for a user
"""""""""""""""""""""""""""""""

``dav:create-addressbook <user> <name>``

This example creates the addressbook ``mollybook`` for the user molly: ::

 sudo -E -u www-data php occ dav:create-addressbook molly mollybook


Manage calendars
^^^^^^^^^^^^^^^^

List all calendars of a user
""""""""""""""""""""""""""""

``dav:list-calendars <uid>``

This example will list all calendars for user annie: ::

 sudo -E -u www-data php occ dav:list-calendars annie

Create a calendar for a user
""""""""""""""""""""""""""""

``dav:create-calendar <user> <name>``


This example creates the calendar ``mollycal`` for the user molly: ::

 sudo -E -u www-data php occ dav:create-calendar molly mollycal

Delete a calendar for a user
""""""""""""""""""""""""""""

``dav:delete-calendar [--birthday] [-f|--force] [--] <uid> [<name>]`` deletes the
calendar named ``name`` (or the birthday calendar if ``--birthday`` is
specified) of the user ``uid``. You can use the force option ``-f`` or
``--force`` to delete the calendar instead of moving it to the trashbin.

This example will delete the calendar mollycal of user molly::

 sudo -E -u www-data php occ dav:delete-calendar molly mollycal

This example will delete the birthday calendar of user molly::

 sudo -E -u www-data php occ dav:delete-calendar --birthday molly


Move a calendar of a user
"""""""""""""""""""""""""

.. note:: Note that this will change existing share URLs.

``dav:move-calendar [-f|--force] [--] <name> <sourceuid> <destinationuid>`` allows the admin to move a calendar named ``name`` from a user ``sourceuid`` to the user ``destinationuid``. You can use the force option `-f` to enforce the move if there are conflicts with existing shares. The system will also generate a new unique calendar name in case there is a conflict over the destination user.


This example will move calendar named personal from user dennis to user sabine: ::

 sudo -E -u www-data php occ dav:move-calendar personal dennis sabine

Misc
""""

``dav:fix-missing-caldav-changes [<user>]`` attempts to restore calendar sync changes when data in the calendarchanges table has been lost. If the user ID is omitted, the command runs for all users, which may take some time to complete.

``dav:retention:clean-up`` deletes elements from the CalDAV trash that are due for removal.

``dav:remove-invalid-shares`` removes invalid shares that were created due to a bug in the calendar app.

``dav:send-event-reminders`` is a command that should be called regularly through a dedicated
cron job to send event reminder notifications. See :doc:`../groupware/calendar` for more information on how to use this command.


Manage calendar subscriptions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

List all calendar subscriptions of a user
"""""""""""""""""""""""""""""""""""""""""

``dav:list-subscriptions <uid>``

This example will list all calendar subscriptions for user annie: ::

 sudo -E -u www-data php occ dav:list-subscriptions annie


Create a calendar subscription for a user
"""""""""""""""""""""""""""""""""""""""""

``dav:create-subscription <user> <name> <url> [<color>]``

This example creates the subscription for the lunar calendar ``Lunar Calendar`` for the user molly: ::

 sudo -E -u www-data php occ dav:create-subscription molly "Lunar Calendar" webcal://cantonbecker.com/astronomy-calendar/astrocal.ics

Optionally, a color for the new subscription calendar can be passed as a HEX color code::

 sudo -E -u www-data php occ dav:create-subscription molly "Lunar Calendar" calendar webcal://cantonbecker.com/astronomy-calendar/astrocal.ics "#ff5733"

If not set, the theming default color will be used.


Delete a calendar subscription for a user
"""""""""""""""""""""""""""""""""""""""""

``dav:delete-subscription <uid> <uri>``

This example deletes the subscription for the lunar calendar ``Lunar Calendar`` for the user molly: ::

 sudo -E -u www-data php occ dav:delete-subscription molly "Lunar Calendar"


.. _dav-sync-system-address-book:

Sync system address book
^^^^^^^^^^^^^^^^^^^^^^^^

``dav:sync-system-addressbook`` synchronizes all users to the :ref:`system
address book<system-address-book>`::

 sudo -E -u www-data php occ dav:sync-system-addressbook

Sync birthday calendar
^^^^^^^^^^^^^^^^^^^^^^

``dav:sync-birthday-calendar [<user>]`` adds all birthdays to your calendar from
addressbooks shared with you. This example syncs to your calendar from user bernie: ::

 sudo -E -u www-data php occ dav:sync-birthday-calendar bernie


.. _database_conversion_label:

Database conversion
-------------------

The SQLite database is good for testing, and for Nextcloud servers with small
single-user workloads that do not use sync clients, but production servers with
multiple users should use MariaDB, MySQL, or PostgreSQL. You can use ``occ`` to
convert from SQLite to one of these other databases.

::

 db
  db:convert-type           Convert the Nextcloud database to the newly
                            configured one
  db:generate-change-script generates the change script from the current
                            connected db to db_structure.xml

You need:

* Your desired database and its PHP connector installed.
* The login and password of a database admin user.
* The database port number, if it is a non-standard port.

This is example converts SQLite to MySQL/MariaDB::

 sudo -E -u www-data php occ db:convert-type mysql oc_dbuser 127.0.0.1
 oc_database

For a more detailed explanation see
:doc:`../configuration_database/db_conversion`

.. _database_add_indices_label:

Add missing indices
-------------------

It might happen that we add from time to time new indices to already existing database tables,
for example to improve performance. In order to check your database for missing indices run
following command::

 sudo -E -u www-data php occ db:add-missing-indices

Use option ``--dry-run`` to output the SQL queries without running them.

.. _encryption_label:

Encryption
----------

``occ`` includes a complete set of commands for managing encryption::

 encryption
  encryption:change-key-storage-root   Change key storage root
  encryption:decrypt-all               Disable server-side encryption and
                                       decrypt all files
  encryption:disable                   Disable encryption
  encryption:drop-legacy-filekey       Drop legacy filekey for files still using it
  encryption:enable                    Enable encryption
  encryption:enable-master-key         Enable the master key. Only available
                                       for fresh installations with no existing
                                       encrypted data! There is also no way to
                                       disable it again.
  encryption:encrypt-all               Encrypt all files for all users
  encryption:list-modules              List all available encryption modules
  encryption:set-default-module        Set the encryption default module
  encryption:show-key-storage-root     Show current key storage root
  encryption:status                    Lists the current status of encryption

``encryption:status`` shows whether you have active encryption, and your default
encryption module. To enable encryption you must first enable the Encryption
app, and then run ``encryption:enable``::

 sudo -E -u www-data php occ app:enable encryption
 sudo -E -u www-data php occ encryption:enable
 sudo -E -u www-data php occ encryption:status
  - enabled: true
  - defaultModule: OC_DEFAULT_MODULE

``encryption:change-key-storage-root`` is for moving your encryption keys to a
different folder. It takes one argument, ``newRoot``, which defines your new
root folder::

 sudo -E -u www-data php occ encryption:change-key-storage-root /etc/oc-keys

You can see the current location of your keys folder::

 sudo -E -u www-data php occ encryption:show-key-storage-root
 Current key storage root:  default storage location (data/)

``encryption:list-modules`` displays your available encryption modules. You will
see a list of modules only if you have enabled the Encryption app. Use
``encryption:set-default-module [module name]`` to set your desired module.

``encryption:encrypt-all`` encrypts all data files for all users. You must first
put your Nextcloud server into :ref:`maintenance
mode<maintenance_commands_label>` to prevent any user activity until encryption
is completed.

``encryption:decrypt-all`` decrypts all user data files, or optionally a single
user::

 sudo -E -u www-data php occ encryption:decrypt freda

Users must have enabled recovery keys on their Personal pages.

Note that if you do not have master key/recovery key enabled, you can ONLY
decrypt files per user, one user at a time and NOT when in maintenance mode.
You will need the users' password to decrypt the files.

Use ``encryption:disable`` to disable your encryption module. You must first put
your Nextcloud server into :ref:`maintenance mode <maintenance_commands_label>`
to prevent any user activity.

``encryption:enable-master-key`` creates a new master key, which is used for all
user data instead of individual user keys. This is especially useful to enable
single-sign on. Use this only on fresh installations with no existing data, or
on systems where encryption has not already been enabled. It is not possible to
disable it.

``encryption:drop-legacy-filekey`` scans the files for the legacy filekey
format using RC4 and get rid of it (if master key is enabled). The operation can
be quite slow as it needs to rewrite each encrypted file. If you do not do it files
will be migrated to drop their legacy filekey on the first modification. If you have
old files from Nextcloud<25 still using base64 encoding this will migrate them to the
binary format and save about 33% disk space.

See :doc:`../configuration_files/encryption_configuration` to learn more.

.. _federation_sync_label:

Federation sync
---------------

.. note::
  This command is only available when the "Federation" app (``federation``) is
  enabled.

Synchronize the addressbooks of all federated Nextcloud servers::

 federation:sync-addressbooks  Synchronizes addressbooks of all
                               federated clouds

In Nextcloud, servers connected with federation shares can share user
address books, and auto-complete usernames in share dialogs. Use this command
to synchronize federated servers::

  sudo -E -u www-data php occ federation:sync-addressbooks

.. _file_operations_label:

File operations
---------------

Available ``occ`` commands for the ``files`` namespace::

  files:cleanup                    cleanup filecache
  files:copy                       Copy a file or folder
  files:delete                     Delete a file or folder
  files:get                        Get the contents of a file
  files:move                       Move a file or folder
  files:object:delete              Delete an object from the object store
  files:object:get                 Get the contents of an object
  files:object:put                 Write a file to the object store
  files:put                        Write contents of a file
  files:recommendations:recommend
  files:reminders                  List file reminders
  files:repair-tree                Try and repair malformed filesystem tree structures
  files:scan                       rescan filesystem
  files:scan-app-data              rescan the AppData folder
  files:transfer-ownership         All files and folders are moved to another user - outgoing shares and incoming user file shares (optionally) are moved as well.

.. _occ_files_scan_label:

Scan
^^^^

The ``files:scan`` command scans for new files and updates the file cache. You
may rescan all files, per-user, a space-delimited list of users, and limit the
search path. If not using ``--quiet``, statistics will be shown at the end of
the scan::

 sudo -E -u www-data php occ files:scan --help
 Description:
   rescan filesystem

 Usage:
   files:scan [options] [--] [<user_id>...]

 Arguments:
   user_id                  will rescan all files of the given user(s)

 Options:
       --output[=OUTPUT]    Output format (plain, json or json_pretty, default is plain) [default: "plain"]
   -p, --path=PATH          limit rescan to this path, eg. --path="/alice/files/Music", the user_id is determined by the path and the user_id parameter and --all are ignored
       --generate-metadata  Generate metadata for all scanned files
       --all                will rescan all files of all known users
       --unscanned          only scan files which are marked as not fully scanned
       --shallow            do not scan folders recursively
       --home-only          only scan the home storage, ignoring any mounted external storage or share
   -h, --help               Display help for the given command. When no command is given display help for the list command
   -q, --quiet              Do not output any message
   -V, --version            Display this application version
       --ansi|--no-ansi     Force (or disable --no-ansi) ANSI output
   -n, --no-interaction     Do not ask any interactive question
       --no-warnings        Skip global warnings, show command output only
   -v|vv|vvv, --verbose     Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug


Verbosity levels of ``-vv`` or ``-vvv`` are automatically reset to ``-v``

Note for option ``--unscanned``:
In general there is a background job (through cron) that will do that scan periodically.
The ``--unscanned`` option makes it possible to trigger this from the CLI.

When using the ``--path`` option, the path must consist of following
components::

  "user_id/files/path"
    or
  "user_id/files/mount_name"
    or
  "user_id/files/mount_name/path"

where the term ``files`` is mandatory.

Example::

  --path="/alice/files/Music"

In the example above, the user_id ``alice`` is determined implicitly from the
path component given.

The ``--path``, ``--all`` and ``[user_id]`` parameters are exclusive - only
one must be specified.

Scan appdata
^^^^^^^^^^^^^

Appdata is a folder inside of the data directory which contains files that
are shared between users and can be put there by the server or apps like
avatar images, file previews and cached CSS files for example.

Since the regular files scan only operates on user files the ``occ files:scan-app-data``
command will check the appdata directory and make sure that the filecache is consistent
with the files on the actual storage.::

  Usage:
    files:scan-app-data [options] [--] [<folder>]

  Arguments:
    folder                 The appdata subfolder to scan [default: ""]

.. _occ_cleanup_previews:

Cleanup previews
^^^^^^^^^^^^^^^^

``preview:cleanup`` removes all of the server's preview files. This is useful
when changing the previews configuration (sizes, quality or file), and especially
on systems using Object Storage as Primary Storage where the ``appdata_xxx/preview``
folder can't simply be deleted.

See :doc:`configuration_files/previews_configuration`.


Cleanup
^^^^^^^

``files:cleanup`` tidies up the server's file cache by deleting all file
entries that have no matching entries in the storage table.

Repair-Tree
^^^^^^^^^^^

``files:repair-tree`` try and repair malformed filesystem tree structures.
If for any reason the path of an entry in the filecache doesn't match with
it's expected path, based on the path of it's parent node, you end up with an
entry in the filecache that exists in different places based on how the entry
is generated. For example, if while listing folder ``/foo`` it contains a file
``bar.txt``, but when trying to do anything with ``/foo/bar.txt`` the file
doesn't exists.

This command attempts to repair such entries by querying for entries where the path
doesn't match the expected path based on it's parent path and filename and resets it's
path to the expected one.

.. _occ_files_sanitize_filenames:

Sanitize filenames
^^^^^^^^^^^^^^^^^^

This command allows to automatically rename files not matching the current file naming constraints,
for example after enabling the :ref:`Windows compatible filenames <windows_compatible_filenames>`::

 Usage:
   files:sanitize-filenames [options] [--] [<user_id>...]

 Arguments:
   user_id                                 Limit filename sanitizing to files given user(s) have access to

 Options:
      --dry-run                            Do not actually rename any files but just check filenames.
  -c, --char-replacement=CHAR-REPLACEMENT  Replacement for invalid character (by default space, underscore or dash is used)

When running this command without parameters it will scan all files of all users
for filenames not comply with the current filename constraints and try to automatically
rename those files.
Invalid characters will be replaced by default with either a space, underscore, or dash
depending on which characters are allowed.
If your constraints forbid all of them, then you have to provide an character replacement
yourself by specifying the ``--char-replacement`` option.

The ``--dry-run`` option allows to perform the sanitizing without the actual renaming,
this is useful for estimating the execution time and to get an overview on what renaming
actions will be performed.

Transfer
^^^^^^^^

The command ``occ files:transfer-ownership`` can be used to transfer files from one user to another::

 Usage:
   files:transfer-ownership [options] [--] <source-user> <destination-user>

 Arguments:
   source-user                                                owner of files which shall be moved
   destination-user                                           user who will be the new owner of the files

 Options:
       --path=PATH                                            selectively provide the path to transfer. For example --path="folder_name" [default: ""]
       --move                                                 move data from source user to root directory of destination user, which must be empty
       --transfer-incoming-shares[=TRANSFER-INCOMING-SHARES]  transfer incoming user file shares to destination user. Usage: --transfer-incoming-shares=1 (value required) [default: "2"]

You may transfer all files and shares from one user to another. This is useful
before removing a user::

 sudo -E -u www-data php occ files:transfer-ownership <source-user> <destination-user>

The transferred files will appear inside a new sub-directory in the destination user's home.

.. note::
  Unless server side encryption is enabled, **the command will init the <destination-user> file system** in Nextcloud versions **22.2.6, 23.0.3 and since 24**. When it is unable to create the user's folder in the data directory it will show the following error: ``unable to rename, destination directory is not writable``. Before 22.2.6 the command ``occ files:transfer-ownership`` would only work after the user has logged in for the first time.

If the destination user has no files at all (empty home), it is possible to also transfer all the source user's files by passing ``--move``::

 sudo -E -u www-data php occ files:transfer-ownership --move <source-user> <destination-user>

In this case no sub-directory is created and all files will appear directly in the root of the user's home.

It is also possible to transfer only one directory along with its contents. This can be useful to restructure your organization or quotas. The ``--path`` argument is given as the path to the directory as seen from the source user::

 sudo -E -u www-data php occ files:transfer-ownership --path="path_to_dir" <source-user> <destination-user>

Incoming shares are not moved by default because the sharing user holds the ownership of the respective files. There is however an option to enable moving incoming shares.

In case the incoming shares must be transferred as well, use the argument ``--transfer-incoming-shares`` with ``0`` or ``1`` as parameters ::

 sudo -E -u www-data php occ files:transfer-ownership --transfer-incoming-shares=1 --path="path_to_dir" <source-user> <destination-user>

As an alternative, the system configuration option ``transferIncomingShares`` in config.php can be set to ``true`` to always transfer incoming shares.

The command line option ``--transfer-incoming-shares`` overwrites the config.php option ``transferIncomingShares``. For example, ``'transferIncomingShares => true`` can be overwritten by: ::

 sudo -E -u www-data php occ files:transfer-ownership --transfer-incoming-shares=0 <source-user> <destination-user>

Users may also transfer files or folders selectively by themselves.
See `user documentation <https://docs.nextcloud.com/server/latest/user_manual/en/files/transfer_ownership.html>`_ for details.

.. TODO ON RELEASE: Update version number above on release

.. _occ_files_windows_filenames:

Toggle Windows compatibility
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The command ``occ files:windows-compatible-filenames`` can be used to toggle
enforcing :ref:`Windows compatible filenames <windows_compatible_filenames>`::

 Usage:
   files:windows-compatible-filenames [options]

 Options:
       --enable                  enable enforcing windows compatible filenames
       --disable                 disable enforcing windows compatible filenames

.. _occ_sharing_label:

Files Sharing
-------------

Commands for handling shares::

 sharing
  sharing:cleanup-remote-storages  Cleanup shared storage entries that have no matching entry in the shares_external table
  sharing:expiration-notification  Notify share initiators when a share will expire the next day
  sharing:delete-orphan-shares     Delete shares where the owner no longer has access to the file or the file is not available anymore

.. _files_external_label:

Files external
--------------

These commands are used for managing Nextcloud's *External Storage* feature. In
addition to replicating the configuration capabilities in the Web UI, additional
capabilities include exporting / importing configurations, scanning *External
Storage* mounts that require login credentials, and configuring update notifications
(if supported by the storage type).

.. note::
  These commands are only available when the "External storage support" app
  (``files_external``) is enabled.

Available commands for the "files_external" namespace::

  files_external:applicable   Manage applicable users and groups for a mount
  files_external:backends     Show available authentication and storage backends
  files_external:config       Manage backend configuration for a mount
  files_external:create       Create a new mount configuration
  files_external:delete       Delete an external mount
  files_external:export       Export mount configurations
  files_external:import       Import mount configurations
  files_external:list         List configured admin or personal mounts
  files_external:notify       Listen for active update notifications for a configured external mount
  files_external:option       Manage mount options for a mount
  files_external:scan         Scan an external storage for changed files
  files_external:verify       Verify mount configuration
  files_external:dependencies Check for any missing dependencies needed for mounting external storages

``files_external:scan`` provides the ability to provide a username and/or password for cases where login credentials are used.

Use ``files_external:export`` to export all admin mounts to stdout, and
``files_external:export [user_id]`` to export the mounts of the specified
Nextcloud user.

Use ``files_external:import [filename]`` to import legacy JSON configurations,
and to copy external mount configurations to another Nextcloud server.

.. _integrity_check_label:

Integrity check
---------------

Apps which have a ``Featured`` tag MUST be code signed with Nextcloud. Unsigned featured apps won't be installable anymore. Code signing is optional for all third-party applications::

 integrity
  integrity:check-app                 Check app integrity using a signature.
  integrity:check-core                Check core integrity using a signature.
  integrity:sign-app                  Signs an app using a private key.
  integrity:sign-core                 Sign core using a private key

After creating your signing key, sign your app like this example::

 sudo -E -u www-data php occ integrity:sign-app --privateKey=/Users/lukasreschke/contacts.key --certificate=/Users/lukasreschke/CA/contacts.crt --path=/Users/lukasreschke/Programming/contacts

Verify your app::

  sudo -E -u www-data php occ integrity:check-app --path=/pathto/app appname

When it returns nothing, your app is signed correctly. When it returns a message then there is an error. See `Code Signing
<https://docs.nextcloud.com/server/latest/developer_manual/app_publishing_maintenance/code_signing.html#how-to-get-your-app-signed>`_ in the Developer manual for more detailed information.

.. TODO ON RELEASE: Update version number above on release

``integrity:sign-core`` is for Nextcloud core developers only.

See :doc:`../issues/code_signing` to learn more.

.. _create_javascript_translation_files_label:

l10n, create JavaScript translation files for apps
--------------------------------------------------

This command is for app developers to update their translation mechanism from
ownCloud 7 to Nextcloud.

.. _ldap_commands_label:

LDAP commands
-------------

.. note::
  These commands are only available when the "LDAP user and group backend" app
  (``user_ldap``) is enabled.

These LDAP commands appear only when you have enabled the LDAP app. Then
you can run the following LDAP commands with ``occ``::

 ldap
  ldap:check-user               checks whether a user exists on LDAP.
  ldap:check-group              checks whether a group exists on LDAP.
  ldap:create-empty-config      creates an empty LDAP configuration
  ldap:delete-config            deletes an existing LDAP configuration
  ldap:search                   executes a user or group search
  ldap:set-config               modifies an LDAP configuration
  ldap:show-config              shows the LDAP configuration
  ldap:show-remnants            shows which users are not available on
                                LDAP anymore, but have remnants in
                                Nextcloud.
  ldap:test-config              tests an LDAP configuration
  ldap:test-user-settings       runs tests and show information about user
                                related LDAP settings

ldap\:search
^^^^^^^^^^^^

Search for an LDAP user, using this syntax

 sudo -E -u www-data php occ ldap:search [--group] [--offset="..."]
 [--limit="..."] search

Searches will match at the beginning of the attribute value only. This example
searches for givenNames that start with "rob"::

 sudo -E -u www-data php occ ldap:search "rob"

This will find robbie, roberta, and robin. Broaden the search to find, for
example, ``jeroboam`` with the asterisk wildcard::

 sudo -E -u www-data php occ ldap:search "*rob"

User search attributes are set with ``ldap:set-config``
(below). For example, if your search attributes are
``givenName`` and ``sn`` you can find users by first name + last name very
quickly. For example, you'll find Terri Hanson by searching for ``te ha``.
Trailing whitespaces are ignored.

Check if an LDAP user exists. This works only if the Nextcloud server is
connected to an LDAP server::

 sudo -E -u www-data php occ ldap:check-user robert

ldap\:check-user
^^^^^^^^^^^^^^^^

Will not run a check when it finds a disabled LDAP
connection. This prevents users that exist on disabled LDAP connections from
being marked as deleted. If you know for certain that the user you are searching for
is not in one of the disabled connections, and exists on an active connection,
use the ``--force`` option to force it to check all active LDAP connections::

 sudo -E -u www-data php occ ldap:check-user --force robert

ldap\:check-group
^^^^^^^^^^^^^^^^^

Checks whether a group still exists in the LDAP directory.
Use with ``--update`` to update the group membership cache on the Nextcloud side::

 sudo -E -u www-data php occ ldap:check-group --update mygroup

ldap\:create-empty-config
^^^^^^^^^^^^^^^^^^^^^^^^^

Creates an empty LDAP configuration. The first one you create
has ``configID`` ``s01``, and all subsequent configurations
that you create are automatically assigned IDs::

 sudo -E -u www-data php occ ldap:create-empty-config
    Created new configuration with configID 's01'

Then you can list and view your configurations::

 sudo -E -u www-data php occ ldap:show-config

And view the configuration for a single configID::

 sudo -E -u www-data php occ ldap:show-config s01

ldap\:delete-config
^^^^^^^^^^^^^^^^^^^

Deletes an existing LDAP configuration::

 sudo -E -u www-data php occ ldap:delete  s01
 Deleted configuration with configID 's01'

ldap\:set-config
^^^^^^^^^^^^^^^^

This command is for manipulating configurations, like this
example that sets search attributes::

 sudo -E -u www-data php occ ldap:set-config s01 ldapAttributesForUserSearch
 "cn;givenname;sn;displayname;mail"

ldap\:test-config
^^^^^^^^^^^^^^^^^

Tests whether your configuration is correct and can bind to
the server::

 sudo -E -u www-data php occ ldap:test-config s01
 The configuration is valid and the connection could be established!

ldap\:test-user-settings
^^^^^^^^^^^^^^^^^^^^^^^^

Tests user-related LDAP settings::

  sudo -E -u www-data php occ ldap:test-user-settings "cn=philip j. fry,ou=people,dc=planetexpress,dc=com" --group "Everyone"

  User cn=philip j. fry,ou=people,dc=planetexpress,dc=com is mapped with account name fry.
  Known UUID is ce6cd914-71d5-103f-95a8-ad2dab17b2f9.
  Configuration prefix is s01

  Attributes set in configuration:
  - ldapExpertUsernameAttr: uid
  - ldapUuidUserAttribute: auto
  - ldapEmailAttribute: mail
  - ldapUserDisplayName: cn

  Attributes fetched from LDAP using filter (|(objectclass=inetOrgPerson)):
  - entryuuid: ["ce6cd914-71d5-103f-95a8-ad2dab17b2f9"]
  - uid: ["fry"]
  - mail: ["fry@planetexpress.com"]
  - cn: ["Philip J. Fry"]

  Detected UUID attribute: entryuuid

  UUID for cn=philip j. fry,ou=people,dc=planetexpress,dc=com: ce6cd914-71d5-103f-95a8-ad2dab17b2f9

  Group information:
  Configuration:
  - ldapGroupFilter: (|(objectclass=groupOfNames))
  - ldapGroupMemberAssocAttr: member

  Primary group:
  Group from gidNumber:
  All known groups: ["Ship crew", "Everyone"]
  MemberOf usage: off (0,1)

  Group Everyone:
  Group cn=everyone,ou=groups,dc=planetexpress,dc=com is mapped with name Everyone.
  Known UUID is ce8b61c2-71d5-103f-95af-ad2dab17b2f9.
  Members: ["bender", "fry", "leela"]

ldap\:show-remnants
^^^^^^^^^^^^^^^^^^^

Used to cleaning up the LDAP mappings table, and is
documented in :doc:`../configuration_user/user_auth_ldap_cleanup`.

.. _logging_commands_label:

Logging commands
----------------

These commands view and configure your Nextcloud logging preferences::

 log
  log:file        manipulate Nextcloud logging backend
  log:manage      manage logging configuration
  log:tail        tail the nextcloud logfile [requires app "Log Reader" to be enabled]
  log:watch       watch the nextcloud logfile live [requires app "Log Reader" to be enabled]

Run ``log:file [--] [--enable] [--file] [--rotate-size]`` to see your current logging status::

 sudo -E -u www-data php occ log:file
 Log backend Nextcloud: enabled
 Log file: /opt/nextcloud/data/nextcloud.log
 Rotate at: disabled

* ``--enable`` turns on logging.
* ``--file`` sets a different log file path.
* ``--rotate-size`` sets your rotation by log file size in bytes with; 0 disables rotation.

``log:manage [--backend] [--level] [--timezone]`` sets your logging backend, log level, and timezone. The defaults
are ``file``, ``warning``, and ``UTC``. Available options are:

* ``--backend [file, syslog, errorlog, systemd]``
* ``--level [debug|info|warning|error|fatal]``
* ``--timezone`` according to https://www.php.net/manual/en/timezones.php

.. _maintenance_commands_label:

Maintenance commands
--------------------

Use these commands when you upgrade Nextcloud, manage encryption, perform
backups and other tasks that require locking users out until you are finished::

 maintenance
  maintenance:data-fingerprint        update the systems data-fingerprint after a backup is restored
  maintenance:mimetype:update-db      Update database mimetypes and update filecache
  maintenance:mimetype:update-js      Update mimetypelist.js
  maintenance:mode                    set maintenance mode
  maintenance:repair                  repair this installation
  maintenance:theme:update            Apply custom theme changes
  maintenance:update:htaccess         Updates the .htaccess file
  maintenance:repair-share-owner      Fix some shares owner if it felt out of sync

``maintenance:mode`` locks the sessions of all logged-in users, including
administrators, and displays a status screen warning that the server is in
maintenance mode. Users who are not already logged in cannot log in until
maintenance mode is turned off. When you take the server out of maintenance mode
logged-in users must refresh their Web browsers to continue working::

 sudo -E -u www-data php occ maintenance:mode --on
 sudo -E -u www-data php occ maintenance:mode --off

After restoring a backup of your data directory or the database, you should always
call ``maintenance:data-fingerprint`` once. This changes the ETag for all files
in the communication with sync clients, allowing them to realize a file was modified.

The ``maintenance:repair`` command runs automatically during upgrades to clean
up the database, so while you can run it manually there usually isn't a need
to::

 sudo -E -u www-data php occ maintenance:repair

``maintenance:mimetype:update-db`` updates the Nextcloud database and file cache
with changed mimetypes found in ``config/mimetypemapping.json``. Run this
command after modifying ``config/mimetypemapping.json``. If you change a
mimetype, run ``maintenance:mimetype:update-db --repair-filecache`` to apply the
change to existing files.

Run the ``maintenance:theme:update`` command if the icons of your custom theme are not
updated correctly. This updates the mimetypelist.js and clears the image cache.

.. _security_commands_label:

Security
--------

Use these commands to manage server-wide security related parameters. Currently this
includes :doc:`configuration_server/bruteforce_configuration` and SSL certificates (the latter are useful when
creating federation connections with other Nextcloud servers that use self-signed certificates::

 security
  security:bruteforce:attempts  show bruteforce attempts status for a given IP address
  security:bruteforce:reset     resets bruteforce attempts for a given IP address
  security:certificates         list trusted certificates
  security:certificates:import  import trusted certificate
  security:certificates:remove  remove trusted certificate

Reset an IP::

 sudo -E -u www-data php occ security:bruteforce:reset [IP address]


This example lists your installed certificates::

 sudo -E -u www-data php occ security:certificates

Import a new certificate::

 sudo -E -u www-data php occ security:certificates:import /path/to/certificate

Remove a certificate::

 sudo -E -u www-data php occ security:certificates:remove [certificate name]

Status
------

Use the status command to retrieve information about the current installation::

 $ sudo -E -u www-data php occ status
   - installed: true
   - version: 25.0.2.3
   - versionstring: 25.0.2
   - edition:
   - maintenance: false
   - needsDbUpgrade: false
   - productname: Nextcloud
   - extendedSupport: false

This information can also be formatted via JSON instead of plain text::

 $ sudo -E -u www-data php occ status --output=json_pretty
 {
     "installed": true,
     "version": "25.0.2.3",
     "versionstring": "25.0.2",
     "edition": "",
     "maintenance": false,
     "needsDbUpgrade": false,
     "productname": "Nextcloud",
     "extendedSupport": false
 }

Status return code
^^^^^^^^^^^^^^^^^^

And finally, the ``-e`` (for exit code) parameter can be used to check
the state of the nextcloud installation via return code::

 $ sudo -E -u www-data php occ status -e
 $ echo $?
 0
 $ sudo -E -u www-data php occ maintenance:mode --on
 Maintenance mode enabled
 $ sudo -E -u www-data php occ status -e
 $ echo $?
 1
 $ sudo -E -u www-data php occ maintenance:mode --off
 Maintenance mode disabled
 $ sudo -E -u www-data php occ status -e
 $ echo $?
 0

Note that by default there is no output when run with ``-e``. This is
intentional, so it can be used in scripts, monitoring checks, and systemd
units.

+-------------+--------------------------------------------------------+
| Return code | Description                                            |
+=============+========================================================+
| 0           | normal operation                                       |
+-------------+--------------------------------------------------------+
| 1           | maintenance mode is enabled; the instance is currently |
|             | unavailable to users.                                  |
+-------------+--------------------------------------------------------+
| 2           | ``sudo -E -u www-data php occ upgrade`` is required    |
+-------------+--------------------------------------------------------+

.. _trashbin_label:

Trashbin
--------

These commands allow for manually managing various aspects of the trash bin (deleted files)::

 trashbin
  trashbin:cleanup      Permanently remove deleted files
  trashbin:expire       Expires the users trashbin
  trashbin:size         Configure the target trashbin size
  trashbin:restore      Restore all deleted files according to the given filters

.. note::
  These commands are only available when the "Deleted files" app
  (``files_trashbin``) is enabled.

The ``trashbin:cleanup  [--all-users] [--] [<user_id>...]`` command removes the deleted files of the specified
users in a space-delimited list, or all users if --all-users is specified.

This example permanently removes the deleted files of all users::

  sudo -E -u www-data php occ trashbin:cleanup --all-users
  Remove all deleted files for all users
  Remove deleted files for users on backend Database
   freda
   molly
   stash
   rosa
   edward

This example permanently removes the deleted files of users molly and freda::

 sudo -E -u www-data php occ trashbin:cleanup molly freda
 Remove deleted files of   molly
 Remove deleted files of   freda

The ``trashbin:restore  [--all-users] [--scope[=SCOPE]] [--since[=SINCE]] [--until[=UNTIL]] [--dry-run] [--] [<user_id>...]`` command restores the deleted files of the specified
users in a space-delimited list, or all users if --all-users is specified.

This example restores the deleted user-files of all users::

 sudo -E -u www-data php occ trashbin:restore --all-users

This example restores the deleted user-files of users molly and freda::

 sudo -E -u www-data php occ trashbin:restore molly freda

The ``--scope`` option can be used to limit the restore to a specific scope.
Possible values are "user", "groupfolders" or "all" [default: "user"].

This example restores the deleted files of all groupfolders which are visible to the user freda::

  sudo -E -u www-data php occ trashbin:restore --scope groupfolders freda

The ``--since`` and ``--until`` options can be used to limit the restore to files deleted inside of the given time period.

This example restores the locally deleted files and files of any groupfolders which are visible to the user
freda. Additionally the files have to be deleted between ``01.08.2023 11:55:22`` and ``02.08.2023 01:33``::

  sudo -E -u www-data php occ trashbin:restore --scope all --since "01.08.2023 11:55:22" --until "02.08.2023 01:33" freda

The ``--dry-run`` option can be used to simulate the restore without actually restoring the files.

.. note::
  You can use the verbose options (``-v`` or ``-vv``) to get more information about
  the restore process and why some files might be skipped.

.. _user_commands_label:

User commands
-------------

The ``user`` commands create and remove users, reset passwords, manage authentication tokens / sessions, display a simple
report showing how many users you have, and when a user was last logged in::

 user
  user:add                            adds a user
  user:add-app-password               adds a app password named "cli" (deprecated: alias for user:auth-tokens:add)
  user:auth-tokens:add                Add app password for the named account
  user:auth-tokens:delete             Deletes an authentication token
  user:auth-tokens:list               List authentication tokens of an user
  user:clear-avatar-cache             clear avatar cache
  user:delete                         deletes the specified user
  user:disable                        disables the specified user
  user:enable                         enables the specified user
  user:info                           shows information about the specific user
  user:keys:verify                    Verify if the stored public key matches the stored private key
  user:lastseen                       shows when the user was logged in last time
  user:list                           shows list of all registered users
  user:report                         shows how many users have access
  user:resetpassword                  Resets the password of the named user
  user:setting                        Read and modify user settings
  user:keys:verify                    Verify that the stored public key matches
                                      the stored private key

user:add
^^^^^^^^

You can create a new user with their display name, login name, and any group
memberships with the ``user:add`` command. The syntax is::

 user:add [--password-from-env] [--generate-password] [--display-name[="..."]] [-g|--group[="..."]] [--email EMAIL]
 uid

The ``display-name`` corresponds to the **Full Name** on the Users page in your
Nextcloud Web UI, and the ``uid`` is their **Username**, which is their
login name. This example adds new user Layla Smith, and adds them to the
**users** and **db-admins** groups. Any groups that do not exist are created::

 sudo -E -u www-data php occ user:add --display-name="Layla Smith"
   --group="users" --group="db-admins" layla
   Enter password:
   Confirm password:
   The user "layla" was created successfully
   Display name set to "Layla Smith"
   User "layla" added to group "users"
   User "layla" added to group "db-admins"

Go to your Users page, and you will see your new user.

``password-from-env`` allows you to set the user's password from an environment
variable. This prevents the password from being exposed to all users via the
process list, and will only be visible in the history of the user (root)
running the command. This also permits creating scripts for adding multiple new
users.

To use ``password-from-env`` you must run as "real" root, rather than ``sudo``,
because ``sudo`` strips environment variables. This example adds new user Fred
Jones::

 export OC_PASS=newpassword
 sudo -E -u www-data php occ user:add --password-from-env --display-name="Fred Jones" --group="users" fred
 The user "fred" was created successfully
 Display name set to "Fred Jones"
 User "fred" added to group "users"

``generate-password`` allows you to set a securely generated password for the user.
This is never shown in the output and can be used to create users with temporary
passwords. This can be used in conjunction with the ``email`` option to create
users with a temporary password and send a welcome email to the user's email
address without user interaction::

 sudo -E -u www-data php occ user:add layla --generate-password --email layla@example.tld
   The account "layla" was created successfully
   Welcome email sent to layla@example.tld

The ``email`` option allows you to set the user's email address when creating
the user. A welcome email will be sent to the user's email address if
``newUser.sendEmail`` is set to ``yes`` in ``core``'s app config or not set at all::

 sudo -E -u www-data php occ user:add layla --email layla@example.tld
   Enter password:
   Confirm password:
   The account "layla" was created successfully
   Welcome email sent to layla@example.tld

user:resetpassword
^^^^^^^^^^^^^^^^^^

You can reset any user's password, including administrators (see
:doc:`../configuration_user/reset_admin_password`)::

 sudo -E -u www-data php occ user:resetpassword layla
   Enter a new password:
   Confirm the new password:
   Successfully reset password for layla

You may also use ``password-from-env`` to reset passwords::

 export OC_PASS=newpassword
 sudo -E -u www-data php occ user:resetpassword --password-from-env layla
   Successfully reset password for layla

user:delete
^^^^^^^^^^^

You can delete users::

 sudo -E -u www-data php occ user:delete fred

user:lastseen
^^^^^^^^^^^^^

View a specific user's most recent login::

 sudo -E -u www-data php occ user:lastseen layla
   layla's last login: 2024-03-20 17:18

View a list of all users' most recent login::

 sudo -E -u www-data php occ user:lastseen --all
   albert's last login: 2024-03-18 10:30
   bob has never logged in.
   layla's last login: 2024-03-20 17:18
   stephanie's last login: 2024-01-11 13:26

user:profile
^^^^^^^^^^^^

Read user profile properties::

  sudo -E -u www-data php occ user:profile admin
    - displayname: admin
    - address: Berlin
    - email: admin@example.net
    - profile_enabled: 1
    - pronouns: they/them

Get a single profile property for a user::

    sudo -E -u www-data php occ user:profile address
      Berlin

Set a profile property::

    sudo -E -u www-data php occ user:profile address Stuttgart

Delete a profile property::

    sudo -E -u www-data php occ user:profile address --delete

user:setting
^^^^^^^^^^^^

Read user settings::

 sudo -E -u www-data php occ user:setting layla
   - core:
     - lang: en
   - login:
     - lastLogin: 1465910968
   - settings:
     - email: layla@example.tld

Filter by app::

 sudo -E -u www-data php occ user:setting layla core
   - core:
     - lang: en

Get a single setting::

 sudo -E -u www-data php occ user:setting layla core lang
 en

Set a setting::

 sudo -E -u www-data php occ user:setting layla settings email "new-layla@example.tld"

Delete a setting::

 sudo -E -u www-data php occ user:setting layla settings email --delete

user:report
^^^^^^^^^^^

Generate a simple report that counts all users, including users on external user
authentication servers such as LDAP::

 sudo -E -u www-data php occ user:report
 +------------------+----+
 | User Report      |    |
 +------------------+----+
 | Database         | 12 |
 | LDAP             | 86 |
 |                  |    |
 | total users      | 98 |
 |                  |    |
 | user directories | 2  |
 | active users     | 15 |
 | disabled users   | 0  |
 +------------------+----+

`active users` shows the number of users which logged in at least once.
`disabled users` shows the number of users which are disabled.

There might be a discrepancy between the total number of users compared to the number of active users and the number of disabled users.
Users that have never logged in before are not counted as active or disabled users.
Some user backends also do not allow a count for the number of users.

user:list
^^^^^^^^^

You can use the command ``user:list`` to list users. By default it will limit the output to 500 users but you can override that with options ``--limit`` and ``--offset``. Use ``--disabled`` to only list disabled users.

user:info
^^^^^^^^^

With the ``user:info`` command, you can access an account information such as: user id, display name, quota, groups, storage usage... and many more

.. code-block::

  user:info admin
    - user_id: admin
    - display_name: admin
    - email: admin@domain.com
    - cloud_id: admin@cloud.domain.com
    - enabled: true
    - groups:
      - admin
      - users
    - quota: none
    - storage:
      - free: 162409623552
      - used: 1110
      - total: 162409624662
      - relative: 0
      - quota: -3
    - first_seen: 2025-03-14T08:44:46+00:00
    - last_seen: 2025-03-25T20:21:13+00:00
    - user_directory: /var/www/nextcloud/data/admin
    - backend: Database

.. _group_commands_label:

Group commands
--------------

The ``group`` commands create and remove groups, add and remove users in
groups, display a list of all users in a group::

 group
  group:add                           add a group
  group:delete                        remove a group
  group:adduser                       add a user to a group
  group:removeuser                    remove a user from a group
  group:list                          list configured groups

You can create a new group with the ``group:add`` command. The syntax is::

 group:add [gid]

The ``gid`` corresponds to the group name you entering after clicking
"Add group" on the Users page in your Nextcloud Web UI. This example adds new
group "beer"::

 sudo -E -u www-data php occ group:add beer

Add an existing user to the specified group with the ``group:adduser``
command. The syntax is::

 group:adduser [gid] [uid]

This example adds the user "denis" to the existing group "beer"::

 sudo -E -u www-data php occ group:adduser beer denis

You can remove user from the group with the ``group:removeuser`` command.
This example removes the existing user "denis" from the existing
group "beer"::

 sudo -E -u www-data php occ group:removeuser beer denis

Remove a group with the ``group:delete`` command. Removing a group doesn't
remove users in a group. You cannot remove the "admin" group. This example
removes the existing group "beer"::

 sudo -E -u www-data php occ group:delete beer

List configured groups via the ``group:list`` command. The syntax is::

 group:list [-l|--limit [LIMIT]] [-o|--offset [OFFSET]] [-i|--info] [--output [OUTPUT]]

``limit`` allows you to specify the number of groups to retrieve (default: ``500``).

``offset`` is an offset for retrieving groups.

``info`` Show additional info (backend).

``output`` Output format: ``plain``, ``json`` or ``json_pretty`` (default: ``plain``).

.. _versions_label:

Versions
--------

.. note::
  This command is only available when the "Versions" app (``files_versions``) is
  enabled.

Use this command to delete file versions for specific users, or for all users
when none are specified::

 versions
  versions:cleanup   Delete versions
  versions:expire    Expires the users file versions

This example deletes all versions for all users::

 sudo -E -u www-data php occ versions:cleanup
 Delete all versions
 Delete versions for users on backend Database
   freda
   molly
   stash
   rosa
   edward

You can delete versions for specific users in a space-delimited list::

 sudo -E -u www-data php occ versions:cleanup freda molly
 Delete versions of   freda
 Delete versions of   molly

.. _command_line_installation_label:

Command line installation
-------------------------

These commands are available only after you have downloaded and unpacked the
Nextcloud archive, and taken no further installation steps.

You can install Nextcloud entirely from the command line. After downloading the
tarball and copying Nextcloud into the appropriate directories you can use ``occ``
commands in place of running the graphical Installation Wizard.

Then choose your ``occ`` options. This lists your available options::

 sudo -E -u www-data php /var/www/nextcloud/occ
 Nextcloud is not installed - only a limited number of commands are available
 Nextcloud version 19.0.0

 Usage:
  [options] command [arguments]

 Options:
  --help (-h)           Display this help message
  --quiet (-q)          Do not output any message
  --verbose (-v|vv|vvv) Increase the verbosity of messages: 1 for normal
  output,  2 for more verbose output and 3 for debug
  --version (-V)        Display this application version
  --ansi                Force ANSI output
  --no-ansi             Disable ANSI output
  --no-interaction (-n) Do not ask any interactive question

 Available commands:
  check                 check dependencies of the server environment
  help                  Displays help for a command
  list                  Lists commands
  status                show some status information
  app
  l10n
  l10n:createjs         Create javascript translation files for a given app
  maintenance
  maintenance:install   install Nextcloud

Display your ``maintenance:install`` options::

 sudo -E -u www-data php occ help maintenance:install
 Nextcloud is not installed - only a limited number of commands are available
 Usage:
  maintenance:install [--database="..."] [--database-name="..."]
 [--database-host="..."] [--database-user="..."] [--database-pass[="..."]]
 [--database-table-prefix[="..."]] [--admin-user="..."] [--admin-pass="..."]
 [--data-dir="..."]

 Options:
  --database               Supported database type (default: "sqlite")
  --database-name          Name of the database
  --database-host          Hostname of the database (default: "localhost")
  --database-user          User name to connect to the database
  --database-pass          Password of the database user
  --admin-user             User name of the admin account (default: "admin")
  --admin-pass             Password of the admin account
  --data-dir               Path to data directory (default:
                           "/var/www/nextcloud/data")
  --help (-h)              Display this help message
  --quiet (-q)             Do not output any message
  --verbose (-v|vv|vvv)    Increase the verbosity of messages: 1 for normal
   output, 2 for more verbose output and 3 for debug
  --version (-V)           Display this application version
  --ansi                   Force ANSI output
  --no-ansi                Disable ANSI output
  --no-interaction (-n)    Do not ask any interactive question

This example completes the installation::

 cd /var/www/nextcloud/
 sudo -E -u www-data php occ maintenance:install --database
 "mysql" --database-name "nextcloud"  --database-user "root" --database-pass
 "password" --admin-user "admin" --admin-pass "password"
 Nextcloud is not installed - only a limited number of commands are available
 Nextcloud was successfully installed

Supported databases are::

 - sqlite (SQLite3 - Nextcloud Community edition only)
 - mysql (MySQL/MariaDB)
 - pgsql (PostgreSQL)
 - oci (Oracle - Nextcloud Enterprise edition only)

.. _command_line_upgrade_label:

Command line upgrade
--------------------

These commands are available only after you have downloaded upgraded packages or
tar archives, and before you complete the upgrade.

List all options::

 sudo -E -u www-data php occ upgrade -h
 Usage:
 upgrade [--quiet]

 Options:
 --help (-h)            Display this help message.
 --quiet (-q)           Do not output any message.
 --verbose (-v|vv|vvv)  Increase the verbosity of messages: 1 for normal output,
   2 for more verbose output and 3 for debug.
 --version (-V)         Display this application version.
 --ansi                 Force ANSI output.
 --no-ansi              Disable ANSI output.
 --no-interaction (-n)  Do not ask any interactive question

When you are performing an update or upgrade on your Nextcloud server (see the
Maintenance section of this manual), it is better to use ``occ`` to perform the
database upgrade step, rather than the Web GUI, in order to avoid timeouts. PHP
scripts invoked from the Web interface are limited to 3600 seconds. In larger
environments this may not be enough, leaving the system in an inconsistent
state. After performing all the preliminary steps (see
:doc:`../maintenance/upgrade`) use this command to upgrade your databases,
like this example on CentOS Linux. Note how it details the steps::

 sudo -E -u www-data php occ upgrade
 Nextcloud or one of the apps require upgrade - only a limited number of
 commands are available
 Turned on maintenance mode
 Checked database schema update
 Checked database schema update for apps
 Updated database
 Updating <gallery> ...
 Updated <gallery> to 0.6.1
 Updating <activity> ...
 Updated <activity> to 2.1.0
 Update successful
 Turned off maintenance mode

Enabling verbosity displays timestamps::

 sudo -E -u www-data php occ upgrade -v
 Nextcloud or one of the apps require upgrade - only a limited number of commands are available
 2015-06-23T09:06:15+0000 Turned on maintenance mode
 2015-06-23T09:06:15+0000 Checked database schema update
 2015-06-23T09:06:15+0000 Checked database schema update for apps
 2015-06-23T09:06:15+0000 Updated database
 2015-06-23T09:06:15+0000 Updated <files_sharing> to 0.6.6
 2015-06-23T09:06:15+0000 Update successful
 2015-06-23T09:06:15+0000 Turned off maintenance mode

If there is an error it throws an exception, and the error is detailed in your
Nextcloud logfile, so you can use the log output to figure out what went wrong,
or to use in a bug report::

 Turned on maintenance mode
 Checked database schema update
 Checked database schema update for apps
 Updated database
 Updating <files_sharing> ...
 Exception
 ServerNotAvailableException: LDAP server is not available
 Update failed
 Turned off maintenance mode

.. _two_factor_auth_label:

Two-factor authentication
-------------------------
If a two-factor provider app is enabled, it is enabled for all users by default
(though the provider can decide whether or not the user has to pass the challenge).
In the case of a user losing access to the second factor (e.g. lost phone with
two-factor SMS verification), the admin can try to disable the two-factor
check for that user via the occ command::

 sudo -E -u www-data php occ twofactorauth:disable <uid> <provider_id>

.. note:: This is not supported by all providers.

To re-enable two-factor auth again use the following command::

 sudo -E -u www-data php occ twofactorauth:enable <uid> <provider_id>

.. note:: This is not supported by all providers.

.. _disable_user_label:

Disable users
-------------
Admins can disable users via the occ command too::

 sudo -E -u www-data php occ user:disable <username>

Use the following command to enable the user again::

 sudo -E -u www-data php occ user:enable <username>

Note that once users are disabled, their connected browsers will be disconnected.


.. _system_tags_commands_label:

System Tags
-----------

List tags::

  sudo -E -u www-data php occ tag:list

Add a tag::

  sudo -E -u www-data php occ tag:add <name> <access>

Edit a tag::

  sudo -E -u www-data php occ tag:edit --name <name> --access <access> <id>

`--name` and `--access` are optional.

Delete a tag::

  sudo -E -u www-data php occ tag:delete <id>

Access level

========== ======== ==========
Level      Visible¹ Assignable²
========== ======== ==========
public     Yes      Yes
restricted Yes      No
invisible  No       No
========== ======== ==========

| ¹ User can see the tag
| ² User can assign the tag to a file

.. _antivirus_commands_label:

Antivirus
---------

Get info about files in the scan queue::

  sudo -E -u www-data php occ files_antivirus:status [-v]

Manually trigger the background scan::

  sudo -E -u www-data php occ files_antivirus:background-scan [-v] [-m MAX]

Manually scan a single file::

  sudo -E -u www-data php occ files_antivirus:scan <path>

Mark a file as scanned or unscanned::

  sudo -E -u www-data php occ files_antivirus:mark <path> <scanned|unscanned>

.. _setupchecks_commands_label:

Setupchecks
-----------

Run the setupchecks via occ::

  sudo -E -u www-data php occ setupchecks

Example output::

  dav:
    ✓ DAV system address book: No outstanding DAV system address book sync.
  network:
    ✓ WebDAV endpoint: Your web server is properly set up to allow file synchronization over WebDAV.
    ✓ Data directory protected
    ✓ Internet connectivity
    ...

.. _share_operations_label:

Share operations
----------------

Available ``occ`` commands for the ``share`` namespace::

  share:list                       list shares on the system

.. _occ_share_list_label:

List
^^^^

The ``share:list`` command lists all shares created on the system, with optional filters for recipient,
sharee, shared file and more.

.. _occ_debugging:

Debugging
---------

In certain situations it's necessary to generate debugging information, e.g. before submitting a bug report. You can run ``occ`` with debug logging::

 NC_loglevel=0 sudo -E -u www-data php occ -h
