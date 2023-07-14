# ANSIBLE-PROJECT
This repository contains several Ansible Projects and hands-on lab on Configuration management and Infrastructure as code (Iac).

## Configuration Management
According to [Red Hat](https://www.redhat.com/en/topics/automation/what-is-configuration-management#:~:text=Configuration%20management%20is%20a%20process,in%20a%20desired%2C%20consistent%20state.&text=Managing%20IT%20system%20configurations%20involves,building%20and%20maintaining%20those%20systems.), Configuration management is a process for maintaining computer systems, servers, and software in a desired, consistent state. It’s a way to make sure that a system performs as it’s expected to as changes are made over time. 

Managing IT system configurations involves defining a system's desired state—like server configuration—then building and maintaining those systems. Closely related to configuration assessments and drift analyses, configuration management uses both to identify systems to update, reconfigure, or patch.

### Ansible Playbook
An Ansible® Playbook is a blueprint of automation tasks—which are complex IT actions executed with limited or no human involvement. Ansible Playbooks are executed on a set, group, or classification of hosts, which together make up an Ansible inventory.

Ansible Playbooks are essentially frameworks, which are prewritten code developers can use ad-hoc or as starting template. Ansible Playbooks are regularly used to automate IT infrastructure (such as operating systems and Kubernetes platforms), networks, security systems, and developer personas (such as Git).

Ansible Playbooks help IT staff program applications, services, server nodes, or other devices without the manual overhead of creating everything from scratch. And Ansible Playbooks—as well as the conditions, variables, and tasks within them—can be saved, shared, or reused indefinitely.

### How do Ansible Playbooks work?

Ansible modules execute tasks. One or more Ansible tasks can be combined to make a play. Two or more plays can be combined to create an Ansible Playbook. Ansible Playbooks are lists of tasks that automatically execute against hosts. Groups of hosts form your Ansible inventory.

Each module within an Ansible Playbook performs a specific task. Each module contains metadata that determines when and where a task is executed, as well as which user executes it. There are thousands of other Ansible modules that perform all kinds of IT tasks, such as:
* User management
* Networking
* Cloud Management
* Security
* Configuration management
* Communication

### How do I use Ansible Playbooks?

Ansible uses the YAML syntax. Depending on whom you ask, YAML stands for yet another markup language or YAML ain’t markup language (a recursive acronym). There are also 2 different, but perfectly acceptable, YAML file extensions: .yaml or .yml.

There are 2 ways of using Ansible Playbooks: From the command line interface (CLI) or using the [Red Hat Ansible Automation Platform’s push-button deployments](https://www.redhat.com/en/technologies/management/ansible2).

Reference: [Redhat documentation](https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook)

Testing Jenkins CI
Testing Jenkins CI

Check Jenkins CI

Making changes
