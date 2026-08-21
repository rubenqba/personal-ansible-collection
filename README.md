# Shared Ansible Collection

Reusable Ansible roles, plugins, and modules shared across multiple projects.

## Development

Open this repository in the included Dev Container, then install dependencies:

```bash
pip install -r requirements.txt
ansible-galaxy collection build
ansible-lint
```

Replace `your_namespace` in `galaxy.yml` and in usage examples with the namespace
used when publishing the collection.

## Layout

- `roles/`: reusable roles
- `plugins/`: collection plugins
- `tests/`: automated tests
- `docs/`: additional documentation
