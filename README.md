ansible_generic_mount
======================

This is a ansible role to perform Actifio mounts to a host.
Please find another roles (ansible_appaware_mount, ansible_newvm_mount) if you need these operation.

Role Variables
--------------

Following variables are accepted/required for this role. 

### Actifio Applinace Related 

| Variable Name    | Description | Required (Y/N) |
|------------------|---|---|
| act_appliance    | Actifio Appliance IP or FQDN. | Y               |
| act_user         | Actifio username. This should be a Actifio user with System Manage priviledges | Y
| act_pass         | Password for the Actifio User | Y
| act_vendorkey    | Vendor key can be obtained by the customer through opening a Support Case with the CSE. | Y
| act_appname 	   | Source Application (VM) name | Y
| act_restoretime  | Desired time to recover the database to. Based on the time specified, the appropriate image will be selected (if an image is not specified). If a recovery image is not availble for the stipulated restore time, and if the strict_policy is set to no, then the closest image to the restore time will be selected. | N
| strict_policy    | See act_restoretime | N
| act_job_class    | snapshot, dedup, dedupasync, liveclone, syncback and OnVault. If not specified would select any based on the Restore time, without any preference to the jobclass. | N
| act_imagename    | Actifio Image Name to be mounted. This parameter overrides act_appname, act_restoretime and act_job_class. | N 
| act_nowait_mount  | If set to true waits for the mount job to complete. Else return after submitting the job. | N
| act_imagelabel   | Label for mounted image. Default is 'Ansible_Playbook'. | N
| src_host 	   | Source host where the application is protected from. | Y
| src_host_isvm    | Specify 'true' if the source host is VM and there is a physical host registration with the same name. | N
| tgt_host 	   | Target host where the application image to be mounted. | Y
| mount_point      | Mount point path name or drive letter. (Only perform with target host with Actifio connector) | N

Example Playbook
----------------

### Mount a windows drive backup image to another host.

```
- name: testng mount image to a host
  hosts: "{{ host_group }}"
  become: yes
  become_method: sudo
  roles:
    - { role: ansible_generic_mount, act_appliance: my-actifio, act_user: ansible, act_pass: mypassword }
  vars:
    act_vendorkey: "{{ contact CSE to get yours }}"
    act_appname: "E:\\"
    act_job_class: "snapshot"
    src_host: "win2012tmp1"
    tgt_host: "win2012tmp2"
    mount_point: "X:\\"

```


License
-------

Copyright 2018 <Hiroshi Takeuchi hiroshi.takeuchi@actifio.com>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
