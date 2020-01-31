Description
--------------

Role to export selected trace files in the reverse proxy.
This obviously requires you to have had enabled tracing at a certain moment in time.
The role web/set_tracing does this for you, and uses the same variables/variable structure.

Example
--------------

- hosts: "all"
  connection: local
  gather_facts: no
  tasks:
    - name: Download reverse proxy trace log
      tags: ["trace"]
      import_role:
          name: web/export_tracingfile
      vars:
          logs_export_dir: "{{ export_dir | default(inventory_dir) }}"
          logs_become: false
          logs_group: "{{ ansible_group_for_administrators | default(omit) }}"
          logs_mode: '0770'
          tracing_instances:   
            - inst_name: "{{ 'medewerkerwrp0' + appliance_number }}"
              tracing:
               - component_id: "pdweb.debug" 
               - component_id: "pdweb.wns.session"