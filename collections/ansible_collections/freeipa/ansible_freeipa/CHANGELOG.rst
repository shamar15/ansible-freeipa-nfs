Changes for 1.17.0 since 1.16.0
-------------------------------

  - ipaclient_test: Drop extra ca_cert_files list test (#1430)
  - Ansible test sanity fixes (#1429)
  - ipauser: Fix query state error handling and per-field conversion errors (#1428)
  - ipagroup: Use PARAM_MAPPING and query state support (#1427)
  - Ipauser query (#1422)
  - Fix SSH public key tests without key type name (#1418)
  - build-collection.sh: Rename base README.md to make it invisible for AAH (#1411)
  - infra/image/shcontainer: New container_save and container_load (#1410)
  - Reworked and renamed script to generate Ansible collections (#1409)

Detailed changelog for 1.17.0 since 1.16.0 by author
----------------------------------------------------
  1 authors, 13 commits

Thomas Woerner (13)

  - ipaclient_test: Drop extra ca_cert_files list test
  - Fix latest ansible-test sanity findings
  - ipagroup: Use PARAM_MAPPING and query state support
  - ipauser: Fix query state error handling and per-field conversion errors
  - test_group_case_insensitive.yml: Fix wrong user absent task
  - ipauser: Use PARAM_MAPPING and query state support
  - ansible_freeipa_module.py: New PARAM_MAPPING and query support
  - setup.cfg: Set max-public-methods to 25
  - Update pre-commit repo versions, deactivate parseable for ansible-lint
  - Fix SSH public key tests without key type name
  - build-collection.sh: Rename base README.md to make it invisible for AAH
  - infra/image/shcontainer: New container_save and container_load
  - Reworked and renamed script to generate Ansible collections

