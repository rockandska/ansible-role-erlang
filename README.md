ansible-role-erlang
=========

Ansible role to install Erlang/OTP versions provided by RabbitMQ.

**/!\ Not compatible with ansible > 2.8.7 < 2.8.13 due to a [bug](https://github.com/ansible/ansible/issues/70081)**

Requirements
------------

#### Debian / Ubuntu
- apt-transport-https
- gpg-agent
- ca-certificates

#### CentOS / RedHat
- gnupg2

Role Variables
--------------

Defaults variables are inside `defaults/main.yml`
```yaml
---
erlang_series: 22

erlang_rpm_repo_url: https://dl.bintray.com/rabbitmq-erlang/rpm/erlang
erlang_rpm_gpg_url: https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc
erlang_rpm_repo_tpl: etc/yum.repos.d/rabbitmq_erlang.repo.j2
erlang_series_rpm_version:

erlang_deb_repo_url: https://dl.bintray.com/rabbitmq-erlang/debian
erlang_deb_gpg_url: https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc
erlang_deb_repo_tpl: etc/apt/sources.list.d/rabbitmq_erlang.list.j2
erlang_deb_pinning_tpl: etc/apt/preferences.d/erlang.j2
erlang_series_deb_version:
```

#### Details:

- `erlang_series`

  - should be an integer (19,20,21,22,23 available at 06.14.2020)
  - don't forget to choose a serie compatible with the rabbitmq version that will be installed (see [rabbitmq documentation](https://www.rabbitmq.com/which-erlang.html))

- `erlang_rpm_repo_url`

  - repository base url used for the yum template

- `erlang_rpm_gpg_url`

  - gpg key to used for the yum template

- `erlang_rpm_repo_tpl`

  - path to the yum repository template
  - if you want to use your own template
    - add your template next to your playbook in `templates`
    - use a different path than the default one
    - keep the repository name as `rabbitmq_erlang`

- `erlang_series_rpm_version`
  - install a specific version of the `erlang_series` for the Centos / Redhat systems
  - examples:
    ```
    20.3.8.15-1.el7.centos
    20.3.8.17-1.el7.centos
    ```

- `erlang_deb_repo_url`

  - repository base url used for the apt template

- `erlang_deb_gpg_url`

  - gpg key to used for the apt template

- `erlang_deb_repo_tpl`

  - path to the apt repository template
  - if you want to use your own template
    - add your template next to your playbook in `templates`
    - use a different path than the default one

- `erlang_deb_pinning_tpl`

  - path to the apt pinning template
  - if you want to use your own template
    - add your template next to your playbook in `templates`
    - use a different path than the default one

- `erlang_series_deb_version`

  - install a specific version of the `erlang_series` for Debian systems
  - examples:
  	```
  	1:20.3.8.17-1
    1:20.3.8.16-1
    1:20.3.8.15-1
    ```

Example Playbook
----------------

```yaml
- hosts: rabbitmq
  roles:
     - { role: rockandska.erlang, erlang_series: 20 }
```

Local Testing
-------------

The preferred way of locally testing the role is to use Docker and [molecule](https://github.com/metacloud/molecule) (v2.x). You will have to install Docker on your system. See "Get started" for a Docker package suitable to for your system.
We are using tox to simplify process of testing on multiple ansible versions. To install tox execute:
```sh
$ sudo pip install tox
# or
$ pip install --user tox
```

To run tests on all ansible versions (WARNING: this can take some time)
```sh
$ tox
```
To run a custom molecule command on custom environment with only default test scenario:
```sh
$ tox -e py27-ansible25 -- molecule test -s default
```
For more information about molecule go to their [docs](http://molecule.readthedocs.io/en/latest/).

If you would like to run tests on remote docker host just specify `DOCKER_HOST` variable before running tox tests.

License
-------

BSD
