Ansible Role Openvpn server
========

[![Build Status](https://travis-ci.org/Turgon37/ansible-gitlab-runner.svg?branch=master)](https://travis-ci.org/Turgon37/ansible-gitlab-runner)
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![Ansible Role](https://img.shields.io/badge/ansible%20role-Turgon37.gitlab_runner-blue.svg)](https://galaxy.ansible.com/Turgon37/gitlab_runner/)

## Description
    
:grey_exclamation: Before using this role, please know that all my Ansible roles are fully written and accustomed to my IT infrastructure. So, even if they are as generic as possible they will not necessarily fill your needs, I advice you to carrefully analyse what they do and evaluate their capability to be installed securely on your servers.

This roles configures a Gitlab Runner instance.

## Requirements

Require Ansible >= 2.4

### Dependencies


## OS Family

This role is available for Debian

## Features

At this day the role can be used to :

  * install gitlab runner from official repository
  * runner(s) instance configuration with :
      * gitlab host registration and unregistration (using registration token)
      * config.toml templating
  * [local facts](#facts)

## Configuration

### Role

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below. To see default values please refer to this file.

| Name                          | Description                                                                                               |
| ----------------------------- | --------------------------------------------------------------------------------------------------------- |
| `openvpn__version`            | Choose the OpenVPN version to install (as available in repositories) Ex: 2.4.0-6+deb9u2 or 2.4.6-stretch0 |

## Facts

By default the local fact are installed and expose the following variables :

* ```ansible_local.gitlab-runner.version_full```
* ```ansible_local.gitlab-runner.version_major```


## Example

### Playbook

Use it in a playbook as follows:

```yaml
- hosts: all
  roles:
    - turgon37.gitlab_runner
```

### Inventory

```
gitlab_runner__coordinator_url: https://gitlab.www.custom.domain.net/
gitlab_runner__registration_token: XXXXXX
gitlab_runner__instances_host:
  - name: docker01
    executor: docker
    tags:
      - docker
      - x86
      - x86-docker
    environment:
      DOCKER_HOST: tcp://username__docker-dind-pgis:2375
      HTTP_PROXY: '{{ http_proxy }}'
      HTTPS_PROXY: '{{ https_proxy }}'
      http_proxy: '{{ http_proxy }}'
      https_proxy: '{{ https_proxy }}'
    docker_options:
      allowed_images:
        - docker:latest
      allowed_services:
        - username/docker-dind-pgis:latest
      disable_cache: false
      image: docker:latest
      privileged: true
      pull_policy: always
      volumes:
        - /data/runner-cache:/cache:rw

  - name: shell01
    executor: shell
    tags:
      - shell
      - x86
      - x86-shell
```