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

Create the logging container(s)

.. code-block:: bash

    cd /opt/openstack-ansible/playbooks
    openstack-ansible lxc-containers-create.yml -e container_group=logging_containers

install master/data elasticsearch nodes on the elastic-logstash containers

.. code-block:: bash

    cd /opt/openstack-ansible-ops
    openstack-ansible installElastic.yml -e elk_hosts=elastic-logstash -e node_master=true -e node_data=true

Install an Elasticsearch client on the kibana container to serve as a loadbalancer for the Kibana backend server

.. code-block:: bash

    openstack-ansible installElastic.yml -e elk_hosts=kibana -e node_master=false -e node_data=false

Install Logstash on all the elastic containers

.. code-block:: bash

    openstack-ansible installLogstash.yml

InstallKibana on the kibana container

.. code-block:: bash

    openstack-ansible installKibana.yml

(Optional) Reverse proxy kibana container to your loadbalancer host

.. code-block:: bash

    openstack-ansible reverseProxyKibana.yml

load topbeat indices into elastic-search and kibana

.. code-block:: bash

    openstack-ansible loadKibana.yml

install Topbeat everywhere to start shipping metrics to our logstash instances

.. code-block:: bash

    openstack-ansible installTopbeat.yml --forks 100
