# Dynatrace-Server-Ansible

An [Ansible](http://www.ansible.com) role for automated deployments of the [Dynatrace](http://bit.ly/dttrial) Server.

## Download

The role is available via:

- [Ansible Galaxy](https://galaxy.ansible.com/list#/roles/2623)
- [GitHub](https://github.com/Dynatrace/Dynatrace-Server-Ansible)

## Requirements

Download the Dynatrace (full) installer from [downloads.dynatrace.com](downloads.dynatrace.com) and place the artifact as ```dynatrace.jar``` in the role's ```files/linux``` directory from where it will be picked up during the automated installation. Alternatively, you can make the Dynatrace installer available at an HTTP, HTTPS or FTP resource and point the installation script to the right location, see below.

## Optional

- Download a Dynatrace Fixpack from [downloads.dynatrace.com](downloads.dynatrace.com) and place the artifact as ```dynatrace-fixpack.dtf``` in the role's ```files``` directory. Alternatively, you can make the Dynatrace Fixpack available at an HTTP, HTTPS or FTP resource and point the installation script to the right location, see below.
- Place the Dynatrace License as ```dynatrace-license.key``` in the role's ```files``` directory. Alternatively, you can make the Dynatrace License available at an HTTP, HTTPS or FTP resource and point the installation script to the right location, see below. **You can obtain a free trial license for Dynatrace from [bit.ly/dttrial](http://bit.ly/dttrial).**

## Role Variables

As defined in ```defaults/main.yml```:

| Name                                          | Default                                          | Description |
|-----------------------------------------------|--------------------------------------------------|-------------|
| *dynatrace_server_linux_install_dir*          | /opt                                             | The Dynatrace Server will be installed into the directory *$dynatrace_server_linux_install_dir*/dynatrace-*$major*-*$minor*-*$rev*, where *$major*, *$minor* and *$rev* are given by the installer. A symbolic link to the actual installation directory will be created in *$dynatrace_server_linux_install_dir*/dynatrace. |
| *dynatrace_server_linux_installer_file_name*  | dynatrace.jar                                    | The file name of the Dynatrace installer in the role's ```files``` directory. |
| *dynatrace_server_linux_installer_file_url*   | http://downloads.dynatracesaas.com/6.2/dynatrace-linux-x64.jar | A HTTP, HTTPS or FTP URL to the Dynatrace installer in the form (http\|https\|ftp)://[user[:pass]]@host.domain[:port]/path. |
| *dynatrace_server_linux_service_names*        | [dynaTraceServer]                                | The full installer installs the Dynatrace Server, Collector and Agents. However, by default only ```dynaTraceServer``` will run as a service. You can control which services shall be made available upon startup by specifying any of ```dynaTraceServer```, ```dynaTraceCollector``` or ```dynaTraceWebServerAgent``` in this list, as seen in the example below. |
| *dynatrace_server_fixpack_file_name*          | dynatrace-fixpack.dtf                            | The file name of the Dynatrace Fixpack in the role's ```files``` directory. |
| *dynatrace_server_fixpack_file_url*           | http://localhost/dynatrace/dynatrace-fixpack.dtf | A HTTP, HTTPS or FTP URL to the Dynatrace Fixpack in the form (http\|https\|ftp)://[user[:pass]]@host.domain[:port]/path. |
| *dynatrace_server_license_file_name*          | dynatrace-license.key                            | The file name of the Dynatrace License in the role's ```files``` directory. |
| *dynatrace_server_license_file_url*           | http://localhost/dynatrace/dynatrace-license.key | A HTTP, HTTPS or FTP URL to the Dynatrace License in the form (http\|https\|ftp)://[user[:pass]]@host.domain[:port]/path. |
| *dynatrace_server_collector_port*             | 6698                                             | The port where the Server service (if enabled via *$dynatrace_server_linux_service_names*) shall listen for Collectors. Use either ```6698``` (non-SSL) or ```6699``` (SSL). |
| *dynatrace_server_collector_agent_port*       | 9998                                             | The port where the Collector service (if enabled via *$dynatrace_server_linux_service_names*) shall listen for Agents. |
| *dynatrace_server_collector_server_hostname*  | localhost                                        | The location of the Server the Collector service (if enabled via *$dynatrace_server_linux_service_names*) shall connect to. |
| *dynatrace_server_collector_server_port*      | 6698                                             | The port on the Server the Collector service (if enabled via *$dynatrace_server_linux_service_names*) shall connect to. Use either ```6698``` (non-SSL) or ```6699``` (SSL). |
| *dynatrace_server_wsagent_name*               | dtwsagent                                        | The name the Web Server Agent as it appears in Dynatrace (if enabled via *$dynatrace_server_linux_service_names*). |
| *dynatrace_server_wsagent_collector_hostname* | localhost                                        | The location of the Collector the Web Server Agent service (if enabled via *$dynatrace_server_linux_service_names*) shall connect to. |
| *dynatrace_server_wsagent_collector_port*     | 9998                                             | The port on the Collector the Web Server Agent service (if enabled via *$dynatrace_server_linux_service_names*) shall connect to. |
| *dynatrace_server_do_pwh_connection*          | no                                               | Whether a connection to an existing Performance Warehouse (database) shall be established, or not. **Note**: requires Dynatrace >= v6.2. |
| *dynatrace_server_pwh_connection_hostname*    | localhost                                        |             |
| *dynatrace_server_pwh_connection_port*        | 5432                                             |             |
| *dynatrace_server_pwh_connection_dbms*        | postgresql                                       | The DBMS type of the Performance Warehouse. Possible values are ```embedded``` (not suitable for production systems), ```db2```, ```oracle```, ```postgresql```, ```sqlazure```, ```sqlserver``` |
| *dynatrace_server_pwh_connection_database*    | dynatrace                                        |             |
| *dynatrace_server_pwh_connection_username*    | dynatrace                                        |             |
| *dynatrace_server_pwh_connection_password*    | dynatrace                                        |             |
| *dynatrace_server_role_name*                  | dynatrace.Dynatrace-Server                       | The actual name of this role in an [Ansible Playbook's](http://docs.ansible.com/playbooks.html) ```roles``` directory. |

## Example Playbook

	- hosts: all
	  roles:
	    - role: dynatrace.Dynatrace-Server
	      dynatrace_server_linux_service_names: [dynaTraceServer, dynaTraceCollector, dynaTraceWebServerAgent]
	      dynatrace_server_do_pwh_connection: yes

## Testing

We use [Test Kitchen](http://kitchen.ci) to automatically test our automated deployments with [Serverspec](http://serverspec.org):

1) Install Kitchen and its dependencies from within the project's directory:

```
gem install bundler
bundle install
```

2) Run tests

```
kitchen test
```

## Additional Resources

- [Blog: How to Automate Enterprise Application Monitoring with Ansible](http://apmblog.dynatrace.com/2015/03/04/how-to-automate-enterprise-application-monitoring-with-ansible/)
- [Blog: How to Automate Enterprise Application Monitoring with Ansible - Part II](http://apmblog.dynatrace.com/2015/04/23/how-to-automate-enterprise-application-monitoring-with-ansible-part-ii/)
- [Slide Deck: Automated Deployments](http://slideshare.net/MartinEtmajer/automated-deployments-slide-share)
- [Slide Deck: Automated Deployments (of Dynatrace) with Ansible](http://www.slideshare.net/MartinEtmajer/automated-deployments-with-ansible)
- [Slide Deck: Testing Ansible Roles with Test Kitchen, Serverspec and RSpec](http://www.slideshare.net/MartinEtmajer/testing-ansible-roles-with-test-kitchen-serverspec-and-rspec-48185017)
- [Tutorials: Automated Deployments (of Dynatrace) with Ansible](https://community.compuwareapm.com/community/display/LEARN/Tutorials+on+Automated+Deployments#TutorialsonAutomatedDeployments-ansible)

## Questions?

Feel free to post your questions on the Dynatrace Community's [Continuous Delivery Forum](https://community.dynatrace.com/community/pages/viewpage.action?pageId=46628921).

## License

Licensed under the MIT License. See the LICENSE file for details.
[![analytics](https://www.google-analytics.com/collect?v=1&t=pageview&_s=1&dl=https%3A%2F%2Fgithub.com%2FdynaTrace&dp=%2FDynatrace-Server-Ansible&dt=Dynatrace-Server-Ansible&_u=Dynatrace~&cid=github.com%2FdynaTrace&tid=UA-54510554-5&aip=1)]()
