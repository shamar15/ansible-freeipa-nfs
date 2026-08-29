FreeIPA Ansible collection
==========================

This repository contains Ansible roles and playbooks to install and uninstall FreeIPA servers, replicas and clients, also management modules.

Important
---------

For the documentation of this collection, please have a look at the documentation in the collection archive. Starting point: Base collection directory, file **`README-COLLECTION.md`**.

GALAXY is not providing proper user documentation nor is able to render the documentation that is part of the collection. Therefore original `README.md` had to be renamed to `README-COLLECTION.md` to ensure that GALAXY is not trying to render it.

Please ignore any modules and plugins in the GALAXY documentation section with the prefix `ipaserver_`, `ipareplica_`, `ipaclient_`, `ipabackup_` and `ipasmartcard_` and also `module_utils` and `doc_fragments`. These files are used internally only and are not supported to be used otherwise.

There is also the [generic ansible-freeipa 1.17.0 upstream documentation](https://github.com/freeipa/ansible-freeipa/blob/v1.17.0/README.md) and also the [latest generic ansible-freeipa upstream documentation](https://github.com/freeipa/ansible-freeipa/blob/master/README.md), both without using the collection prefix `freeipa.ansible_freeipa`.
