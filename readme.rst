OpenNext Private Logging Stack
##############################
:date: 2018-01-13
:tags: openstack, ansible, opennext
:category: \*openstack, \*nix, \*logging


About this repository
---------------------

This set of playbooks will deploy Elasticsearch, Fluentd and Kibana to collect,
analyse and visualise the all the logs of the OpenStack infrastructure.

Process
-------


Clone the ONPC Logging repo

.. code-block:: bash

    cd /opt
    git clone https://github.com/opennext-io/onpc-logging.git
    cd /opt/openstack-ansible/playbooks

Create the logging container(s)

.. code-block:: bash

    openstack-ansible lxc-containers-create.yml -e 'container_group=elastic-fluentd_containers:kibana_containers'

Install master/data Elasticsearch servers on the elasticsearch containers

.. code-block:: bash

    openstack-ansible playbook_elasticsearch.yml -e 'elastic_hosts=elasticsearch -e node_master=true -e node_data=true'

Install an Elasticsearch client on the kibana container to serve as a loadbalancer for the Kibana backend server

.. code-block:: bash

    openstack-ansible playbook_elasticsearch.yml -e 'elastic_hosts=kibana -e node_master=false -e node_data=false'

Install Fluentd (td-agent) on the fluentd containers
   
.. code-block:: bash

    openstack-ansible playbook_fluentd.yml

Install Kibana on Kibana containers

.. code-block:: bash

    openstack-ansible playbook_kibana.yml
