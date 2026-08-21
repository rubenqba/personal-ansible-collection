# Rubenqba Servers Collection

This repository contains the `rubenqba.servers` Ansible Collection.

<!--start requires_ansible-->
<!--end requires_ansible-->

## External requirements

Some modules and plugins require external libraries. Please check the
requirements for each plugin or module you use in the documentation to find out
which requirements are needed.

## Included content

<!--start collection content-->
<!--end collection content-->

## Using this collection

Install the collection as a dependency of an Ansible project. Create a
`collections/requirements.yml` file in the project directory:

```yaml
---
collections:
  - name: rubenqba.servers
  - name: community.docker
    version: ">=3.0.0"
```

Install the project dependencies from the project root:

```bash
ansible-galaxy collection install -r collections/requirements.yml
```

Use the role in a playbook with its fully qualified collection name:

```yaml
---
- name: Deploy Traefik
  hosts: docker
  become: true
  roles:
    - role: rubenqba.servers.traefik
```

The `community.docker` dependency is required by the Traefik role. If the
project uses other roles from this collection, add any additional collection
dependencies to the same requirements file.

To install the collection directly from Ansible Galaxy instead, run:

```bash
ansible-galaxy collection install rubenqba.servers
```

You can also install the collection directly from this Git repository. In a
Git-based requirement, use the repository URL as `name` (not the collection
name). To use a specific tag, set `type: git` and use the tag in `version`:

```yaml
collections:
  - name: https://github.com/rubenqba/personal-ansible-collection.git
    type: git
    version: v1.0.0
```

To use a branch, replace the tag with the branch name. For example, to use the
`main` branch:

```yaml
collections:
  - name: https://github.com/rubenqba/personal-ansible-collection.git
    type: git
    version: main
```

The collection name used in playbooks remains `rubenqba.servers`, as defined in
`galaxy.yml`. Install either Git-based configuration from the project root with:

```bash
ansible-galaxy collection install -r collections/requirements.yml
```

To pin a collection version, add a `version` field to its entry. For example:

```yaml
collections:
  - name: rubenqba.servers
    version: ">=1.0.0,<2.0.0"
```

To upgrade a directly installed collection, run:

```bash
ansible-galaxy collection install rubenqba.servers --upgrade
```

See
[Ansible Using Collections](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html)
for more details.

## Release notes

See the
[changelog](https://github.com/ansible-collections/rubenqba.servers/tree/main/CHANGELOG.rst).

## Roadmap

<!-- Optional. Include the roadmap for this collection, and the proposed release/versioning strategy so users can anticipate the upgrade/update cycle. -->

## More information

<!-- List out where the user can find additional information, such as working group meeting times, slack/matrix channels, or documentation for the product this collection automates. At a minimum, link to: -->

- [Ansible collection development forum](https://forum.ansible.com/c/project/collection-development/27)
- [Ansible User guide](https://docs.ansible.com/ansible/devel/user_guide/index.html)
- [Ansible Developer guide](https://docs.ansible.com/ansible/devel/dev_guide/index.html)
- [Ansible Collections Checklist](https://docs.ansible.com/ansible/devel/community/collection_contributors/collection_requirements.html)
- [Ansible Community code of conduct](https://docs.ansible.com/ansible/devel/community/code_of_conduct.html)
- [The Bullhorn (the Ansible Contributor newsletter)](https://docs.ansible.com/ansible/devel/community/communication.html#the-bullhorn)
- [News for Maintainers](https://forum.ansible.com/tag/news-for-maintainers)

## Licensing

GNU General Public License v3.0 or later.

See [LICENSE](https://www.gnu.org/licenses/gpl-3.0.txt) to see the full text.
