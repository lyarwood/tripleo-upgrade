tripleo-upgrade
=========

This role aims to provide a unified tool for upgrading TripleO based deploments.

Requirements
------------

This role requires:

* An ansible inventory file containing reacheable undercloud and overcloud nodes

* Nodes in the inventory file are placed in groups based on their roles (e.g compute nodes are part of the 'compute' group)

* Repositories containing packages to be upgraded are already installed on undercloud and overcloud nodes

* The initial overcloud deploy command is placed in a script file located in the path set by the overcloud_deploy_script var. Each option/environment file should be placed on a separate new line, e.g:

    source ~/stackrc
    export THT=/usr/share/openstack-tripleo-heat-templates/

    openstack overcloud deploy --templates $THT \
    -r ~/openstack_deployment/roles/roles_data.yaml \
    -e $THT/environments/network-isolation.yaml \
    -e $THT/environments/network-management.yaml \
    -e $THT/environments/storage-environment.yaml \
    -e ~/openstack_deployment/environments/nodes.yaml \
    -e ~/openstack_deployment/environments/network-environment.yaml \
    -e ~/openstack_deployment/environments/disk-layout.yaml \
    -e ~/openstack_deployment/environments/neutron-settings.yaml \
    --log-file overcloud_deployment.log &> overcloud_install.log

Role Variables
--------------

Available variables are listed below:

    overcloud_deploy_script: "~/overcloud_deploy.sh"

Location of the initial overcloud deploy script.

    undercloud_upgrade_script: "~/undercloud_upgrade.sh"

Location of the undercloud upgrade script which is going to be generated by this role.

    overcloud_composable_upgrade_script: "~/composable_docker_upgrade.sh"

Location of the upgrade script used in the composable docker upgrade step which is going to be generated by this role.

    overcloud_converge_upgrade_script: "~/converge_docker_upgrade.sh"

Location of the upgrade script used in the converge docker upgrade step which is going to be generated by this role.

    undercloud_rc: "~/stackrc"

Location of the undercloud credentials file.

    overcloud_rc: "~/overcloudrc"

Location of the overcloud credentials file.

Dependencies
------------

None.

Example Playbook
----------------

An example playbook is provided in tests/test.yml:

    - hosts: all
      gather_facts: true

    - hosts: undercloud
      gather_facts: false
      become: yes
      become_method: sudo
      become_user: stack
      roles:
        - tripleo-upgrade

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
