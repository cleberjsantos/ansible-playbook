Requirements
------------

Target server
^^^^^^^^^^^^^

Supported platforms
```````````````````

At the moment, we're only supporting Debian/Ubuntu environments for the target server. This is simply because the expertise of the initial authors is with the .deb world. Adding RPM environments should not be difficult, but we need help. Your pull requests are welcome.

At the moment, we are testing with Ubuntu 12 (Precise) LTS and 14 (Trusty) LTS and with Debian Squeeze and wheezy.

SSH access; sudo
````````````````

Beyond the basic platform, the only requirements are that you have ``ssh`` access to the remote server with full ``sudo`` rights.

For local testing via virtual machine, any machine that support VirtualBox/Vagrant should be adequate.

Local setup
^^^^^^^^^^^

Python 2.#, virtualenv, git

Optional
^^^^^^^^

github account for easy branching and customizaiton

Preparing your playbook
-----------------------

Installing Ansible
^^^^^^^^^^^^^^^^^^

    virtualenv

Setting up the Playbook
^^^^^^^^^^^^^^^^^^^^^^^

Clone or branch-and-clone
`````````````````````````

Take a few moments to think about how you're going to customize the Plone Playbook. Are you likely to make substantial changes? Or simply change the option settings?

If you expect to make substantial changes, you'll want to create your own git branch of the Plone Playbook. Then, clone your branch. That way you'll be able to push changes back to your branch. We assume that you either know how to use git, or will learn, so we won't try to document this usage.

If you expect to change only option settings, then just clone the Plone Playbook to your local computer (not the target server)::

    git clone https://github.com/plone/ansible-playbook.git

Picking up required roles
`````````````````````````

*Roles* are packages of Ansible settings and tasks. The Plone Playbook has separate roles for each of the major components it works with. These roles are not included with the playbook itself, but they are easy to install.

To install the required roles, issue the command ``ansible-galaxy -p roles -r requirements.txt install`` from the playbook directory. This will create a roles subdirectory and fill it with the required roles.

If you want to store your roles elsewhere, edit the ``ansible.cfg`` file in the playbook directory.

Customizing the deployment
^^^^^^^^^^^^^^^^^^^^^^^^^^

There are two major strategies for customization.

**If you are working on your own branch**, it's yours. You may edit ``configure.yml`` to set options.

**If you cloned or downloaded the master distribution**, you will probably want to avoid changing the files from the distribution. That would make it hard to update. Instead, create a new file ``local-configure.yml`` and put your custom option specifications in it. This file will not be overriden when you pull an update from the master.

Using the local configuration strategy, copy from ``configure.yml`` only the options you wish to change to ``local-configure.yml``. Edit them there.

Customizing buildout configuration
``````````````````````````````````

Plone is typically installed using `buildout <http://www.buildout.org/en/latest/>`_ to manage Python dependencies. Plone's Ansible Playbook uses operating-system package managers to manage system-level dependencies and uses buildout to manage Python-package dependencies.

Buildout cofiguration files are nearly always customized to meet the need of the particular Plone installation. At a minimum, the buildout configuration details Plone add ons for the install. It is nearly always additionally customized to meet performance and integration requirements.

You have two available mechanisms for doing this customization in conjunction with Ansible:

* You may rely on the buildout skeleton supplied by this playbook. It will allow you to set values for commonly changed options like the egg (Python package) list, ports and cluster client count.

* You may supply a git repository specification, including branch or tag, for a buildout directory skeleton. The Plone Ansible Playbook will clone this or pull updates as necessary.

If you choose the git repository strategy, your buildout skeleton must, at a minimum, include ``bootstrap.py`` and ``buildout.cfg`` files. It will also commonly contain a ``src/`` subdirectory and extra configuration files. It will probably **not** contain ``bin/``, ``var/`` or ``parts/`` directories. Those will typically be excluded in your ``.gitignore`` file.

If you use a buildout directory checkout, you must still specify in your Playbook variables the names and listening port numbers of any client parts you wish included in the load balancer configuration. Also specify the name of your ZEO server part if it is not ``zeoserver``.

The Configuration File
^^^^^^^^^^^^^^^^^^^^^^

YAML








