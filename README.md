TripleO os-collect-config
=========
This role is used for configuring os-collect-config on deployed servers to integrate them into a TripleO deployment using the "Deployed Server" functionality.
This is documented at http://tripleo.org/install/advanced_deployment/deployed_server.html

Requirements
------------

This role requires a working TripleO undercloud, and requires the overcloud nodes to be configured have a repository with python-heat-agent and os-collect-config available

Role Variables
--------------

```undercloud_command_host``` This is the host that you wish to run the commands to obtain the information from the heat stack. It defaults to the first host in the group "undercloud".
If you have the appropriate python-openstack client tools on your local machine, this could be set to localhost.
```undercloud_creds_command``` This is the command that is run before all commands to interrogate the heat stack, in order to authenticate to the undercloud sucessfully. It defaults to "source /home/stack/stackrc"
which is typical when running on an undercloud host. If you have os-cloud-config configured correctly, this could be set to 'export OS_CLOUD=undercloud' or something similar
```tripleo_server_node_index``` This is a variable which needs to be declared on each host and matches that server to the index in the TripleO deployment. This number should be unique across each group of servers with
the same tripleo_server_role
```tripleo_server_role``` This is a variable which needs to be decalred on each host and matches that server to it's role in the TripleO roles file.

Example Playbook
----------------

The following is an example of an ansible inventory (inventory) and a playbook against that inventory to set things up. Note that the playbook will block and keep looping until a tripleo stack reports the correct
data needed for the playbook to work. So you can either kick off a TripleO deploy before running this playbook, or while it's running and awaiting for the data to be available.

    inventory
    ---
    [undercloud]
    undercloud.example.com ansible_become=true ansible_become_user=stack

    [overcloud]
    controller-0 tripleo_server_node_index=0 tripleo_server_role=ControllerDeployedServer
    controller-1 tripleo_server_node_index=1 tripleo_server_role=ControllerDeployedServer
    controller-2 tripleo_server_node_index=2 tripleo_server_role=ControllerDeployedServer
    compute-0 tripleo_server_node_index=0 tripleo_server_role=ComputeDeployedServer
    compute-1 tripleo_server_node_index=1 tripleo_server_role=ComputeDeployedServer
    compute-2 tripleo_server_node_index=2 tripleo_server_role=ComputeDeployedServer

    playbook.yaml
    ---
    - hosts: overcloud
      roles:
        - ansible-role-tripleo-os-collect-config

License
-------

BSD

Author Information
------------------

Graeme Gillies
graeme.r.gillies@gmail.com
https://github.com/ggillies
