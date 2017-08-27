[![Build Status](https://travis-ci.org/zaxos/tomcat-ansible-role.svg?branch=master)](https://travis-ci.org/zaxos/tomcat-ansible-role)
[![Ansible Galaxy](https://img.shields.io/badge/galaxy-_zaxos.tomcat--ansible--role-blue.svg)](https://galaxy.ansible.com/zaxos/tomcat-ansible-role/)

tomcat-ansible-role
===================

Ansible role to install and configure Apache Tomcat on CentOS/RHEL. 


Requirements
------------
* Tomcat supported versions by this role:
  * 7.0
  * 8.0
  * 8.5
* CentOS/RHEL 7
* SELinux disabled

Installation
------------
```
$ ansible-galaxy install zaxos.tomcat-ansible-role
```

Example Playbook
----------------
```yaml
- hosts: servers
  vars:
    tomcat_version: 8.5.20
    
    tomcat_users:
      - username: "tomcat"
        password: "t3mpp@ssw0rd"
        roles: "tomcat,admin,manager,manager-gui"
      - username: "exampleuser"
        password: "us3rp@ssw0rd"
        roles: "tomcat"        
  roles:
    - role: zaxos.tomcat-ansible-role
```

Role Variables
--------------
The main variable:
- `tomcat_version`: tomcat version to install

Some variables that require review:
- `tomcat_install_java`: True   
By default OpenJDK Java will be installed. Change it to "False" if you don't want OpenJDK Java to be installed by this role.
- `tomcat_java_version`: 1.8   
OpenJDK Java version to be installed. Default is "1.8".
- `tomcat_install_path`: /opt   
Location in which tomcat will be installed. Default is "/opt".
- JVM memory management:   
You can set the minimum and maximum memory heap size with the following JVM -Xms and -Xmx variables as a percentage of the total system memory. For example, for a 2GB RAM system, using the default values: Xms=307m (15% of 2048MB), Xmx=1126m (55% of 2048MB).
  * `tomcat_jvm_memory_percentage_xms`: 15
  * `tomcat_jvm_memory_percentage_xmx`: 55   
- `tomcat_allow_manager_access_only_from_localhost`: False   
If set to "True", tomcat manager app will be accessible only from localhost for security reasons. This behaviour is default only in Tomcat 8.5.
- `tomcat_allow_host_manager_access_only_from_localhost`: False   
If set to "True", tomcat host manager app will be accessible only from localhost for security reasons. This behaviour is default only in Tomcat 8.5.
- `tomcat_users`: List of tomcat users to be created. See example for the expected format.
- `tomcat_debug_mode`: False   
Change it to "True" in order to configure tomcat to allow remote debugging. Default debug port is set to tcp/8000 (you can change it through the corresponding variable).

Tomcat ports:
- `tomcat_port_connector`: 8080
- `tomcat_port_shutdown`: 8005
- `tomcat_port_redirect`: 8443
- `tomcat_port_ajp`: 8009
- `tomcat_port_debug`: 8000

Some defaults (probably not requiring tampering):
- `tomcat_service_name`: tomcat
- `tomcat_service_enabled_on_startup`: True
- `tomcat_java_home`: /usr/lib/jvm/jre
- `tomcat_downloadURL`: https://<i></i>archive.apache.org/dist
- `tomcat_user`: tomcat
- `tomcat_group`: tomcat
- `tomcat_temp_download_path`: /tmp/ansibletomcattempdir

Optional variables (by default undefined):
- You can set custom user uid and group gid for homogeneity across multiple servers. For example:
  * `tomcat_user_uid`: 500
  * `tomcat_group_gid`: 500

In case of uninstallation:
- To uninstall tomcat that was installed using this role, set the following variable to "absent". Default value is "present".
  * `tomcat_state`: absent
- `tomcat_uninstall_create_backup`: True   
By default, in a better safe than sorry basis, a backup tar archive will be created at "tomcat_install_path" before deletion.
- `tomcat_uninstall_remove_java`: False   
Change it to "True" to uninstall Java after tomcat is uninstalled.
- By default, tomcat user and group will be removed . Change to "False" to preserve them after tomcat is uninstalled.
  * `tomcat_uninstall_remove_user`: True
  * `tomcat_uninstall_remove_group`: True
- `tomcat_uninstall_remove_all`: False   
In order to override the above values and uninstall everything, set it to "True".
