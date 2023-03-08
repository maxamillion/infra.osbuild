# Osbuild Composer Ansible Collection

[![GitHub Super-Linter](https://github.com/redhat-cop/infra.osbuild/workflows/Lint%20Code%20Base/badge.svg)](https://github.com/marketplace/actions/super-linter)

[Ansible Collection](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html)
for management of [osbuild composer](https://www.osbuild.org/documentation/#composer) 
to build [rpm-ostree](https://rpm-ostree.readthedocs.io/en/latest/) based images for Fedora,
Red Hat Enterprise Linux, and Centos Stream. This collection has roles to build an osbuild server,
an apache httpd server to host images, and a role to build installer images and rpm-ostree updates.

## Installing

To install this collection and its dependencies, you will need to use the [Ansible](https://github.com/ansible/ansible) `ansible-galaxy` command:

```shell
ansible-galaxy collection install git+https://github.com/redhat-cop/infra.osbuild
```

## How to use

You will need a RHEL, Centos Stream, or Fedora system that you can connect to
remotely via `ssh`, and a playbook to call the desired roles to result in the
desired functionality. Each role has it's own documentation specific to its
provided automation.

To use the example playbooks provided in this repository, please create an
inventory file that only has the osbuild build server(s) listed in them as these
example playbooks use the `all` Ansible [group](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#inventory-basics-formats-hosts-and-groups)
as the [host patten](https://docs.ansible.com/ansible/latest/inventory_guide/intro_patterns.html).

### Configure Osbuild Builder (this will also host the images)

```shell
ansible-playbook playbooks/osbuild_setup_server.yml
```

### Build an Image

```shell
ansible-playbook playbooks/osbuild_builder.yml
```
You can specify what kind of build you prefer with the variable buidler_compose_type.

Current supported and tested build types are:

- edge-commit
- edge-container
- edge-installer
- edge-simplified-installer
- edge-raw-image
- iot-commit (fedora only)
- iot-container (fedora only)
- iot-installer (fedora only)
- iot-raw-image (fedora only)

Example:

```shell
ansible-playbook playbooks/osbuild_builder.yml -e builder_compose_type=edge-installer
```


## Releases

Releases are based on git tags and follow [semantic versioning](https://semver.org/).

### Creating a release

1. Create a new branch in your fork called `release/X.Y.Z` (replacing the `X.Y.Z` with the appropriate version number).
2. Generate the Changelog from the snippets using [antsibull-changelog](https://github.com/ansible-community/antsibull-changelog), as prescribed in the [Ansible Developer docs](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections_changelogs.html).
3. Ensure the version `X.Y.Z` is changed in the `galaxy.yml` file to correctly represent the new release version.
4. Commit changes
5. Create a pull request
6. Once the pull request is merged, have one of the [repo maintainers](https://github.com/redhat-cop/infra.osbuild/graphs/contributors) tag the commit and push it to the parent repository.


