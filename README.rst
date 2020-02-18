.. _section-role-honzamach-warden-fail2ban:

Role **honzamach.warden_fail2ban**
================================================================================

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/warden_fail2ban>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-warden-fail2ban>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-warden-fail2ban>`__

Main purpose of this role is to ...

**Table of Contents:**

* :ref:`section-role-honzamach-warden-fail2ban-installation`
* :ref:`section-role-honzamach-warden-fail2ban-dependencies`
* :ref:`section-role-honzamach-warden-fail2ban-usage`
* :ref:`section-role-honzamach-warden-fail2ban-variables`
* :ref:`section-role-honzamach-warden-fail2ban-files`
* :ref:`section-role-honzamach-warden-fail2ban-author`

This role is part of the `MSMS <https://github.com/honzamach/ansible-role-commonenv>`__
package. Some common features are documented in its :ref:`manual <section-manual>`.


.. _section-role-honzamach-warden-fail2ban-installation:

Installation
--------------------------------------------------------------------------------

To install the role `honzamach.warden_fail2ban <https://galaxy.ansible.com/honzamach/warden_fail2ban>`__
from `Ansible Galaxy <https://galaxy.ansible.com/>`__ please use variation of
following command::

    ansible-galaxy install honzamach.warden_fail2ban

To install the role directly from `GitHub <https://github.com>`__ by cloning the
`ansible-role-warden-fail2ban <https://github.com/honzamach/ansible-role-warden-fail2ban>`__
repository please use variation of following command::

    git clone https://github.com/honzamach/ansible-role-warden-fail2ban.git honzamach.warden_fail2ban

Currently the advantage of using direct Git cloning is the ability to easily update
the role when new version comes out.


.. _section-role-honzamach-warden-fail2ban-dependencies:

Dependencies
--------------------------------------------------------------------------------

This role is not dependent on any other role.

This role is dependent on following roles:

* :ref:`role1 <section-role-role1>`
* :ref:`role2 <section-role-role2>` *[SOFT]*

No roles have dependency on this role.

Following roles have dependency on this role:

* :ref:`role3 <section-role-role3>`
* :ref:`role4 <section-role-role4>` *[SOFT]*


.. _section-role-honzamach-warden-fail2ban-usage:

Usage
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers_warden_fail2ban]
    your-server

Example content of role playbook file ``role_playbook.yml``::

    - hosts: servers_warden_fail2ban
      remote_user: root
      roles:
        - role: honzamach.warden_fail2ban
      tags:
        - role-warden-fail2ban

Example usage::

    # Run everything:
    ansible-playbook --ask-vault-pass --inventory inventory role_playbook.yml

It is recommended to follow these configuration principles:

* Create/edit file ``inventory/group_vars/all/vars.yml`` and within define some sensible
  defaults for all your managed servers.

* Use files ``inventory/host_vars/[your-server]/vars.yml`` to customize settings
  for particular servers. Please see section :ref:`section-role-warden-fail2ban-variables`
  for all available options. Example::

        # Enable test mode to prevent from spamming Warden server before you are ready.
        hm_warden_f2b__test_mode: true

        # Define unique name for your detector node.
        hm_warden_f2b__node_name: cz.cesnet.fail2ban.blacklist

        # Use log stream for central log server.
        hm_warden_f2b__log_stream: /var/log/net-all.log

        # Report to different email.
        hm_warden_f2b__dest_email: masters@cesnet.cz

        # Never ban these IP addresses.
        hm_warden_f2b__ignore_ip: "127.0.0.1 195.113.161.46"


.. _section-role-honzamach-warden-fail2ban-variables:

Configuration variables
--------------------------------------------------------------------------------


Internal role variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. envvar:: hm_warden_f2b__install_packages

    List of packages related to this role that will be installed on target hosts.

    * *Datatype:* ``dict``
    * *Default:* (please see YAML file ``defaults/main.yml``)
    * *Example:*

    .. code-block:: yaml

        hm_warden_f2b__install_packages:
          debian:
            apt:
              - fail2ban

.. envvar:: hm_warden_f2b__service_name

    Name of the system service.

    * *Datatype:* ``string``
    * *Default:* ``"fail2ban"``

.. envvar:: hm_warden_f2b__test_mode

    Install in test mode. Events will be generated, logged and then discarded.

    * *Datatype:* ``bool``
    * *Default:* ``"false"``

.. envvar:: hm_warden_f2b__node_name

    Node name for the detector. This value will be filled into the generated event.

    * *Datatype:* ``string``
    * *Default:* ``"org.domain.fail2ban.blacklist"``

.. envvar:: hm_warden_f2b__log_stream

    Log file (stream) to watch and process.

    * *Datatype:* ``string``
    * *Default:* ``"/var/log/syslog"``

.. envvar:: hm_warden_f2b__dest_email

    Default email address for email based notification actions.

    * *Datatype:* ``string``
    * *Default:* ``"info@domain.org"``

.. envvar:: hm_warden_f2b__ignore_ip

    Space separated list of ignored IP addresses/CIDRs that should never be banned.
    * *Datatype:* ``string``
    * *Default:* ``"127.0.0.1"``

.. envvar:: hm_warden_f2b__log_level_f2b

    Fail2Ban logging level.

    * *Datatype:* ``string``
    * *Default:* ``"INFO"``

.. envvar:: hm_warden_f2b__logdir

    Directory for all log files.

    * *Datatype:* ``string``
    * *Default:* ``"/var/log/fail2ban"``

.. envvar:: hm_warden_f2b__log_file_f2b

    Log file for Fail2Ban itself.

    * *Datatype:* ``string``
    * *Default:* ``"{{ hm_warden_f2b__logdir }}/fail2ban.log"``

.. envvar:: hm_warden_f2b__log_file_action

    Log file for custom ``cust-log.conf`` action.

    * *Datatype:* ``string``
    * *Default:* ``{{ hm_warden_f2b__logdir }}/warden-f2b.log``

.. envvar:: hm_warden_f2b__log_file_events

    Log file for custom ``warden-f2b-*.sh`` action scripts for sending events to Warden.

    * *Datatype:* ``string``
    * *Default:* ``"{{ hm_warden_f2b__logdir }}/warden-f2b-events.log"``


Built-in Ansible variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

:envvar:`ansible_lsb['codename']`

    Linux distribution codename. It is used to generate correct APT repository URL
    and for :ref:`template customizations <section-overview-role-customize-templates>`.


.. _section-role-honzamach-warden-fail2ban-files:

Managed files
--------------------------------------------------------------------------------

.. note::

    This role supports the :ref:`template customization <section-overview-role-customize-templates>` feature.

This role manages content of following files on target system:

* ``/etc/default/warden-f2b`` *[TEMPLATE]*
* ``/etc/logrotate.d/fail2ban`` *[TEMPLATE]*
* ``/usr/local/bin/warden-f2b-found-listed.sh`` *[TEMPLATE]*
* ``/usr/local/bin/warden-f2b-spammer.sh`` *[TEMPLATE]*
* ``/usr/local/bin/warden-f2b-unknown-email.sh`` *[TEMPLATE]*
* ``/etc/fail2ban/fail2ban.local`` *[TEMPLATE]*
* ``/etc/fail2ban/jail.local`` *[TEMPLATE]*
* ``/etc/fail2ban/action.d/cust-blacklist-found-listed.conf`` *[COPY]*
* ``/etc/fail2ban/action.d/cust-blacklist-spammer.conf`` *[COPY]*
* ``/etc/fail2ban/action.d/cust-blacklist-unknown-email.conf`` *[COPY]*
* ``/etc/fail2ban/action.d/cust-log.conf`` *[COPY]*
* ``/etc/fail2ban/action.d/cust-warden-found-listed.conf`` *[COPY]*
* ``/etc/fail2ban/action.d/cust-warden-spammer.conf`` *[COPY]*
* ``/etc/fail2ban/action.d/cust-warden-unknown-email.conf`` *[COPY]*
* ``/etc/fail2ban/filter.d/cust-postfix-blocked.conf`` *[COPY]*
* ``/etc/fail2ban/filter.d/cust-postfix-spam.conf`` *[COPY]*
* ``/etc/fail2ban/filter.d/cust-postfix-unknown-email.conf`` *[COPY]*


.. _section-role-honzamach-warden-fail2ban-author:

Author and license
--------------------------------------------------------------------------------

| *Copyright:* (C) since 2020 Jan Mach <jan.mach@cesnet.cz>
| *Author:* Jan Mach <jan.mach@cesnet.cz>
| Use of this role is governed by the MIT license, see LICENSE file.
|
