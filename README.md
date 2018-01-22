Consul Ansible Role
===================

An ansible role for installing Docker

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------



Dependencies
------------

- mjcramer.system

Tags
----
- apply
- configure
- initialize
- check

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: mjcramer.docker, x: 42 }

License
-------

Unlicensed

Author Information
------------------

Michael J. Cramer (github: mjcramer), michael@cramer.name
