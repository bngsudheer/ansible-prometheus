<p><img src="https://cdn.worldvectorlogo.com/logos/prometheus.svg" alt="prometheus logo" title="prometheus" align="right" height="60" /></p>

Ansible Role: prometheus
========================

[![Build Status](https://travis-ci.org/SoInteractive/ansible-prometheus.svg?branch=master)](https://travis-ci.org/SoInteractive/ansible-prometheus) [![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT) [![Ansible Role](https://img.shields.io/badge/ansible%20role-SoInteractive.prometheus-blue.svg)](https://galaxy.ansible.com/SoInteractive/prometheus/) [![GitHub tag](https://img.shields.io/github/tag/sointeractive/ansible-prometheus.svg)](https://github.com/SoInteractive/ansible-prometheus/tags) [![Twitter URL](https://img.shields.io/twitter/follow/sointeractive.svg?style=social&label=Follow%20%40SoInteractive)](https://twitter.com/sointeractive)

Deploy Prometheus monitoring system

More info
---------

An Ansible role that installs Prometheus Monitoring server.

Requirements
------------

None

Role Variables
--------------
Below is a list of default role variables that can be overridden. Have a look at [defaults/main.yml](defaults/main.yml) for the definitive list of variables.

* prometheus_version: 1.8.0
* prometheus_config_dir: /etc/prometheus
* prometheus_db_dir: /var/lib/prometheus
* prometheus_root_dir: /opt/prometheus
* prometheus_interface: "{{ ansible_default_ipv4.interface }}"
* prometheus_web_listen_address: "0.0.0.0:9090"
* prometheus_web_external_url: 'http://localhost:9090'
* prometheus_alertmanager_url: 'localhost:9093'
* prometheus_global_scrape_interval: 15s
* prometheus_global_scrape_timeout: 10s
* prometheus_global_evaluation_interval: 15s
* prometheus_config_flags_extra: {}
* prometheus_external_labels:
  environment: "{{ ansible_domain }}"
* prometheus_rules_files: []
* prometheus_targets_dns: []
* prometheus_targets: []
* prometheus_custom_config_path: ''

Use the variable *prometheus_custom_config_path* to provide your own *prometheus.yml* configuration. This is useful when you want to discard the default configuration provided by this Ansible role. The role first looks for the file *prometheus.yml.j2* file in the provided path. If not found, the role uses the default configuration.

Example usage
-------------

```yaml
---
- hosts: all
  gather_facts: yes

- hosts: all
  become: yes
  become_user: root
  roles:
  - role:
      - SoInteractive.prometheus
  vars:
    prometheus_external_labels:
      monitoring: a
    prometheus_targets:
      - targets:
        - host_ip_or_domain:port
        labels:
          env: environment_name_usually_domain_name
          job: node
```

Defining alerting rules files
-----------------------------

Put the rules files to rules folder

Alerting rules are defined in the following syntax:
```yaml
ALERT <alert name>
  IF <expression>
  [ FOR <duration> ]
  [ LABELS <label set> ]
  [ ANNOTATIONS <label set> ]
```
Example Alertmanager rules files:
```yaml
# Alert for any instance that is unreachable for >5 minutes.
ALERT InstanceDown
  IF up == 0
  FOR 5m
  LABELS { severity = "page" }
  ANNOTATIONS {
    summary = "Instance {{ $labels.instance }} down",
    description = "{{ $labels.instance }} of job {{ $labels.job }} has been down for more than 5 minutes.",
  }
```
