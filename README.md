# Dynatrace-Server-Ansible

An [Ansible](http://www.ansible.com) role for automated deployments of the [Dynatrace](http://bit.ly/dttrial) Server.

**Note**: Currently, we support only Linux hosts, support for installing Windows hosts is in the making.

## Download

The role is available via:

- [Ansible Galaxy](https://galaxy.ansible.com/list#/roles/)
- [GitHub](https://github.com/Dynatrace/Dynatrace-Server-Ansible)

## Requirements

Download the Dynatrace (full) installer from [downloads.compuwareapm.com](http://downloads.compuwareapm.com) and place the artifact as ```dynatrace.jar``` in the role's ```files``` directory from where it will be picked up during the automated installation.

Optionally, you can place a license key file at ```files/dtlicense.key``` and a fix pack installer at ```files/dynatrace.dtf```.

## Role Variables

As defined in ```defaults/main.yml```:

| Name                                         | Default               | Description |
|----------------------------------------------|-----------------------|-------------|
| *dynatrace_server_linux_install_dir*         | /opt                  | The Dynatrace Server will be installed into the directory *$dynatrace_agents_linux_install_dir*/dynatrace-*$major*-*$minor*-*$rev*, where *$major*, *$minor* and *$rev* are given by the installer. A symbolic link to the actual installation directory will be created in *$dynatrace_collector_linux_install_dir*/dynatrace. |
| *dynatrace_server_linux_installer_file_name* | dynatrace.jar         | The file name of the Dynatrace installer in the role's ```files``` directory. |
| *dynatrace_server_linux_service_names*       | [dynaTraceServer]     | The full installer installs the Dynatrace Server, Collector and Agents. However, by default only ```dynaTraceServer``` will run as a service. You can control which services shall be made available upon startup by specifying any of ```dynaTraceServer```, ```dynaTraceCollector``` or ```dynaTraceWebServerAgent``` in this list, as seen in the example below. |
| *dynatrace_server_fixpack_file_name*         | dynatrace-fixpack.dtf | The file name of the Dynatrace Fixpack in the role's ```files``` directory. |
| *dynatrace_server_license_file_name*         | dynatrace-license.dtf | The file name of the Dynatrace License in the role's ```files``` directory. |
| *dynatrace_server_role_name*                 | Dynatrace-Server      | The actual name of this role in an [Ansible Playbook's](http://docs.ansible.com/playbooks.html) ```roles``` directory. |

## Environment Variables

As defined in ````vars/main.yml```:

| Name                                        | Default    | Description                                                                               |
|---------------------------------------------|------------|-------------------------------------------------------------------------------------------|
| *DYNATRACE_COLLECTOR_HOST*                  | localhost  | The location of the collector the Web Server Agent service (if enabled) shall connect to. |

## Example Playbook

	- hosts: all
	  roles:
	    - { role: Dynatrace-Server, dynatrace_server_linux_service_names: [dynaTraceServer, dynaTraceCollector, dynaTraceWebServerAgent] }

## Additional Resources

- [Slide Deck: Automated Deployments](http://slideshare.net/MartinEtmajer/automated-deployments-slide-share)
- [Slide Deck: Introduction to Automated Deployments with Ansible](http://www.slideshare.net/MartinEtmajer/introduction-to-automated-deployments-with-ansible)

## Questions?

Feel free to post your questions on the Dynatrace Community's [Continuous Delivery Forum](https://community.dynatrace.com/community/pages/viewpage.action?pageId=46628921).

## License

Licensed under the MIT License. See the LICENSE file for details.
