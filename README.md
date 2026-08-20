# windows_update

Ansible role for managing Windows updates in a controlled, task-based workflow.

This role provides reusable tasks that can be imported into a playbook to:

- check whether a reboot is pending
- search for missing updates
- download updates
- install updates
- detect whether an update requires a reboot after installation

## Requirements

- Ansible managed Windows hosts
- Windows Remote Management (WinRM) enabled and reachable
- Network access to Microsoft Update or a configured WSUS endpoint
- The `ansible.windows` collection installed in the control node

## Role Variables

The default update categories are:

```yaml
windows_update_category_names:
  - CriticalUpdates
  - SecurityUpdates
  - UpdateRollups
```

You can override this variable to match the update categories required by your environment:

```yaml
windows_update_category_names:
  - CriticalUpdates
  - SecurityUpdates
  - FeaturePacks
  - UpdateRollups
```

## Available Tasks

The role exposes task files that are intended to be imported with `import_role` or `include_role`:

- `is-reboot-required.yml`
- `search-updates.yml`
- `download-updates.yml`
- `install-updates.yml`

## Examples Playbook

This example shows the recommended pattern used in this repository:

```yaml
---
- name: Update Windows systems
  hosts: windows
  gather_facts: false

  roles:
    - tvmalmeida.windows_update
```

For a workflow where only download is required

```yaml
---
- name: Update Windows systems
  hosts: windows
  gather_facts: false

  tasks:
    - name: Check if reboot is required before updates
      ansible.builtin.import_role:
        name: tvmalmeida.windows_update
        tasks_from: is-reboot-required.yml

    - name: Search for updates
      ansible.builtin.import_role:
        name: tvmalmeida.windows_update
        tasks_from: search-updates.yml

    - name: Download updates
      ansible.builtin.import_role:
        name: tvmalmeida.windows_update
        tasks_from: download-updates.yml

```

A complete example is also available in [examples/ensure-windows-update.yml](examples/ensure-windows-update.yml).

## Dependencies

None.

## License

BSD

## Author Information

Developed by Thiago V. Melo Almeida.
